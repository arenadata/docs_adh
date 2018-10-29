Настройка URL-адресов сервисов Hadoop
=======================================


Настройка доступа к внутреннему сервису **Hadoop** через **Knox Gateway** осуществляется в два шага:

1. Необходимо изменить файл *$gateway/conf/topologies$cluster-name.xml*, добавив запись, аналогичную следующей, для каждого сервиса **Hadoop**:

  ::
  
   <topology>
       <gateway>
        ...
        </gateway>
        <service>
            <role> $service_name </role>
            <url> $schema://$hostname:$port</url>
        </service>
   </topology>

Где:

+ *$service_name* -- это *AMBARI*, *AMBARIUI*, *ATLAS*, *HIVE*, *JOBTRACKER*, *NAMENODE*, *OOZIE*, *RANGER*, *RANGERUI*, *RESOURCEMANAGER*, *WEBHBASE*, *WEBHCAT*, *WEBHDFS*, *ZEPPELINUI* или *ZEPPELINWS*;
+ *<url>* -- это полный URL-адрес внутреннего кластера для доступа к сервису, включая:

  + *$schema* -- протокол сервиса;
  + *$hostname* -- разрешенное имя внутреннего узла;
  + *$port* -- порт прослушивания сервиса.

2. Сохранить файл. При этом шлюз создает новый WAR-файл с измененной временной меткой в *$gateway/data/deployments*.

Перезапуск сервера **Knox** после внесения изменений в топологию или сервисы кластера **Hadoop** не требуется.


