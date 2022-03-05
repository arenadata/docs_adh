Настройка прозрачного шифрования Hadoop
=======================================

Настройка прозрачного шифрования **HDFS** осуществляется при помощи системы управления ключами (**KMS**).

Hadoop **KMS** представляет собой сервер управляющий криптографическими ключами на основе **KeyProvider API**. Он предоставляет компоненты клиента и сервера, которые связываются по **HTTP** при помощи **REST API**. Клиент, как применение **KeyProvider** взаимодействует с **KMS** при помощи **KMS HTTP REST API**. **KMS** и его клиент имеют встроенную безопасность и поддерживают авторизацию **HTTP SPNEGO Kerberos** и защищенный протокол **HTTPS**.

**KMS** является веб-приложением **Java Jetty**.

Полную информацию о настройке **KMS** можно найти в `документации Hadoop <https://hadoop.apache.org/docs/current/hadoop-kms/index.html>`_.

Настройка клиента KMS
---------------------

**KeyProvider** клиента **KMS** использует схему **kms**, а вложенный URL должен быть адресом **KMS**. Например, для KMS запущенного на *http://localhost:9600/kms*, **KeyProvider** URI будет *kms://http@localhost:9600/kms*. А для **KMS** запущенного на *https://localhost:9600/kms*, **KeyProvider** URI будет *kms://https@localhost:9600/kms*.

Пример конфигурации **HDFS NameNode** в качестве клиента **KMS** в *core-site.xml*:

::

  <property>
    <name>hadoop.security.key.provider.path</name>
    <value>kms://http@localhost:9600/kms</value>
    <description>
      KeyProvider для взаимодействия с ключами шифрования, применяемый при чтении и записи в зону шифрования
    </description>
  </property>

Команды KMS
-----------

Запуск/остановка KMS
^^^^^^^^^^^^^^^^^^^^

Для запуска/остановки **KMS** используется ``hadoop --daemon start|stop kms``. Пример:

::

  hadoop-3.3.1 $ hadoop --daemon start kms

.. Note:: Скрипт *kms.sh* больше не используется. Теперь это только оболочка для **hadoop kms**.

Конфигурация KMS
^^^^^^^^^^^^^^^^

Свойства **KeyProvider** изменяются в конфигурационном файле *etc/hadoop/kms-site.xml*:

::

    <property>
       <name>hadoop.kms.key.provider.uri</name>
       <value>jceks://file@/${user.home}/kms.keystore</value>
    </property>

    <property>
      <name>hadoop.security.keystore.java-keystore-provider.password-file</name>
      <value>kms.keystore.password</value>
    </property>
  
Файл пароля ищется в папке конфигурации **Hadoop** через классовый путь.

.. Note:: Для применения изменений необходимо перезапустить **KMS**.

.. Note:: Сервер **KMS** может выбирать любое применение **KeyProvider** в качестве запасного провайдера. Приведенный пример использует **JavaKeyStoreProvider**, который может быть использован для экспериментов и никогда для коммерческого использования. Для подробного использования и особенностей **JavaKeyStoreProvider**, см. `документацию CredentialProvider API Guide <https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/CredentialProviderAPI.html#Keystore_Passwords>`_.

Конфигурация KMS HTTP
^^^^^^^^^^^^^^^^^^^^^

**KMS** предварительно настраивает HTTP порт на 9600.

**KMS** поддерживает следующие `параметры конфигурации <https://hadoop.apache.org/docs/current/hadoop-kms/kms-default.html>`_ HTTP в *etc/hadoop/kms-site.xml*.

.. Note:: Для применения изменений необходимо перезапустить **KMS**.

Кэш KMS
^^^^^^^

KMS имеет два способа кэширования: **CachingKeyProvider** для кэширования ключей шифрования, и **KeyProvider* для кэширования **EEK**.

CachingKeyProvider
..................

**KMS** временно кэширует ключи шифрования, чтобы избежать чрезмерного количества вызовов **KeyProvider**.

Этот кэш по умолчанию включен (может быть отключено через настройку *hadoop.kms.cache.enable* установкой его в *false*).

Кэш используется только с тремя методами: *getCurrentKey()*, *getKeyVersion()* и *getMetadata()*.

Для метода *getCurrentKey()* кэшированные данные удерживаются максимум 30000 миллисекунд, вне зависимости от частоты использования ключа (чтобы избежать оценки устаревших ключей как действующих).

Для методов *getKeyVersion()* и *getMetadata()*кэшированные данные удерживаются с таймером неактивности в 600000 миллисекунд (10 минут).

Кэш становится недействительным, когда ключ удаляется при помощи *deleteKey()*, или когда запускается *invalidateCache()*.

Эти конфигурации могут быть изменены в следующих настройках файла конфигурации *etc/hadoop/kms-site.xml*:

::

   <property>
     <name>hadoop.kms.cache.enable</name>
     <value>true</value>
   </property>

   <property>
     <name>hadoop.kms.cache.timeout.ms</name>
     <value>600000</value>
   </property>

   <property>
     <name>hadoop.kms.current.key.cache.timeout.ms</name>
     <value>30000</value>
   </property>

KeyProvider
...........

Архитектурно, и со стороны сервера (**KMS**), и со стороны клиента (**NameNode**) присутствует кэш для **EEK**. Следующие параметры можно настраивать в кэше:

* Размер кэша. Максимально число **EEK** которое может кэшироваться с каждым именем ключа.

* Цифровой водяной знак (ЦВЗ) для кэша. Для каждого имени ключа, если после запуска число кэшированных **EEK** меньше (размер * ЦВЗ), кэш под этим именем ключа будет заполнен несинхронно. Для каждого имени ключа может быть запущен только 1 поток для несинхронного заполнения.

* Максимальное число несинхронных потоков по всем именам ключей, которые могут заполнять запросы в кэше.

* Время истечения кэша в миллисекундах. Применяется внутренний кэш **Guava**. Метод истечения `expireAfterAccess <https://code.google.com/p/guava-libraries/wiki/CachesExplained>`_.

Учтите, что из-за механизма несинхронного заполнения, возможно, что после запроса *rollNewVersion()*, запрашивающий будет получать старые **EEK**. В худшем случае, запрашивающий может дойти до (размер кэша сервера + размер кэша клиента) числа старых **EEK**, или пока оба кэша не истекут. 

Далее представлены конфигурации и их значения по умолчанию.

Со стороны сервера можно изменять следующие свойства в файле конфигурации *etc/hadoop/kms-site.xml*:

::

   <property>
     <name>hadoop.security.kms.encrypted.key.cache.size</name>
     <value>500</value>
   </property>

   <property>
     <name>hadoop.security.kms.encrypted.key.cache.low.watermark</name>
     <value>0.3</value>
   </property>

   <property>
     <name>hadoop.security.kms.encrypted.key.cache.num.fill.threads</name>
     <value>2</value>
   </property>

   <property>
     <name>hadoop.security.kms.encrypted.key.cache.expiry</name>
     <value>43200000</value>
   </property>

Со стороны клиента можно изменять следующие свойства в файле конфигурации *etc/hadoop/core-site.xml*:

::

   <property>
     <name>hadoop.security.kms.client.encrypted.key.cache.size</name>
     <value>500</value>
   </property>

   <property>
     <name>hadoop.security.kms.client.encrypted.key.cache.low-watermark</name>
     <value>0.3</value>
   </property>

   <property>
     <name>hadoop.security.kms.client.encrypted.key.cache.num.refill.threads</name>
     <value>2</value>
   </property>

   <property>
     <name>hadoop.security.kms.client.encrypted.key.cache.expiry</name>
     <value>43200000</value>
   </property>

Агрегированные логи аудита KMS
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Логи аудита агрегируются для API доступа к операциям *GET_KEY_VERSION*, *GET_CURRENT_KEY*, *DECRYPT_EEK*, *GENERATE_EEK*, *REENCRYPT_EEK*.

Данные сгруппированы по (пользователь, ключ, операция) комбинированному ключу для настраиваемого интервала агрегации, после которого число доступов к конкретному эндпоинту пользователем для заданного ключа удаляется из лога аудита.

Интервал агрегации настраивается через следующий параметр:

::

  <property>
    <name>hadoop.kms.aggregation.delay.ms</name>
    <value>10000</value>
  </property>

Настройка безопасности KMS
^^^^^^^^^^^^^^^^^^^^^^^^^^

Включение авторизации HTTP SPNEGO в Kerberos
............................................

Для включения авторизации **HTTP SPNEGO**, необходимо настроить файл конфигурации **Kerberos** *etc/krb5.conf* на вашем сервере **KDC**.

Необходимо создать *service principal* и его вкладку для **KMS** и сделать его *HTTP*.

Настройка безопасности осуществляется изменением следующих параметров в конфигурационном файле *etc/hadoop/kms-site.xml*:

::

   <property>
     <name>hadoop.kms.authentication.type</name>
     <value>kerberos</value>
   </property>

   <property>
     <name>hadoop.kms.authentication.kerberos.keytab</name>
     <value>${user.home}/kms.keytab</value>
   </property>

   <property>
     <name>hadoop.kms.authentication.kerberos.principal</name>
     <value>HTTP/localhost</value>
   </property>

   <property>
     <name>hadoop.kms.authentication.kerberos.name.rules</name>
     <value>DEFAULT</value>
   </property>

.. Note:: Для применения изменений необходимо перезапустить **KMS**.

Настройка прокси-юзера KMS
..........................

Каждый прокси-юзер должен быть настроен в конфигурационном файле *etc/hadoop/kms-site.xml* со следующими свойствами:

::

  <property>
    <name>hadoop.kms.proxyuser.#USER#.users</name>
    <value>*</value>
  </property>

  <property>
    <name>hadoop.kms.proxyuser.#USER#.groups</name>
    <value>*</value>
  </property>

  <property>
    <name>hadoop.kms.proxyuser.#USER#.hosts</name>
    <value>*</value>
  </property>

*#USER#* - имя пользователя, которое надо настроить для прокси-юзера.

Свойство *users* определяет пользователей которого можно имитировать.

Свойство *groups* определяет группу, к которой должен принадлежать пользователь, которого имитируют.

По крайней мере одно из свойств *users* или *groups* должно быть задано. Если заданы оба - настроенный прокси-юзер сможет имитировать как пользователя из списка *users*, так и любого пользователя, принадлежащего к одной из групп в списке *groups*.

Свойство *hosts* определяет с каких хостов прокси-юзер может запрашивать вход.

Если *users*, *groups* или *hosts* выставлен как * - в этой области отсутствуют ограничения для прокси-юзера.

KMS по HTTPS (SSL)
..................

**SSL** включается в конфигурационном файле *etc/hadoop/kms-site.xml*:

::

  <property>
    <name>hadoop.kms.ssl.enabled</name>
    <value>true</value>
    <description>
      Включен ли SSL. По умолчанию false, т.е. отключен.
    </description>
  </property>

Настройка осуществляется изменением следующих параметров в конфигурационном файле *etc/hadoop/ssl-server.xml*:

::

  <property>
    <name>ssl.server.keystore.location</name>
    <value>${user.home}/.keystore</value>
    <description>Используемый Keystore. Должен быть указан.</description>
  </property>

  <property>
    <name>ssl.server.keystore.password</name>
    <value></value>
    <description>Должен быть указан.</description>
  </property>

  <property>
    <name>ssl.server.keystore.keypassword</name>
    <value></value>
    <description>Должен быть указан.</description>
  </property>

Пароль **SSL** может быть защищен провайдером данных. См. документацию `Credential Provider API <https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/CredentialProviderAPI.html>`_.

Необходимо создать сертификат **SSL** для **KMS**. Для этого в качестве пользователя Unix *kms* используйте команду в инструменте Java:

::

  $ keytool -genkey -alias jetty -keyalg RSA

В процессе выполнения вам будет задан ряд вопросов, которые надо заполнить. По завершению будет создан файл **keystore** с именем *.keystore*, размещенный в начальной папке пользователя.

Пароль, который вы используете для “keystore password” должен совпадать со значением свойства *ssl.server.keystore.password* в конфигурационном файле *ssl-server.xml*.

В ответ на “What is your first and last name?” необходимо ввести имя хоста машины, где будет работать **KMS**.

.. Note:: Для применения изменений необходимо перезапустить **KMS**.

.. Note:: Некоторые старые клиенты **SSL** могут использовать слабое шифрование, не поддерживаемое сервером **KMS**. Рекомендуется обновлять клиент **SSL**.

Списки управления доступом
..........................

**KMS** поддерживает списки управления доступом (ACL) для более тонкой настройки доступов.

В **KMS** существует два уровня **ACL**: **KMS ACL** и ACL ключа**. **KMS ACL** контролируют доступ на уровне операций **KMS** и действует раньше **ACL ключа**. В частности, только если разрешение выдано на уровне **KMS ACL**, будет проводиться проверка разрешения на уровне **ACL ключа**.

Далее приведены конфигурации и применение обоих уровней.

KMS ACL
"""""""

Конфигурация **KMS ACL** настраивается в конфигурационном файле *etc/hadoop/kms-acls.xml*. Этот файл мгновенно перезагружается при внесении изменений.

Настройки KMS поддерживают как тонкий контроль доступа, так и черный список для операций.

Пользователь, входящий в KMS, сначала проверяется по спискам управления доступом на запрашиваемую операцию, после чего по черному списку на нее же.

::

  <configuration>
    <property>
      <name>hadoop.kms.acl.CREATE</name>
      <value>*</value>
      <description>
            ACL для операций create-key.
            Если пользователя нет в GET ACL, данные ключа не возвращаются в ответе.
      </description>
    </property>

    <property>
      <name>hadoop.kms.blacklist.CREATE</name>
      <value>hdfs,foo</value>
      <description>
            Черный список для операций create-key.
            Если пользователя в черном списке, данные ключа не возвращаются в ответе.
      </description>
    </property>

    <property>
      <name>hadoop.kms.acl.DELETE</name>
      <value>*</value>
      <description>
            ACL для операций delete-key.
      </description>
    </property>

    <property>
      <name>hadoop.kms.blacklist.DELETE</name>
      <value>hdfs,foo</value>
      <description>
            Черный список для операций delete-key.
      </description>
    </property>

    <property>
      <name>hadoop.kms.acl.ROLLOVER</name>
      <value>*</value>
      <description>
            ACL для операций rollover-key.
            Если пользователя в черном списке, данные ключа не возвращаются в ответе.
      </description>
    </property>

    <property>
      <name>hadoop.kms.blacklist.ROLLOVER</name>
      <value>hdfs,foo</value>
      <description>
            Черный список для операций rollover-key.
      </description>
    </property>

    <property>
      <name>hadoop.kms.acl.GET</name>
      <value>*</value>
      <description>
           ACL для операций get-key-version и get-current-key.
      </description>
    </property>

    <property>
      <name>hadoop.kms.blacklist.GET</name>
      <value>hdfs,foo</value>
      <description>
            Черный список для операций get-key-version и get-current-key.
      </description>
    </property>

    <property>
      <name>hadoop.kms.acl.GET_KEYS</name>
      <value>*</value>
      <description>
           ACL для операций get-keys.
      </description>
    </property>

    <property>
      <name>hadoop.kms.blacklist.GET_KEYS</name>
      <value>hdfs,foo</value>
      <description>
            Черный список для операций get-keys.
      </description>
    </property>

    <property>
      <name>hadoop.kms.acl.GET_METADATA</name>
      <value>*</value>
      <description>
          ACL для операций get-key-metadata и get-keys-metadata.
      </description>
    </property>

    <property>
      <name>hadoop.kms.blacklist.GET_METADATA</name>
      <value>hdfs,foo</value>
      <description>
           Черный список для операций get-key-metadata и get-keys-metadata.
      </description>
    </property>

    <property>
      <name>hadoop.kms.acl.SET_KEY_MATERIAL</name>
      <value>*</value>
      <description>
              Дополняющий ACL для операций CREATE и ROLLOVER, позволяющих клиенту вносить данные ключа при создании и перезаписи.
      </description>
    </property>

    <property>
      <name>hadoop.kms.blacklist.SET_KEY_MATERIAL</name>
      <value>hdfs,foo</value>
      <description>
              Дополняющий черный список для операций CREATE and ROLLOVER, позволяющих клиенту вносить данные ключа при создании и перезаписи.
      </description>
    </property>

    <property>
      <name>hadoop.kms.acl.GENERATE_EEK</name>
      <value>*</value>
      <description>
            ACL для операций generateEncryptedKey и CryptoExtension.
      </description>
    </property>

    <property>
      <name>hadoop.kms.blacklist.GENERATE_EEK</name>
      <value>hdfs,foo</value>
      <description>
            Черный список для операций generateEncryptedKey и CryptoExtension.
      </description>
    </property>

    <property>
      <name>hadoop.kms.acl.DECRYPT_EEK</name>
      <value>*</value>
      <description>
            ACL для операций расшифровки EncryptedKey и CryptoExtension.
      </description>
    </property>

    <property>
      <name>hadoop.kms.blacklist.DECRYPT_EEK</name>
      <value>hdfs,foo</value>
      <description>
            Черный список для операций расшифровки EncryptedKey и CryptoExtension.
      </description>
    </property>
  </configuration>

ACL ключа
"""""""""

**KMS** поддерживает контроль доступа для всех операций не-чтения на уровне ключа. Операции доступа ключа делятся на следующие классы:

* **MANAGEMENT** - *createKey*, *deleteKey*, *rolloverNewVersion*.

* **GENERATE_EEK** - *generateEncryptedKey*, *reencryptEncryptedKey*, *reencryptEncryptedKeys*, *warmUpEncryptedKeys*.

* **DECRYPT_EEK** - *decryptEncryptedKey*.

* **READ** - *getKeyVersion*, *getKeyVersions*, *getMetadata*, *getKeysMetadata*, *getCurrentKey*.

* **ALL** - все вышеперечисленные.

Они могут быть определены в конфигурационном файле *etc/hadoop/kms-acls.xml*.

Для всех ключей, которым не был задан доступ ключа, можно установить контроль доступа по-умолчанию на класс операций.

Также можно настроить “белый список” для ACL ключа на класс операций. Белый список предоставляет доступ к ключу в дополнение к установленным ACL ключа или ACL ключа по умолчанию. Если никакого ACL ключа не было задано, пользователь получит доступ, если они присутствуют в ACL ключа по-умолчанию или белом списке ACL ключа. Если ACL ключа задан, пользователь получит доступ, если он присутствуют в установленном ACL ключа или белом списке ACL ключа.

Если у операции не настроено ACL для конкретного ключа И отсутствует ACL по-умолчанию И отсутствует белый список, в доступе будет отказано.

.. Note:: ACL ключа по-умолчанию и белый список не поддерживают класс операций *ALL*.

::

    <property>
      <name>key.acl.testKey1.MANAGEMENT</name>
      <value>*</value>
      <description>
        ACL для операций create-key, deleteKey и rolloverNewVersion.
      </description>
    </property>

    <property>
      <name>key.acl.testKey2.GENERATE_EEK</name>
      <value>*</value>
      <description>
        ACL для операций generateEncryptedKey.
      </description>
    </property>

    <property>
      <name>key.acl.testKey3.DECRYPT_EEK</name>
      <value>admink3</value>
      <description>
        ACL для операций decryptEncryptedKey.
      </description>
    </property>

    <property>
      <name>key.acl.testKey4.READ</name>
      <value>*</value>
      <description>
        ACL для операций getKeyVersion, getKeyVersions, getMetadata, getKeysMetadata, getCurrentKey.
      </description>
    </property>

    <property>
      <name>key.acl.testKey5.ALL</name>
      <value>*</value>
      <description>
        ACL для операций ALL.
      </description>
    </property>

    <property>
      <name>whitelist.key.acl.MANAGEMENT</name>
      <value>admin1</value>
      <description>
        Белый список ACL для операций MANAGEMENT для всех ключей.
      </description>
    </property>

    <!--
    'testKey3' key ACL is defined. Since a 'whitelist'
    key is also defined for DECRYPT_EEK, in addition to
    admink3, admin1 can also perform DECRYPT_EEK operations
    on 'testKey3'
  -->
    <property>
      <name>whitelist.key.acl.DECRYPT_EEK</name>
      <value>admin1</value>
      <description>
        Белый список ACL для операций DECRYPT_EEK для всех ключей.
      </description>
    </property>

    <property>
      <name>default.key.acl.MANAGEMENT</name>
      <value>user1,user2</value>
      <description>
        ACL по-умолчанию для операций MANAGEMENT operations для всех явно не заданных ключей.
      </description>
    </property>

    <property>
      <name>default.key.acl.GENERATE_EEK</name>
      <value>user1,user2</value>
      <description>
        ACL по-умолчанию для операций GENERATE_EEK для всех явно не заданных ключей.
      </description>
    </property>

    <property>
      <name>default.key.acl.DECRYPT_EEK</name>
      <value>user1,user2</value>
      <description>
        ACL по-умолчанию для операций DECRYPT_EEK для всех явно не заданных ключей.
      </description>
    </property>

    <property>
      <name>default.key.acl.READ</name>
      <value>user1,user2</value>
      <description>
        ACL по-умолчанию для операций READ для всех явно не заданных ключей.
      </description>
    </property>

Настройка токенов делегирования KMS
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**KMS** поддерживает токены делегирования для авторизации в **key providers** из процессов без данных **Kerberos**.

Авторизация токена делегирования KMS расширяет авторизацию Hadoop по-умолчанию. Как и авторизация Hadoop, токены делегирования KMS не должны быть переданы или обновлены при помощи авторизации токена делегирования. См. `документацию Hadoop Auth <https://hadoop.apache.org/docs/current/hadoop-auth/index.html>`_.

Также, токен делегирования KMS настраивается при помощи следующих параметров:

::

  <property>
    <name>hadoop.kms.authentication.delegation-token.update-interval.sec</name>
    <value>86400</value>
    <description>
      Как часто ротируется мастер-ключ, в секундах. Значение по умолчанию 1 день.
    </description>
  </property>

  <property>
    <name>hadoop.kms.authentication.delegation-token.max-lifetime.sec</name>
    <value>604800</value>
    <description>
      Предельное время существования токена делегирования, в секундах. Значение по умолчанию 7 дней.
    </description>
  </property>

  <property>
    <name>hadoop.kms.authentication.delegation-token.renew-interval.sec</name>
    <value>86400</value>
    <description>
      Интервал обновления токена делегирования, в секундах. Значение по умолчанию 1 день.
    </description>
  </property>

  <property>
    <name>hadoop.kms.authentication.delegation-token.removal-scan-interval.sec</name>
    <value>3600</value>
    <description>
      Интервал сканирования для удаления устаревших токенов делегирования.
    </description>
  </property>














