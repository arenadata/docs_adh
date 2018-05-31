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

**advertised.host.name** -- Применяется при неустановленном параметре *advertised.listeners* или *listeners*. Рекомендуется использовать *advertized.listeners*. Имя хоста для публикации в ZooKeeper для использования клиентами. В средах IaaS может отличаться от интерфейса, к которому привязывается брокер. Если параметр не задан, используется настроенное значение для *host.name*. В противном случае -- значение из *java.net.InetAddress.getCanonicalHostName()*

+ TYPE -- string
+ DEFAULT -- null
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- read-only

**advertised.listeners** -- Слушатели для публикации в ZooKeeper для использования клиентами (если есть отличие от свойства *listeners*). В средах IaaS может отличаться от интерфейса, к которому привязывается брокер. Если параметр не задан, используется значение для *listeners*. В отличие от *listeners* значение "0.0.0.0" недопустимо.

+ TYPE -- string
+ DEFAULT -- null
+ IMPORTANCE -- high
+ DYNAMIC UPDATE MODE -- per-broker

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
