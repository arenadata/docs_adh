Хранилище "ключ-значение"
-------------------------

**Репликация и секционирование данных в памяти кластера**

+ `Data Grid`_
+ `Запросы к данным в кэше`_
+ `Режимы работы кэша`_
+ `Основные и бекап копии`_


Data Grid
^^^^^^^^^

Платформа **Data Grid** была построена с нуля с концепцией горизонтального масштабирования и возможностью добавления узлов по требованию в режиме реального времени; платформа была разработана для линейного масштабирования до сотен узлов с сильной семантикой для маршрутизации данных с целью уменьшения избыточного шума данных.

**Data Grid** -- это *in-memory distributed key-value store* (распределенное хранилище ключей и значений в памяти), которое можно рассматривать как распределенную секционированную хэш-карту с каждым узлом кластера, владеющим частью общих данных. Таким образом, чем больше добавляется узлов кластера, тем больше данных можно кэшировать.

В отличие от других хранилищ "ключ-значение", **Grid** определяет местоположение данных с помощью алгоритма хэширования. Каждый клиент может определить, к какому узлу принадлежит ключ, подключив его к функции хэширования, без необходимости каких-либо mapping servers или name nodes.

**Data Grid** поддерживает локальные, реплицированные и секционированные наборы данных и позволяет свободно выполнять перекрестные запросы между этими наборами данных, используя стандартный синтаксис **SQL**. 

**Grid** поддерживает стандартный **SQL** для запросов данных, включая поддержку распределенных SQL-соединений.

**Data Grid** -- молниеносная платформа, являющаяся одной из самых быстрых по реализации транзакции данных в кластере сегодня.

**Data Grid** реализует спецификацию **JCache** (**JSR 107**).

.. important:: Пока кластер работает, Grid гарантирует, что данные между различными узлами кластера всегда будут оставаться неизменными независимо от сбоев или изменений топологии



Запросы к данным в кэше
^^^^^^^^^^^^^^^^^^^^^^^

**Grid** поддерживает очень элегантный **API** запросов с поддержкой Scan Queries на основе предикатов, запросов **SQL** (совместимых с **ANSI-99**) и текстовых запросов. Для SQL-запросов **Grid** поддерживает индексирование в памяти, поэтому все поисковые запросы чрезвычайно быстры.

**Grid** также обеспечивает поддержку пользовательской индексации с помощью классов *IndexingSpi* и *SpiQuery*.

*IgniteCache* имеет несколько методов запросов, все из которых получают некоторый подкласс класса *Query* и возвращают *QueryCursor*.

Абстрактный класс *Query* представляет собой абстрактный разбитый на страницы запрос для выполнения в распределенном кэше. Размер страницы можно задать с помощью метода *Query.setPageSize(...)*, значение по умолчанию которого -- *1024*.

*QueryCursor* представляет собой набор результатов запроса и позволяет проводить прозрачную постраничную итерацию. Всякий раз, когда пользователь начинает итерацию последней страницы, в фоновом режиме автоматически запрашивается следующая страница. В случаях, когда разбиение на страницы не требуется, можно использовать метод *QueryCursor.getAll()*, который извлекает весь результат запроса и хранит его в коллекции.

.. important:: Курсоры закрываются автоматически при вызове метода *QueryCursor.getAll()*. При итерации по курсору в цикле или при прямом получении *Iterator*, необходимо явно закрыть курсор или использовать синтаксис *AutoCloseable*

Запросы сканирования позволяют запрашивать кэш в распределенной форме на основе определенного пользователем предиката:

+ scan:

  ::
  
   IgniteCache<Long, Person> cache = ignite.cache("mycache");
   
   // Find only persons earning more than 1,000.
   try (QueryCursor cursor = cache.query(new ScanQuery((k, p) -> p.getSalary() > 1000)) {
     for (Person p : cursor)
       System.out.println(p.toString());
   }

+ java7 scan:

  ::
  
   IgniteCache<Long, Person> cache = ignite.cache("mycache");
   
   // Find only persons earning more than 1,000.
   IgniteBiPredicate<Long, Person> filter = new IgniteBiPredicate<>() {
     @Override public boolean apply(Long key, Perons p) {
     	return p.getSalary() > 1000;
   	}
   };
   
   try (QueryCursor cursor = cache.query(new ScanQuery(filter)) {
     for (Person p : cursor)
       System.out.println(p.toString());
   }


Запросы сканирования также поддерживают дополнительное закрытие трансформатора, которое позволяет преобразовать запись на узле сервера перед ее отправкой клиенту. Это полезно, например, когда требуется извлечь только несколько полей из большого объекта и минимизировать сетевой трафик. Пример ниже показывает, как извлекать только ключи и не отправлять объекты.

+ scan with transformer:

  ::
  
   IgniteCache<Long, Person> cache = ignite.cache("mycache");
   
   // Get only keys for persons earning more than 1,000.
   List<Long> keys = cache.query(new ScanQuery<Long, Person>(
       (k, p) -> p.getSalary() > 1000), // Remote filter.
       Cache.Entry::getKey              // Transformer.
   ).getAll();

+ scan with transformer (Java 7):

  ::
  
   IgniteCache<Long, Person> cache = ignite.cache("mycache");
   
   // Get only keys for persons earning more than 1,000.
   List<Long> keys = cache.query(new ScanQuery<>(
       // Remote filter.
       new IgniteBiPredicate<Long, Person>() {
           @Override public boolean apply(Long k, Person p) {
               return p.getSalary() > 1000;
           }
       }),
       // Transformer.
       new IgniteClosure<Cache.Entry<Long, Person>, Long>() {
           @Override public Long apply(Cache.Entry<Long, Person> e) {
               return e.getKey();
           }
       }
   ).getAll();


SQL-запросы в **Grid** рассматриваются в разделе документации `SQL <https://apacheignite-sql.readme.io/docs/java-sql-api>`_.

**Grid** также поддерживает текстовые запросы, основанные на **Lucene** индексировании.

+ text query:

  ::
  
   IgniteCache<Long, Person> cache = ignite.cache("mycache");
   
   // Query for all people with "Master Degree" in their resumes.
   TextQuery txt = new TextQuery(Person.class, "Master Degree");
   
   try (QueryCursor<Entry<Long, Person>> masters = cache.query(txt)) {
     for (Entry<Long, Person> e : cursor)
       System.out.println(e.getValue().toString());
   }


Настройка запросов по аннотациям
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Индексы могут быть сконфигурированы из кода с помощью *@QuerySqlField* аннотаций. Чтобы сообщить **Grid**, какие типы следует индексировать, пары ключ-значение можно передать методу *CacheConfiguration.setIndexedTypes(MyKey.class, MyValue.class)*. Данный метод принимает только пары типов -- один для класса ключей и другой для класса значений.

+ Java:

  ::
   
   public class Person implements Serializable {
     /** Person ID (indexed). */
     @QuerySqlField(index = true)
     private long id;
   
     /** Organization ID (indexed). */
     @QuerySqlField(index = true)
     private long orgId;
   
     /** First name (not-indexed). */
     @QuerySqlField
     private String firstName;
   
     /** Last name (not indexed). */
     @QuerySqlField
     private String lastName;
   
     /** Resume text (create LUCENE-based TEXT index for this field). */
     @QueryTextField
     private String resume;
   
     /** Salary (indexed). */
     @QuerySqlField(index = true)
     private double salary;
     
     ...
   }


Настройка запросов с помощью QueryEntity
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Индексы и поля также можно настроить с помощью *org.apache.ignite.cache.QueryEntity*, удобным для конфигурации **XML** с **Spring**. Это эквивалентно использованию аннотации *@QuerySqlField*, поскольку аннотации классов преобразуются в сущности внутреннего запроса.

+ XML:

  ::
  
   <bean class="org.apache.ignite.configuration.CacheConfiguration">
       <property name="name" value="mycache"/>
       <!-- Configure query entities -->
       <property name="queryEntities">
           <list>
               <bean class="org.apache.ignite.cache.QueryEntity">
                   <property name="keyType" value="java.lang.Long"/>
                   <property name="valueType" value="org.apache.ignite.examples.Person"/>
   
                   <property name="fields">
                       <map>
                           <entry key="id" value="java.lang.Long"/>
                           <entry key="orgId" value="java.lang.Long"/>
                           <entry key="firstName" value="java.lang.String"/>
                           <entry key="lastName" value="java.lang.String"/>
                           <entry key="resume" value="java.lang.String"/>
                           <entry key="salary" value="java.lang.Double"/>
                       </map>
                   </property>
   
                   <property name="indexes">
                       <list>
                           <bean class="org.apache.ignite.cache.QueryIndex">
                               <constructor-arg value="id"/>
                           </bean>
                           <bean class="org.apache.ignite.cache.QueryIndex">
                               <constructor-arg value="orgId"/>
                           </bean>
                           <bean class="org.apache.ignite.cache.QueryIndex">
                               <constructor-arg value="salary"/>
                           </bean>
                       </list>
                   </property>
               </bean>
           </list>
       </property>
   </bean>

+ Java:

  ::
  
   CacheConfiguration<Long, Person> cacheCfg = new CacheConfiguration<>();
   ...
   cacheCfg.setName("mycache");
   
   // Setting up query entity.
   QueryEntity queryEntity = new QueryEntity();
   
   queryEntity.setKeyType(Long.class.getName());
   queryEntity.setValueType(Person.class.getName());
   
   // Listing query fields.
   LinkedHashMap<String, String> fields = new LinkedHashMap();
   
   fields.put("id", Long.class.getName());
   fields.put("orgId", Long.class.getName());
   fields.put("firstName", String.class.getName());
   fields.put("lastName", String.class.getName());
   fields.put("resume", String.class.getName());
   fields.put("salary", Double.class.getName());
   
   queryEntity.setFields(fields);
   
   // Listing indexes.
   Collection<QueryIndex> indexes = new ArrayList<>(3);
   
   indexes.add(new QueryIndex("id"));
   indexes.add(new QueryIndex("orgId"));
   indexes.add(new QueryIndex("salary"));
   
   queryEntity.setIndexes(indexes);
   ...
   cacheCfg.setQueryEntities(Arrays.asList(queryEntity));
   ...



Режимы работы кэша
^^^^^^^^^^^^^^^^^^

Основные и бекап копии
^^^^^^^^^^^^^^^^^^^^^^
