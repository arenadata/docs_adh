Аутентификация Apache Shiro для Apache Zeppelin
-----------------------------------------------

+ `Обзор`_
+ `Настройка безопасности`_
+ `Группы и разрешения (опционально)`_
+ `Настройка Realm (опционально)`_
  
  + `Active Directory`_
  + `LDAP`_
  + `РАМ`_
  + `ZeppelinHub`_

+ `Безопасное Cookie для сессий Zeppelin (опционально)`_
+ `Защитита информации Zeppelin (опционально)`_


Обзор
^^^^^^

`Apache Shiro <http://shiro.apache.org/>`_ -- это мощная и простая в использовании платформа безопасности **Java**, выполняющая аутентификацию, авторизацию, криптографию и управление сессиями. В данном разделе объясняется, как **Shiro** работает для аутентификации блокнота **Zeppelin**.

При подключении к **Apache Zeppelin** необходимо ввести учетные данные. После входа в систему появляется доступ ко всем заметкам, включая заметки других пользователей.


Настройка безопасности
^^^^^^^^^^^^^^^^^^^^^^
  
Настройка аутентификации блокнота **Zeppelin** осуществляется несколькими простыми шагами:
  
**1. Права Shiro**

По умолчанию в *conf* находится файл *shiro.ini.template*. Данный файл используется в качестве примера, и настоятельно рекомендуется создать файл *shiro.ini*, выполнив следующую команду:

  :command:`cp conf/shiro.ini.template conf/shiro.ini`

Дополнительная информация о формате файла *shiro.ini* находится по ссылке `Shiro Configuration <http://shiro.apache.org/configuration.html#Configuration-INISections>`_.


**2. Безопасный канал Websocket**

Для свойства *zeppelin.anonymous.allowed* необходимо установить значение *false* в *conf/zeppelin-site.xml*. В случае если данного файла  нет, следует просто скопировать файл *conf/zeppelin-site.xml.template* в *conf/zeppelin-site.xml*.


**3. Запуск Zeppelin**

Для запуска **Zeppelin** необходимо выполнить команду:

  :command:`bin/zeppelin-daemon.sh start (or restart)`

После чего можно просматривать **Zeppelin** по адресу *http://localhost:8080*. 


**4. Авторизация**

Теперь можно войти в систему, используя комбинацию имени и пароля пользователя (:numref:`Рис.%s.<zeppelin_authentication_login>`).

.. _zeppelin_authentication_login:

.. figure:: ../imgs/zeppelin_authentication_login.*
   :align: center

   Авторизация в Apache Zeppelin

Рядом с паролем можно установить роли для каждого пользователя:

   ::
   
    [users]

    admin = password1, admin
    user1 = password2, role1, role2
    user2 = password3, role3
    user3 = password4, role2



Группы и разрешения (опционально)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Для использования групп пользователей и разрешений необходимо применить одну из следующих конфигураций для **LDAP** или **AD** в секции *[main]* в файле *shiro.ini*:

   ::
    
    activeDirectoryRealm = org.apache.zeppelin.realm.ActiveDirectoryGroupRealm
    activeDirectoryRealm.systemUsername = userNameA
    activeDirectoryRealm.systemPassword = passwordA
    activeDirectoryRealm.searchBase = CN=Users,DC=SOME_GROUP,DC=COMPANY,DC=COM
    activeDirectoryRealm.url = ldap://ldap.test.com:389
    activeDirectoryRealm.groupRolesMap = "CN=aGroupName,OU=groups,DC=SOME_GROUP,DC=COMPANY,DC=COM":"group1"
    activeDirectoryRealm.authorizationCachingEnabled = false
    activeDirectoryRealm.principalSuffix = @corp.company.net

    ldapRealm = org.apache.zeppelin.server.LdapGroupRealm
    # search base for ldap groups (only relevant for LdapGroupRealm):
    ldapRealm.contextFactory.environment[ldap.searchBase] = dc=COMPANY,dc=COM
    ldapRealm.contextFactory.url = ldap://ldap.test.com:389
    ldapRealm.userDnTemplate = uid={0},ou=Users,dc=COMPANY,dc=COM
    ldapRealm.contextFactory.authenticationMechanism = simple


И определить роли/группы, которые необходимо иметь в системе, например:

   ::
    
    [roles]
    admin = *
    hr = *
    finance = *
    group1 = *


Настройка Realm (опционально)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Realms** отвечают за аутентификацию и авторизацию в **Apache Zeppelin**. По умолчанию **Apache Zeppelin** использует `IniRealm <https://shiro.apache.org/static/latest/apidocs/org/apache/shiro/realm/text/IniRealm.html>`_ (пользователи и группы настраиваются в файле *conf/shiro.ini* в секциях *[user]* и *[group]*). Также можно использовать **Shiro Realms**, такие как `JndiLdapRealm <https://shiro.apache.org/static/latest/apidocs/org/apache/shiro/realm/ldap/JndiLdapRealm.html>`_, `JdbcRealm <https://shiro.apache.org/static/latest/apidocs/org/apache/shiro/realm/jdbc/JdbcRealm.html>`_ или `создать собственный <https://shiro.apache.org/static/latest/apidocs/org/apache/shiro/realm/AuthorizingRealm.html>`_. Подробная документация о **Apache Shiro Realm** представлена по `ссылке <http://shiro.apache.org/realm.html>`_.


Active Directory
~~~~~~~~~~~~~~~~

   ::
   
    activeDirectoryRealm = org.apache.zeppelin.realm.ActiveDirectoryGroupRealm
    activeDirectoryRealm.systemUsername = userNameA
    activeDirectoryRealm.systemPassword = passwordA
    activeDirectoryRealm.hadoopSecurityCredentialPath = jceks://file/user/zeppelin/conf/zeppelin.jceks
    activeDirectoryRealm.searchBase = CN=Users,DC=SOME_GROUP,DC=COMPANY,DC=COM
    activeDirectoryRealm.url = ldap://ldap.test.com:389
    activeDirectoryRealm.groupRolesMap = "CN=aGroupName,OU=groups,DC=SOME_GROUP,DC=COMPANY,DC=COM":"group1"
    activeDirectoryRealm.authorizationCachingEnabled = false
    activeDirectoryRealm.principalSuffix = @corp.company.net


Кроме того, вместо указания *systemPassword* в тексте в *shiro.ini* администратор может указать то же самое в *hadoop credential*. Необходимо создать keystore-файл, используя командную строку *hadoop credential*, для этого *hadoop* должен быть в *classpath*:

   ::
   
    hadoop credential create activeDirectoryRealm.systempassword -provider jceks://file/user/zeppelin/conf/zeppelin.jceks

Далее следует изменить следующие значения в файле *Shiro.ini* и раскомментировать строку:

   ::
   
    activeDirectoryRealm.hadoopSecurityCredentialPath = jceks://file/user/zeppelin/conf/zeppelin.jceks

LDAP
~~~~

Для настройки диапазона **LDAP Realm** существует два варианта. Проще использовать **LdapGroupRealm**. Однако, он имеет ограниченную гибкость при сопоставлении групп **LDAP** для пользователей и для авторизации групп пользователей. Далее приведен пример файла конфигурации:

   ::
   
    ldapRealm = org.apache.zeppelin.realm.LdapGroupRealm
    # search base for ldap groups (only relevant for LdapGroupRealm):
    ldapRealm.contextFactory.environment[ldap.searchBase] = dc=COMPANY,dc=COM
    ldapRealm.contextFactory.url = ldap://ldap.test.com:389
    ldapRealm.userDnTemplate = uid={0},ou=Users,dc=COMPANY,dc=COM
    ldapRealm.contextFactory.authenticationMechanism = simple

Другим более гибким вариантом является использование **LdapRealm**. Он позволяет сопоставлять *ldapgroups* с ролями, а также допускает проверку подлинности на основе ролей/групп на сервере *zeppelin*. Пример конфигурации приведен ниже:

   ::
   
    ldapRealm=org.apache.zeppelin.realm.LdapRealm

    ldapRealm.contextFactory.authenticationMechanism=simple ldapRealm.contextFactory.url=ldap://localhost:33389  ldapRealm.userDnTemplate=uid={0},ou=people,dc=hadoop,dc=apache,dc=org
   


РАМ
~~~~

Поддержка аутентификации `PAM <https://en.wikipedia.org/wiki/Pluggable_authentication_module>`_ позволяет повторно использовать существующие модули аутентификации на хосте, где запущен **Zeppelin**. В типичных системных модулях, например, *sshd*, *passwd* и других сервис настраивается в */etc/pam.d/*. Можно повторно использовать один из этих сервисов или создать свой собственный для **Zeppelin**. Для активации аутентификации **PAM** требуется два параметра: 1 -- realm: использование **Shiro realm**; 2 -- service: настроенный в */etc/pam.d/* сервис. Название должно совпадать с именем файла в */etc/pam.d/*.

   ::
    
    [main]
     pamRealm=org.apache.zeppelin.realm.PamRealm
     pamRealm.service=sshd


ZeppelinHub
~~~~~~~~~~~

`ZeppelinHub <https://www.zeppelinhub.com/>`_ -- это сервис, синхронизурующий блокноты **Apache Zeppelin** и обеспечивающий легкое взаимодействие с ними. Для подключения **ZeppelinHub** необходимо применить следующее изменение в *conf/shiro.ini* в секции *[main]*:

   ::
    
    ### A sample for configuring ZeppelinHub Realm
    zeppelinHubRealm = org.apache.zeppelin.realm.ZeppelinHubRealm
    ## Url of ZeppelinHub
    zeppelinHubRealm.zeppelinhubUrl = https://www.zeppelinhub.com
    securityManager.realms = $zeppelinHubRealm

.. important:: ZeppelinHub не связан с проектом Apache Zeppelin


Безопасное Cookie для сессий Zeppelin (опционально)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Zeppelin** может быть настроен для установки **HttpOnly** в сессии **cookie**. С такой конфигурацией cookie-файлы **Zeppelin** не могут быть доступны через скрипты на стороне клиента, тем самым предотвращая большинство атак типа **Cross-Site scripting** (**XSS**).

Чтобы включить безопасную поддержку файлов **cookie** через **Shiro**, необходимо добавить следующие строки в *conf/shiro.ini* в секцию *[main]*, а затем задать *sessionManager*:

   ::
    
    cookie = org.apache.shiro.web.servlet.SimpleCookie
    cookie.name = JSESSIONID
    cookie.secure = true
    cookie.httpOnly = true
    sessionManager.sessionIdCookie = $cookie


Защита информации Zeppelin (опционально)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

По умолчанию любой пользователь, определенный в *[users]*, может видеть информацию о параметрах интерпретатора, учетные данные и настройки в **Apache Zeppelin**. В случае если данную информацию необходимо скрыть, поскольку **Shiro** обеспечивает защиту на основе url, следует закомментировать или раскомментировать приведенные ниже строки в *conf/shiro.ini*:

   ::
   
    [urls]

    /api/interpreter/** = authc, roles[admin]
    /api/configurations/** = authc, roles[admin]
    /api/credential/** = authc, roles[admin]

В таком случае информацию о настройках интерпретатора, учетных данных и конфигурации в **Apache Zeppelin** могут видеть только пользователи с ролью *admin*. При необходимости предоставления прав другим пользователям следует изменить роли в секции *[users]*.



