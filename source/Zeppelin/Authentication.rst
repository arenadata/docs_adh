Аутентификация Apache Shiro для Apache Zeppelin
-----------------------------------------------

+ `Обзор`_;
+ `Настройка безопасности`_;
+ `Группы и разрешения (опционально)`_;
+ `Настройка Realm (опционально)`_;
  
  + `Active Directory`_;
  + `LDAP`_;
  + `РАМ`_;
  + `ZeppelinHub`_;

+ `Безопасное Cookie для сессий Zeppelin (опционально)`_;
+ `Защитита информации Zeppelin (опционально)`_;
+ `Другие методы проверки идентификации`_.


Обзор
^^^^^^

`Apache Shiro <http://shiro.apache.org/>`_ -- это мощная и простая в использовании платформа безопасности **Java**, выполняющая аутентификацию, авторизацию, криптографию и управление сессиями. В данном разделе объясняется, как **Shiro** работает для аутентификации блокнота **Zeppelin**.

При подключении к **Apache Zeppelin** предлагается ввести учетные данные. После входа в систему появляется доступ ко всем заметкам, включая заметки других пользователей.


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

Можно установить роли для каждого пользователя рядом с паролем:

   ::
   
    [users]

    admin = password1, admin
    user1 = password2, role1, role2
    user2 = password3, role3
    user3 = password4, role2



Группы и разрешения (опционально)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Для использования групп пользователей и разрешений, необходимо применить одну из следующих конфигураций для **LDAP** или **AD** в *[main]* в файле *shiro.ini*:

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



Active Directory
~~~~~~~~~~~~~~~~


LDAP
~~~~



РАМ
~~~~



ZeppelinHub
~~~~~~~~~~~




Безопасное Cookie для сессий Zeppelin (опционально)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^



Защитита информации Zeppelin (опционально)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^




Другие методы проверки идентификации
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

