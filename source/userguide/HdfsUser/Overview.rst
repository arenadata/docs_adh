Общий обзор
===========

**HDFS** -- это основное распределенное хранилище, используемое приложениями **Hadoop**. Кластер **HDFS** в основном состоит из NameNode, который управляет метаданными файловой системы, и DataNode, в которых хранятся фактические данные. Диаграмма архитектуры **HDFS** отображает основные взаимодействия между NameNode, DataNodes и клиентами: клиенты связываются с NameNode для изменения метаданных файла и выполняют фактический ввод-вывод файла непосредственно с помощью DataNodes.

Далее приведены некоторые из основных функций, которые могут представлять интерес для пользователей:

* Hadoop, включая HDFS, хорошо подходит для распределенного хранения и распределенной обработки: он отказоустойчив, масштабируем и чрезвычайно прост в расширении. MapReduce, хорошо известный своей простотой и применимостью для распределенных приложений, является неотъемлемой частью Hadoop.

* HDFS легко настраивается с конфигурацией по умолчанию, которая подходит для многих установок. Но при этом в большинстве случаев в зависимости от специфики решаемых задач конфигурацию необходимо настраивать.

* Hadoop написан на Java и поддерживается на всех основных платформах.

* Hadoop поддерживает shell-подобные команды для непосредственного взаимодействия с HDFS.

* NameNode и Datanodes имеют встроенные веб-интерфейсы, которые позволяют легко проверять текущее состояние кластера.

* Новые функции и улучшения регулярно внедряются в HDFS. Ниже приведен перечень полезных функций HDFS:

  * Права доступа к файлам и аутентификация;
  * Rack awareness: учет физического местоположения узла при планировании задач и выделении хранилища;
  * Safemode: административный режим для сопровождения;
  * fsck: утилита для диагностики работоспособности файловой системы, поиска отсутствующих файлов или блоков;
  * fetchdt: утилита для извлечения DelegationToken и сохранения его в файле в локальной системе;
  * Balancer: инструмент для балансировки кластера, когда данные неравномерно распределены между DataNodes;
  * Upgrade и rollback: после обновления программного обеспечения можно выполнить откат до состояния HDFS перед обновлением в случае непредвиденных проблем;
  * Secondary NameNode: выполняет периодические checkpoints пространства имен и помогает поддерживать размер файла, содержащего журнал изменений HDFS, в определенных пределах в NameNode;
  * Checkpoint node: выполняет периодические checkpoints пространства имен и помогает минимизировать размер журнала с изменениями HDFS, хранящийся в NameNode. NameNode позволяет использовать несколько узлов Checkpoint одновременно, если в системе не зарегистрированы резервные узлы;
  * Backup node: расширение узла Checkpoint. В дополнение к checkpointing он также получает поток изменений от NameNode и поддерживает свою собственную копию пространства имен в памяти, которая всегда синхронизируется с активным состоянием пространства имен NameNode. Только один резервный узел может быть зарегистрирован с помощью NameNode одновременно.

NameNode и DataNode запускают внутренний веб-сервер для отображения базовой информации о текущем состоянии кластера. В конфигурации по умолчанию главная страница NameNode находится по адресу http://namenode-name:9870/. На ней перечислены узлы данных в кластере и основные статистические данные кластера. Веб-интерфейс также можно использовать для просмотра файловой системы, используя ссылку "Browse the file system" на главной странице NameNode.

Права доступа к файлам в системе схожи с правами доступа на других известных платформах, таких как Linux. В настоящее время безопасность ограничивается простыми разрешениями на доступ к файлам. Пользователь, запускающий NameNode, рассматривается как суперпользователь для **HDFS**, однако, будущие версии **HDFS** будут поддерживать сетевые протоколы аутентификации, такие как **Kerberos**, для аутентификации пользователей и шифрования передачи данных.

**HDFS** имеет один NameNode для каждого кластера. В настоящее время общий объем памяти, доступный на NameNode, является основным ограничением масштабируемости. В очень больших кластерах увеличение среднего размера файлов, хранящихся в **HDFS**, помогает увеличить размер кластера без увеличения требований к памяти для NameNode.
