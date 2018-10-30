Проверка подключения к сервисам
=================================


Приведенные далее команды используются для проверки подключения между узлом шлюза и сервисами **Hadoop**, а так же для проверки подключения внешнего клиента к сервисам **Hadoop** через шлюз.

.. important:: При сбое связи между узлом шлюза и внутреннем сервисом Hadoop telnet подключается к порту сервиса, чтобы убедиться, что шлюз имеет доступ к узлу кластера. При этом необходимо использовать имя хоста и порты, указанные в определении сервиса

Тестирование WebHDFS
----------------------

Тестирование **WebHDFS** путем получения пути к домашнему каталогу:

+ На хосте шлюза ввести команду:

  ::
   
   curl http://$webhdfs-host:50070/webhdfs/v1?op=GETHOMEDIRECTORY

При этом хост отображает информацию:

  ::
  
   {"Path":"/user/gopher"}

+ На внешнем клиенте ввести команду:

  ::
   
   curl https://$gateway-host:$gateway_port/$gateway/$cluster_name/$webhdfs_service_name/v1?op=GETHOMEDIRECTORY

При этом внешний клиент отображает информацию:

  ::
  
   {"Path":"/user/gopher"}



Тестирование WebHCat/Templeton
-------------------------------

Тестирование **WebHCat**/**Templeton** путем получения версии:

+ На хосте шлюза ввести команду:

  ::
  
   curl http://$webhdfs-host:50111/templeton/v1/version

При этом хост шлюза отображает информацию:

  ::
  
   {"supportedVersions":["v1"],"version":"v1"}

+ На внешнем клиенте ввести команду:

  ::
  
   curl https://$gateway-host:$gateway_port/$gateway/$cluster_name/$webhcat_service_name/v1/version

При этом внешний клиент отображает информацию:

  ::
  
   {"supportedVersions":["v1"],"version":"v1"}


Тестирование Oozie
-------------------

Тестирование **Oozie** путем получения версии:

+ На хосте шлюза ввести команду:

  ::
  
   curl http://$oozie-host:11000/oozie/v1/admin/build-version

При этом хост шлюза отображает информацию:

  ::
  
   {"buildVersion":"4.0.0.2.1.1.0-302"}

+ На внешнем клиенте ввести команду:

  ::
  
   curl https://$gateway-host:$gateway_port/$gateway/$cluster_name/$oozie_service_name/v1/admin/build-version

При этом внешний клиент отображает информацию:

  ::
  
   {"buildVersion":"4.0.0.2.1.1.0-302"}


Тестирование HBase/Stargate
-----------------------------

Тестирование **HBase**/**Stargate** путем получения версии:

+ На хосте шлюза ввести команду:

  ::
  
   curl http://$hbase-host:17000/version

При этом хост шлюза отображает информацию:

  ::
  
   rest 0.0.2 JVM: Oracle Corporation 1.7.0_51-24.45-b08 OS: Linux 3.8.0-29-generic amd64 Server:jetty/6.1.26 Jersey:1.8:

+ На внешнем клиенте ввести команду:

  ::
  
   curl http://$hbase-host:17000/version

При этом внешний клиент отображает информацию:

  ::
  
   rest 0.0.2 JVM: Oracle Corporation 1.7.0_51-24.45-b08 OS: Linux 3.8.0-29-generic amd64 Server:jetty/6.1.26 Jersey:1.8


Тестирование HiveServer2
-------------------------

Тестирование **HiveServer2** посредством следующих URL-адресов, возвращающих ошибку проверки подлинности, но при этом которую пользователи могут безопасно игнорировать:

+ На хосте шлюза ввести:

  :command:`curl http://$hive-host:10001/cliservice`

+ На внешнем клиенте ввести:

  :command:`curl https://$gateway-host:$gateway_port/$gateway/$cluster_name/$hive_service_name/cliservice`

