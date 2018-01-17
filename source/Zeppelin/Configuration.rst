Конфигурация Apache Zeppelin
----------------------------

+ `Свойства Zeppelin`_;
+ `Конфигурация SSL`_;

  + `Создание и настройка сертификатов`_;
  + `Настройка SSL на стороне сервера`_;
  + `Включение проверки подлинности сертификата на стороне клиента`_;
  + `Скрытие паролей с помощью Jetty Password Tool`_.


Свойства Zeppelin
^^^^^^^^^^^^^^^^^

Существует две локации, в которых можно настроить **Apache Zeppelin**:

+ Переменные параметры экосистемы могут быть заданы в *conf/zeppelin-env.sh* (*conf\zeppelin-env.cmd* для ОС Windows);
+ Свойства Java могут быть определены в *conf/zeppelin-site.xml*.

В случае если настроены обе локации, параметры экосистемы  будут приоритетными.

.. csv-table:: Параметры Zeppelin, их значения и описание
   :header: "zeppelin-env.sh", "zeppelin-site.xml", "Значение по умолчанию", "Описание"
   :widths: 25, 25, 25, 25

   "ZEPPELIN_PORT",	"zeppelin.server.port",	"8080", "Порт сервера Zeppelin. Примечание: необходимо убедиться, что не используется тот же порт, что для разработки веб-приложений Zeppelin (по умолчанию: 9000)"
   "ZEPPELIN_SSL_PORT",	"zeppelin.server.ssl.port",	"8443", "Ssl-сервер порта Zeppelin (используется, когда значение ssl environment/ property имеет значение true)"
   "ZEPPELIN_MEM",	"N/A",	"-Xmx1024m -XX:MaxPermSize=512m", "Параметры JVM mem"
   "ZEPPELIN_INTP_MEM",	"N/A",	"ZEPPELIN_MEM", "Параметры JVM mem для процесса интерпретатора"
   "ZEPPELIN_JAVA_OPTS",	"N/A", " ", "Параметры JVM"
   "ZEPPELIN_ALLOWED_ORIGINS",	"zeppelin.server.allowed.origins",	"*", "Позволяет знаком запятой (,) разделять разрешенные источники в списке для REST и websockets. Например, http://localhost:8080"
   "N/A",	"zeppelin.anonymous.allowed",	"true", "Допуск анонимного пользователя по умолчанию"
   "ZEPPELIN_SERVER_CONTEXT_PATH",	"zeppelin.server.context.path",	"/", "Контекстный путь веб-приложения"
   "ZEPPELIN_SSL",	"zeppelin.ssl",	"false",	" "
   "ZEPPELIN_SSL_CLIENT_AUTH",	"zeppelin.ssl.client.auth",	"false", " "
   "ZEPPELIN_SSL_KEYSTORE_PATH",	"zeppelin.ssl.keystore.path",	"keystore", " "
   "ZEPPELIN_SSL_KEYSTORE_TYPE",	"zeppelin.ssl.keystore.type",	"JKS", " "
   "ZEPPELIN_SSL_KEYSTORE_PASSWORD",	"zeppelin.ssl.keystore.password", " ", " "
   "ZEPPELIN_SSL_KEY_MANAGER_PASSWORD",	"zeppelin.ssl.key.manager.password", " ", " "
   "ZEPPELIN_SSL_TRUSTSTORE_PATH",	"zeppelin.ssl.truststore.path", " ", " "
   "ZEPPELIN_SSL_TRUSTSTORE_TYPE",	"zeppelin.ssl.truststore.type", " ", " "
   "ZEPPELIN_SSL_TRUSTSTORE_PASSWORD",	"zeppelin.ssl.truststore.password", " ", " "
   "ZEPPELIN_NOTEBOOK_HOMESCREEN",	"zeppelin.notebook.homescreen", " ", "Отображение идентификаторов заметок на рабочем столе Apache Zeppelin. Например, 2A94M5J1Z"
   "ZEPPELIN_NOTEBOOK_HOMESCREEN_HIDE",	"zeppelin.notebook.homescreen.hide",	"false", "Скрытие идентификатора заметки, установленной ZEPPELIN_NOTEBOOK_HOMESCREEN на рабочем столе Apache Zeppelin"
   "ZEPPELIN_WAR_TEMPDIR",	"zeppelin.war.tempdir",	"webapps", "Расположение временного каталога jetty"
   "ZEPPELIN_NOTEBOOK_DIR",	"zeppelin.notebook.dir",	"notebook", "Каталог root, в котором хранятся каталоги для блокнотов"
   "ZEPPELIN_NOTEBOOK_S3_BUCKET",	"zeppelin.notebook.s3.bucket",	"zeppelin", "S3 Bucket, где будут сохраняться файлы для блокнотов"
   "ZEPPELIN_NOTEBOOK_S3_USER",	"zeppelin.notebook.s3.user",	"user", "Имя пользователя S3 Bucket. Например, bucket/user/notebook/2A94M5J1Z/note.json"
   "ZEPPELIN_NOTEBOOK_S3_ENDPOINT",	"zeppelin.notebook.s3.endpoint",	"s3.amazonaws.com", "Конечная точка для Bucket"
   "ZEPPELIN_NOTEBOOK_S3_KMS_KEY_ID",	"zeppelin.notebook.s3.kmsKeyID", " ", "Идентификатор ключа AWS KMS, используемый для шифрования данных в S3 (опционально)"
   "ZEPPELIN_NOTEBOOK_S3_EMP",	"zeppelin.notebook.s3.encryptionMaterialsProvider", " ", "Имя класса реализации поставщика материалов шифрования пользовательского S3 для шифрования данных в S3 (опционально)"
   "ZEPPELIN_NOTEBOOK_AZURE_CONNECTION_STRING",	"zeppelin.notebook.azure.connectionString", " ", "Строка подключения учетной записи Azure. Например, DefaultEndpointsProtocol=https; AccountName=<accountName>; AccountKey=<accountKey>"
   "ZEPPELIN_NOTEBOOK_AZURE_SHARE",	"zeppelin.notebook.azure.share",	"zeppelin", "Azure Share, где будут сохраняться файлы блокнотов"
   "ZEPPELIN_NOTEBOOK_AZURE_USER",	"zeppelin.notebook.azure.user",	"user", "Необязательное имя пользователя для совместно используемого файла Azure. Например, share/user/notebook/2A94M5J1Z/note.json"
   "ZEPPELIN_NOTEBOOK_STORAGE",	"zeppelin.notebook.storage",	"org.apache.zeppelin.notebook.repo.GitNotebookRepo", "Разделенный запятыми список мест хранения блокнотов"
   "ZEPPELIN_NOTEBOOK_ONE_WAY_SYNC",	"zeppelin.notebook.one.way.sync",	"false", "Если есть несколько мест для хранения блокнотов, следует ли рассматривать первое как единственное?"
   "ZEPPELIN_NOTEBOOK_PUBLIC",	"zeppelin.notebook.public",	"true", "Сделать блокнот общедоступным по умолчанию при создании или импортировании (установив только владельцев). Если установлено значение false, необходимо добавить user, readers и writers и сделать его конфиденциальным и невидимым для других пользователей, не имующих прав"
   "ZEPPELIN_INTERPRETERS",	"zeppelin.interpreters", "org.apache.zeppelin.spark.SparkInterpreter, org.apache.zeppelin.spark.PySparkInterpreter, org.apache.zeppelin.spark.SparkSqlInterpreter, org.apache.zeppelin.spark.DepInterpreter, org.apache.zeppelin.markdown.Markdown, org.apache.zeppelin.shell.ShellInterpreter, ...", "Конфигурации интерпретатора с разделителями-запятыми [Class]. Примечание: это свойство устарело с Zeppelin-0.6.0 и не будет поддерживаться Zeppelin-0.7.0"
   "ZEPPELIN_INTERPRETER_DIR",	"zeppelin.interpreter.dir",	"interpreter", "Каталог интерпретатора"
   "ZEPPELIN_INTERPRETER_DEP_MVNREPO",	"zeppelin.interpreter.dep.mvnRepo",	"http://repo1.maven.org/maven2/", "Удаленный основной репозиторий для дополнительной загрузки зависимостей интерпретатора"
   "ZEPPELIN_DEP_LOCALREPO",	"zeppelin.dep.localrepo",	"local-repo", "Локальный репозиторий для загрузки зависимостей. Модули npm"
   "ZEPPELIN_HELIUM_NPM_REGISTRY",	"zeppelin.helium.npm.registry",	"http://registry.npmjs.org/", "Удаленный реестр Npm для загрузчки зависимостей Helium"
   "ZEPPELIN_INTERPRETER_OUTPUT_LIMIT",	"zeppelin.interpreter.output.limit",	"102400", "Скрыть выходное сообщение от интерпретатора, превышающего лимит"
   "ZEPPELIN_WEBSOCKET_MAX_TEXT_MESSAGE_SIZE",	"zeppelin.websocket.max.text.message.size",	"1024000", "Размер (в символах) максимального текстового сообщения, которое может быть получено от websocket"
   "ZEPPELIN_SERVER_DEFAULT_DIR_ALLOWED",	"zeppelin.server.default.dir.allowed",	"false", "Включить списки каталогов на сервере"
   
  
Конфигурация SSL
^^^^^^^^^^^^^^^^

Включение **SSL** требует некоторых изменений конфигурации -- необходимо создать сертификаты, а затем обновить необходимые настройки для подключения проверки подлинности **SSL** со стороны сервера и/или клиентской стороны.


Создание и настройка сертификатов
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Информацию о создании сертификатов и хранилище ключей можно найти по `ссылке <https://wiki.eclipse.org/Jetty/Howto/Configure_SSL>`_. Сжатый пример можно найти в верхнем ответе на запись `StackOverflow <http://stackoverflow.com/questions/4008837/configure-ssl-on-jetty>`_.

Хранилище ключей **keystore** содержит закрытый ключ и сертификат на сервер, а хранилище **trustore** содержит клиентские сертификаты. Необходимо убедиться, что путь и пароль для этих двух хранилищ правильно настроены в полях пароля ниже. Они могут быть скрыты с помощью инструмента паролей **Jetty**. После переноса **Maven** всех зависимостей для создания **Zeppelin**, один из jar-файлов **Jetty** будет содержать инструмент "Password". Необходимо вызвать эту команду из каталога сборки *Zeppelin Home* с соответствующей версией, пользователем и паролем:

   ::
   
    java -cp ./zeppelin-server/target/lib/jetty-all-server-<version>.jar org.eclipse.jetty.util.security.Password <user> <password>

Если используется самоподписанный сертификат, сертификат, подписанный недоверенным центром сертификации, или если включена аутентификация клиента, то у клиента должен быть установлен браузер, создающий исключения как для обычного https-порта, так и для websocket-порта. Это можно сделать, установив соединение HTTPS с обоими портами в браузере (например, если порты *443* и *8443*, перейти на *https://127.0.0.1:443* и *https://127.0.0.1:8443*). Данный шаг может быть пропущен, если сертификат сервера подписан доверенным центром сертификации и аутентификация клиента отключена.


Настройка SSL на стороне сервера
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Для включения **SSL** на стороне сервера необходимо обновить в *zeppelin-site.xml* следующие свойства: 

   ::
   
    <property>
      <name>zeppelin.server.ssl.port</name>
      <value>8443</value>
      <description>Server ssl port. (used when ssl property is set to true)</description>
    </property>

    <property>
      <name>zeppelin.ssl</name>
      <value>true</value>
      <description>Should SSL be used by the servers?</description>
    </property>

    <property>
      <name>zeppelin.ssl.keystore.path</name>
      <value>keystore</value>
      <description>Path to keystore relative to Zeppelin configuration directory</description>
    </property>

    <property>
      <name>zeppelin.ssl.keystore.type</name>
      <value>JKS</value>
      <description>The format of the given keystore (e.g. JKS or PKCS12)</description>
    </property>

    <property>
      <name>zeppelin.ssl.keystore.password</name>
      <value>change me</value>
      <description>Keystore password. Can be obfuscated by the Jetty Password tool</description>
    </property>

    <property>
      <name>zeppelin.ssl.key.manager.password</name>
      <value>change me</value>
      <description>Key Manager password. Defaults to keystore password. Can be obfuscated.</description>
    </property>


Включение проверки подлинности сертификата на стороне клиента
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Для включения аутентификации сертификата на стороне клиента необходимо обновить в *zeppelin-site.xml* следующие свойства:

   ::
   
    <property>
      <name>zeppelin.server.ssl.port</name>
      <value>8443</value>
      <description>Server ssl port. (used when ssl property is set to true)</description>
    </property>

    <property>
      <name>zeppelin.ssl.client.auth</name>
      <value>true</value>
      <description>Should client authentication be used for SSL connections?</description>
    </property>

    <property>
      <name>zeppelin.ssl.truststore.path</name>
      <value>truststore</value>
      <description>Path to truststore relative to Zeppelin configuration directory. Defaults to the keystore path</description>
    </property>

    <property>
      <name>zeppelin.ssl.truststore.type</name>
      <value>JKS</value>
      <description>The format of the given truststore (e.g. JKS or PKCS12). Defaults to the same type as the keystore type</description>
    </property>

    <property>
      <name>zeppelin.ssl.truststore.password</name>
      <value>change me</value>
      <description>Truststore password. Can be obfuscated by the Jetty Password tool. Defaults to the keystore password</description>
    </property>


Скрытие паролей с помощью Jetty Password Tool
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Рекомендации по безопасности рекомендуют не использовать текстовые пароли, а с помощью утилиты **Jetty Password Tool** (см. `документацию <http://www.eclipse.org/jetty/documentation/current/configuring-security-secure-passwords.html>`_) можно запутывать пароли, используемые для доступа к **KeyStore** и **TrustStore**.

После установки **Jetty Password Tool**:

   ::
   
    java -cp $ZEPPELIN_HOME/zeppelin-server/target/lib/jetty-util-9.2.15.v20160210.jar \
             org.eclipse.jetty.util.security.Password  \
             password
    
    2016-12-15 10:46:47.931:INFO::main: Logging initialized @101ms
    password
    OBF:1v2j1uum1xtv1zej1zer1xtn1uvk1v1v
    MD5:5f4dcc3b5aa765d61d8327deb882cf99

Затем необходимо обновить конфигурацию со скрытым паролем:

   ::
   
    <property>
      <name>zeppelin.ssl.keystore.password</name>
      <value>OBF:1v2j1uum1xtv1zej1zer1xtn1uvk1v1v</value>
      <description>Keystore password. Can be obfuscated by the Jetty Password tool</description>
    </property>


.. important:: После обновления настроек сервер Zeppelin необходимо перезапустить
