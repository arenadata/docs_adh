Настройки платформы
-------------------

Платформа **ADS** использует пары ключ-значение в `формате файла свойств <http://en.wikipedia.org/wiki/.properties>`_ для конфигурации. Эти значения могут быть поставлены либо из файла, либо программно.


Настройка брокера
^^^^^^^^^^^^^^^^^^

Основные конфигурации брокера:

+ *broker.id*
+ *log.dirs*
+ *zookeeper.connect*

Далее приведен список настроек с описанием и указанием их типа, значений по умолчанию и действительных, их важностью и режимом обновления.

**zookeeper.connect** -- Строка хоста Zookeeper

+ TYPE -- string
+ DEFAULT -- high
+ DYNAMIC UPDATE MODE -- read-only

**advertised.host.name** -- Применяется при неустановленном параметре *advertised.listeners* или *listeners*. Рекомендуется использовать *advertized.listeners*. Обозначает имя хоста для публикации в ZooKeeper для использования клиентами. В средах IaaS может отличаться от интерфейса, к которому привязывается брокер. Если параметр не задан, используется настроенное значение для *host.name*. В противном случае -- значение из *java.net.InetAddress.getCanonicalHostName()*

+ TYPE -- string
+ DEFAULT -- null
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**advertised.listeners** -- Слушатели для публикации в ZooKeeper для использования клиентами (если есть отличие от свойства *listeners*). В средах IaaS может отличаться от интерфейса, к которому привязывается брокер. Если параметр не задан, используется значение для *listeners*. В отличие от *listeners* значение "0.0.0.0" недопустимо

+ TYPE -- string
+ DEFAULT -- null
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- per-broker

**advertised.port** -- Применяется при неустановленном параметре *advertised.listeners* или *listeners*. Рекомендуется использовать *advertized.listeners*. Обозначает имя хоста для публикации в ZooKeeper для использования клиентами. В средах IaaS может отличаться от интерфейса, к которому привязывается брокер. Если параметр не задан, публикуется тот же порт, к которому привязан брокер

+ TYPE -- int
+ DEFAULT -- null
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**auto.create.topics.enable** -- Включение автоматического создания топика на сервере

+ TYPE -- boolean
+ DEFAULT -- true
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**auto.leader.rebalance.enable** -- Включение автоматической балансировки лидера. Балансировка лидера в фоновом режиме через регулярные промежутки времени

+ TYPE -- boolean
+ DEFAULT -- true
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only 

**background.threads** -- Количество потоков для различных задач фоновой обработки

+ TYPE -- int
+ DEFAULT -- 10
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**broker.id** -- Идентификатор брокера для сервера. Если значение не установлено, создается уникальный идентификатор брокера. Чтобы избежать конфликтов между id брокера, созданными с помощью zookeeper, и id брокера, настроенными пользователем, генерация идентификаторов брокера начинается с *reserved.broker.max.id + 1*

+ TYPE -- int
+ DEFAULT -- -1
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**compression.type** -- Конечный тип сжатия для топика. Конфигурация принимает стандартные кодеки сжатия ("gzip", "snappy", "lz4"). Так же возможно "uncompressed", что эквивалентно отсутствию сжатия; и "producer", что означает сохранение исходного кодека сжатия, установленного поставщиком

+ TYPE -- string
+ DEFAULT -- producer
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**delete.topic.enable** -- Включение возможности удаления топика. При отключенном параметре удаление топика через администратора не имеет результата

+ TYPE -- boolean
+ DEFAULT -- true
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**host.name** -- Применяется только когда параметр *listeners* не установлен. Рекомендуется использовать *listeners*. Обозначает имя хоста брокера. Если параметр задан, то привязка выполняется только к данному адресу. Если параметр не задан, привязка выполняется ко всем интерфейсам

+ TYPE -- string
+ DEFAULT -- "" 
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**leader.imbalance.check.interval.seconds** -- Частота, с которой контроллер запускает проверку балансировки партиции

+ TYPE -- long
+ DEFAULT -- 300
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**leader.imbalance.per.broker.percentage** -- Коэффициент дисбаланса лидера, допустимый для каждого брокера. Контроллер запускает балансировку лидера, если он превышает данное значение для брокера. Указывается в процентах

+ TYPE -- int
+ DEFAULT -- 10
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**listeners** -- Listener List -- Разделенный запятыми список URI, которые прослушиваются, и имена слушателей сети. Если имя слушателя не является протоколом безопасности, необходимо установить *listener.security.protocol.map*. Для привязки ко всем интерфейсам указать имя хоста "0.0.0.0". Если имя хоста не указано, привязка осуществляется к интерфейсу по умолчанию. Примеры списков слушателей сети: PLAINTEXT://myhost:9092,SSL://:9091,CLIENT://0.0.0.0:9092,REPLICATION://localhost:9093

+ TYPE -- string
+ DEFAULT -- null
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- per-broker

**log.dir** -- Каталог хранения данных журнала (дополнительный для свойства log.dirs)

+ TYPE -- string
+ DEFAULT -- /tmp/kafka-logs
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**log.dirs** -- Каталоги хранения данных журнала. Если параметр не установлен, используется значение свойства *log.dir*

+ TYPE -- string
+ DEFAULT -- null
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- high

**log.flush.interval.messages** -- Количество накопленных в партиции журнала данных перед их сбросом на диск

+ TYPE -- long
+ DEFAULT -- 9223372036854775807
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.flush.interval.ms** -- Максимальное время хранения данных в любом топике в памяти до их сброса на диск. Если параметр не установлен, используется значение *log.flush.scheduler.interval.ms*. Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- null
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.flush.offset.checkpoint.interval.ms** -- Частота обновления постоянной записи последнего сброса, который действует как точка восстановления журнала

+ TYPE -- int
+ DEFAULT -- 60000
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**log.flush.scheduler.interval.ms** -- Частота log flusher проверки на необходимость сброса какого-либо журнала на диск. Указывается в миллисекундах

+ TYPE -- long
+ DEFAULT -- 9223372036854775807
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**log.flush.start.offset.checkpoint.interval.ms** -- Частота обновления постоянной записи смещения начала журнала

+ TYPE -- int
+ DEFAULT -- 60000
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**log.retention.bytes** -- Максимальный размер журнала перед его удалением

+ TYPE -- long
+ DEFAULT -- -1
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.retention.hours** -- Количество часов для хранения файла журнала перед его удалением, третично по отношению к свойству *log.retention.ms*. Указывается в часах

+ TYPE -- int
+ DEFAULT -- 168
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**** -- 

+ TYPE -- 
+ DEFAULT -- 
+ VALID VALUES -- 
+ IMPORTANCE -- 
+ DYNAMIC UPDATE MODE -- 





Настройка на уровне топика
^^^^^^^^^^^^^^^^^^^^^^^^^^^


Конфигурирование Producer
^^^^^^^^^^^^^^^^^^^^^^^^^


Конфигурирование Consumer
^^^^^^^^^^^^^^^^^^^^^^^^^


Конфигурирование Connect
^^^^^^^^^^^^^^^^^^^^^^^^


Конфигурирование Streams
^^^^^^^^^^^^^^^^^^^^^^^^^
