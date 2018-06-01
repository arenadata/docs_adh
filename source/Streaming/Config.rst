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
+ DEFAULT -- - 1
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

**log.flush.interval.ms** -- Максимальное время хранения данных в любом топике в памяти до их сброса на диск. Указывается в миллисекундах. Если параметр не установлен, используется значение *log.flush.scheduler.interval.ms*

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
+ DEFAULT -- - 1
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.retention.hours** -- Количество часов для хранения файла журнала перед его удалением, третично по отношению к свойству *log.retention.ms*. Указывается в часах

+ TYPE -- int
+ DEFAULT -- 168
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**log.retention.minutes** -- Количество минут для хранения файла журнала перед его удалением, вторично по отношению к свойству *log.retention.hours*. Указывается в минутах

+ TYPE -- int
+ DEFAULT -- null
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**log.retention.ms** -- Количество миллисекунд для хранения файла журнала перед его удалением. Указывается в миллисекундах. Если параметр не установлен, используется значение *log.retention.minutes*

+ TYPE -- long
+ DEFAULT -- null
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.roll.hours** -- Максимальное время до развертывания нового сегмента журнала, вторично по отношению к свойству *log.roll.ms*. Указывается в часах

+ TYPE -- int	
+ DEFAULT -- 168
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- [1,...]
+ DYNAMIC UPDATE MODE -- read-only

**log.roll.jitter.hours** -- Максимально допустимое значение джиттера для вычитания из *logRollTimeMillis*, вторично по отношению к свойству *log.roll.jitter.ms*. Указывается в часах

+ TYPE -- int
+ DEFAULT -- int
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**log.roll.jitter.ms** -- Максимально допустимое значение джиттера для вычитания из *logRollTimeMillis*. Указывается в миллисекундах. Если параметр не установлен, используется значение *log.roll.jitter.hours*

+ TYPE -- long
+ DEFAULT -- long
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.roll.ms** -- Максимальное время до развертывания нового сегмента журнала. Указывается в миллисекундах. Если параметр не установлен, используется значение *log.roll.hours* 

+ TYPE -- long
+ DEFAULT -- null
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.segment.bytes** -- Максимальный размер одного файла журнала

+ TYPE -- int
+ DEFAULT -- 1073741824
+ VALID VALUES -- [14,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**log.segment.delete.delay.ms** -- Время ожидания перед удалением файла из файловой системы

+ TYPE -- long
+ DEFAULT -- 60000
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**message.max.bytes** -- Наибольший размер пакета данных, разрешенный ADS. При увеличении параметра следует также увеличить размер выборки для потребителей с целью обеспечения возможности получения пакета данных установленного размера. Параметр можно настроить для каждого топика с помощью поуровневой конфирурации топика *max.message.bytes*

+ TYPE -- int
+ DEFAULT -- 1000012
+ VALID VALUES -- [0,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**min.insync.replicas** -- При установленном поставщиком подтверждении acks на "all" или "-1", *min.insync.replicas* задается на минимальное количество реплик для подтверждения записи. Если этот минимум не может быть удовлетворен, то поставщик задает исключение (либо *NotEnoughReplicas*, либо *NotEnoughReplicasAfterAppend*). Совместное использование *min.insync.replicas* и acks обеспечивает более высокую гарантию к устойчивости. Типичным сценарием является создание топика с коэффициентом репликации *3*, параметром *min.insync.replicas* равным *2* и acks установленным на "all". Это гарантирует, что поставщик задает исключение, если большинство реплик не принимает запись

+ TYPE -- int
+ DEFAULT -- 1
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**num.io.threads** -- Число потоков, используемых сервером для обработки запросов, которые могут включать дисковые операции ввода-вывода

+ TYPE -- int
+ DEFAULT -- 8
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**num.network.threads** -- Количество потоков, используемых сервером для получения запросов от сети и отправки ответов в сеть

+ TYPE -- int
+ DEFAULT -- 3
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**num.recovery.threads.per.data.dir** -- Число потоков в каталоге данных, используемых для восстановления журнала при запуске или при сбросе по прекращению работы

+ TYPE -- int
+ DEFAULT -- 1
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**num.replica.alter.log.dirs.threads** -- Число потоков, которые могут перемещать реплики между каталогами журналов, включая дисковые операции ввода-вывода

+ TYPE -- int
+ DEFAULT -- null
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**num.replica.fetchers** -- Количество потоков выборки, используемых для репликации данных от исходного брокера. Увеличение этого значения может увеличить степень параллелизма ввода-вывода в брокере-подписчике

+ TYPE -- int
+ DEFAULT -- 1
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- cluster-wide

**offset.metadata.max.bytes** -- Максимальный размер для записи метаданных с учетом фиксации смещения

+ TYPE -- int
+ DEFAULT -- 4096
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**offsets.commit.required.acks** -- Принятие необходимых подтверждений acks перед фиксацией данных. Значение по умолчанию "-1" не следует переопределять

+ TYPE -- short
+ DEFAULT -- - 1
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**offsets.commit.timeout.ms** -- Фиксация смещения откладывается до тех пор, пока все реплики для топика смещения не получат коммит или данный установленный таймаут не будет достигнут. Аналогично времени ожидания запроса поставщика

+ TYPE -- int
+ DEFAULT -- 5000
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**offsets.load.buffer.size** -- Размер пакета для чтения из сегментов смещений при загрузке смещений в кэш

+ TYPE -- int
+ DEFAULT -- 5242880
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**offsets.retention.check.interval.ms** -- Частота проверки устаревших смещений

+ TYPE -- long
+ DEFAULT -- 600000
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**offsets.retention.minutes** -- Сброс смещений старше установленного срока хранения

+ TYPE -- int
+ DEFAULT -- 1440
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only 

**offsets.topic.compression.codec** -- Кодек сжатия для топика смещения. Сжатие может использоваться для достижения "атомных" коммитов

+ TYPE -- int
+ DEFAULT -- 0
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**offsets.topic.num.partitions** -- Количество партиций для коммита топика смещения (не следует изменять после развертывания)

+ TYPE -- int
+ DEFAULT -- 50
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**offsets.topic.replication.factor** -- Коэффициент репликации для топика смещения (устанавливается выше с целью обеспечения доступности). Создание внутреннего топика невозможно, пока размер кластера не соответствует данному требованию коэффициента репликации

+ TYPE -- short
+ DEFAULT -- 3
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**offsets.topic.segment.bytes** -- Байты сегмента топика смещений. Значение должно быть относительно небольшим с целью ускорения сжатия журнала и загрузку кэша

+ TYPE -- int
+ DEFAULT -- 104857600
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**port** -- Применяется при неустановленном параметре *listeners*. Рекомендуется использовать *listeners*. Обозначает порт для прослушивания и приема подключений

+ TYPE -- int
+ DEFAULT -- 9092
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**queued.max.requests** -- Количество запросов в очереди до блокировки сетевых потоков

+ TYPE -- int
+ DEFAULT -- 500
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**quota.consumer.default** -- Применяется при неустановленном параметре динамических квот по умолчанию в Zookeeper. Любой потребитель группы customerId/consumer дросселируется при получении большего количества байтов, чем данное установленное значение в секунду

+ TYPE -- long
+ DEFAULT -- 9223372036854775807
+ VALID VALUES -- [1,...]
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**quota.producer.default** -- Применяется при неустановленном параметре динамических квот по умолчанию в Zookeeper. Любой поставщик с известным clientId дросселируется при получении большего количества байтов, чем данное установленное значение в секунду

+ TYPE -- long
+ DEFAULT -- 9223372036854775807
+ VALID VALUES -- [1,...]
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
