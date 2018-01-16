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
   "ZEPPELIN_NOTEBOOK_DIR",	"zeppelin.notebook.dir",	"notebook", "Каталог root, в котором хранятся каталоги для ноутбуков"
   "ZEPPELIN_NOTEBOOK_S3_BUCKET",	"zeppelin.notebook.s3.bucket",	"zeppelin", "S3 Bucket, где будут сохраняться файлы для ноутбуков"
   
   





Конфигурация SSL
^^^^^^^^^^^^^^^^


Создание и настройка сертификатов
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Настройка SSL на стороне сервера
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



Включение проверки подлинности сертификата на стороне клиента
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



Скрытие паролей с помощью Jetty Password Tool
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~





