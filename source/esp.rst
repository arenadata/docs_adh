Перевод
========

.. important:: Документ в стадии разработки. Не использовать приведенную информацию

ADCM
---------

1) Страница сайта `ссылка <https://arenadata.tech/products/adcm/>`_ 

**Arenadata Clúster Manager** (**ADCM**) proporciona una manera eficiente de implementar y administrar todos los servicios de datos de la empresa, independientemente de la infraestructura utilizada, en la nube, on-premise o como servicios de PaaS. Al dividir la plataforma de datos en dos capas lógicamente no vinculadas -- la capa de infraestructura y la capa de aplicaciones -- el explotación de toda la plataforma se vuelve predecible, flexible y repetible.

**ADCM** tiene una estructura modular -- agregue la funcionalidad que necesita para trabajar con aplicaciones y equipos, según sea necesario. Los módulos tienen un formato abierto que le permite crear sus propios módulos para aplicaciones altamente especializadas y propietarias.

Gracias a la avanzada de REST API, otros sistemas pueden usar **ADCM** para automatizar el trabajo con su plataforma de datos.

**ADCM** viene en forma de docker-image, por lo que su implementación y uso es muy simple.

Funcionalidad de la aplicación:

* Despliegue;
* Actualización;
* Gestión (Administración);
* Vigilancia (Monitoring);
* Integración de componentes.

Aplicaciones soportadas (:numref:`Рис.%s.<ADCM_aplicaciones>`).

.. _ADCM_aplicaciones:

.. figure:: imgs/ADCM_aplicaciones.png
   :align: center

   Aplicaciones soportadas


Proveedores de nube soportados (:numref:`Рис.%s.<ADCM_proveedores>`).

.. _ADCM_proveedores:

.. figure:: imgs/ADCM_proveedores.png
   :align: center

   Proveedores de nube soportados
   

Funcionalidad de infraestructura:

* Creacion / eliminacion MV;
* Trabajo con hosts físicos;
* Preparación de MV / hosts;
* Vigilancia (Monitoring);
* Gestión (Administración) de usuarios;
* Gestión (Administración) de acceso.

Demostración: `URL <https://arenadata.tech/products/adcm/>`_.



Diapositiva 7
^^^^^^^^^^^^^^^

.. _slide7:

.. figure:: imgs/slide7.png
   :align: center

   Moderna plataforma de datos



Diapositiva 15
^^^^^^^^^^^^^^^^

.. _slide15:

.. figure:: imgs/slide15.png
   :align: center

   Separación de componentes e infraestructura



Слайд 22
^^^^^^^^^^^^

4) Слайд 22 Обзорной презентации



Слайд 23
^^^^^^^^^^

5) Слайд 23 Обзорной презентации




ADS
---------

Arenadata Streaming
^^^^^^^^^^^^^^^^^^^^^^

6) https://arenadata.tech/products/ads/

**Arenadata Streaming** (**ADS**) -- es una solución eficiente escalable y tolerante a fallos para el procesamiento de datos en tiempo real, adaptada para uso corporativo y construida sobre la base de Apache Kafka y Apache Nifi -- proyectos con código abierto.

Una de las características de la implementación de la plataforma es el uso de tecnología similar a los registros de transacciones utilizados en los sistemas de administración de bases de datos.

La plataforma de transmisión tiene tres capacidades clave:

* Publicación y suscripción a una transmisión de datos similar a una cola de mensajes o un sistema de mensajería corporativa;
* Almacenar registros de forma tolerante a fallos.
* Procesamiento de subprocesos de registros a medida que ocurren.

ADS se utiliza normalmente para dos amplias clases de aplicaciones:

* Creación de un canal para la transmisión de datos en tiempo real para garantizar un intercambio de datos confiable entre sistemas y aplicaciones;
* Creación de aplicaciones para la transmisión de datos en tiempo real para convertirlas e interactuar con otros flujos.

Ventajas principales

Punto de acceso único:

* Utilice como bus corporativo intercambio de datos para todas sus aplicaciones.

Una forma fácil, segura y confiable de controlar el flujo de datos:

* Recopile grandes flujos de datos de forma segura y gestionarlos efectivamente en tiempo real.

Políticas de seguridad:

* Crea flujos de datos con soporte para derechos de acceso.

Desarrollo rápido y continuo:

* Desarrolle aplicaciones analíticas de flujo en cuestión de minutos en tiempo real sin una sola línea de código.

ADS contiene todos los componentes necesarios para recopilar, analizar y procesar datos en tiempo real, proporciona almacenamiento y transferencia en la semántica "exactly-once delivery" de una manera segura y tolerante a fallos, proporcionando una interfaz conveniente para la administración y el desarrollo.

Características técnicas

Tolerancia a fallos:

* La arquitectura de la solución permite garantizar la consistencia en la transmisión de datos en tiempo real.

Escalabilidad:

* Agregar nuevos servidores al clúster según sea necesario.

Distribución:

* Posibilidad de construir una infraestructura geo-distribuida.

Equipo disponible:

* Funciona en cualquier hardware compatible con x86. Obtenga los requisitos del sistema recomendados por nuestros especialistas.

Tiempo real:

* Control de flujos, adición y configuración de fuentes de datos en tiempo real.

Seguridad:

* Mecanismos flexibles de control de acceso a datos.

Integración:

* Conectores a varios sistemas: Elasticsearch, SAP HANA, Vertica, Couchbase, Cassandra, CouchDB, IBM MQ, etc. Un amplio conjunto de API para la integración con otros sistemas externos.

Simplicidad y flexibilidad:

* Le permite crear el workflow con la ayuda del interfaz gráfico o desarrollar sus propias aplicaciones utilizando el SDK para mejorar la eficiencia del trabajo.


docs.arenadata
^^^^^^^^^^^^^^^^

7) https://docs.arenadata.io/ads/

Una solución efectiva para el procesamiento de datos en tiempo real

**Arenadata Streaming** (**ADS**) -- 

платформа потоковых операций, построенная на базе Apache Kafka и Apache Nifi, проектов с открытым исходным кодом, адаптированное для корпоративного использования. ADS содержит в себе все необходимые компоненты для сбора, анализа и обработки данных в режиме реального времени, обеспечивает хранение и передачу в семантике «exactly-once delivery» безопасным и отказоустойчивым способом, предоставляя удобный интерфейс для администрирования и разработки. Также платформа может выступать в качестве корпоративной шины данных и ETL-инструмента
una plataforma de flujo de trabajo basada en Apache Kafka y Apache Nifi, proyectos de código abierto, adaptados para uso corporativo. ADS contiene todos los componentes necesarios para recopilar, analizar y procesar datos en tiempo real, proporciona almacenamiento y transferencia en la semántica "exactly-once delivery" de una manera segura tolerante a fallas

proporcionando una interfaz conveniente para la administración y el desarrollo. La plataforma también puede actuar como un bus de datos corporativo y una herramienta ETL.

ADS contiene todos los componentes necesarios para recopilar, analizar y procesar datos en tiempo real, proporciona almacenamiento y transferencia en la semántica "exactly-once delivery" de una manera segura y tolerante a errores, proporcionando una interfaz fácil de usar para la administración y el desarrollo. Además, la plataforma puede actuar como un bus de datos corporativo y una herramienta ETL





+	Можно добавить отсюда https://docs.arenadata.io/ads/Intro/index.html
+	Слайд 21 Обзорной презентации
+	Дополнение https://docs.arenadata.io/ads/Architecture/intro.html





ADB
----

docs.arenadata
^^^^^^^^^^^^^^^^^

8) https://arenadata.tech/products/db/ (аналогично тут https://docs.arenadata.io/adb/)




+	Возможно это https://docs.arenadata.io/adb/best_practices/intro.html
+	Слайд 19 Обзорной презентации




Términos comúnmente utilizados
--------------------------------

+ **Clúster** -- кластер -- se aplica a los conjuntos o conglomerados de ordenadores unidos entre sí normalmente por una red de alta velocidad y que se comportan como si fuesen una única computadora.

+ **Servidor** -- сервер
+ **Máquina virtual** (MV) -- виртуальная машина (ВМ)
+ **Sistema operativo** (SO) -- операционная система (ОС)
+ **Memoria RAM** -- оперативная память
+ **Plataforma de datos** -- платформа данных
+ **Almacenamiento** -- хранение
+ **Gestión (Administración)** -- управление
+ **Vigilancia (Monitoring)** -- мониторинг
+ **Para administrar componentes** -- для управления компонентами (`ссылка на использование <https://docs.vmware.com/es/VMware-vSphere/6.5/com.vmware.vsphere.troubleshooting.doc/GUID-AD3FB0FB-7D88-4595-B0BC-D3C3291D8C18.html>`_)

+ **Consola** -- консоль
+ **Host** (hosts) -- хост
+ **Nodo** -- узел -- un nodo es un punto de intersección o unión de varios elementos que confluyen en el mismo lugar.

+ **Un almacén de datos** -- хранилище данных
+ **Los data warehouse** -- хранилище данных
+ **Indicadores de gestión** -- индикаторы управления (`ссылка на использование <https://es.wikipedia.org/wiki/Almac%C3%A9n_de_datos>`_)
+ **Los metadatos** -- метаданные

+ **Extracción**. Acción de obtener la información deseada a partir de los datos almacenados en fuentes externas. -- Извлечение. Действие получения нужной информации из данных, хранящихся во внешних источниках.

+ **Transformación**. Cualquier operación realizada sobre los datos para que puedan ser cargados en el data warehouse o se puedan migrar de éste a otra base de datos. -- Преобразование. Любые операции с данными, которые могут быть загружены в хранилище данных или перенесены из него в другую базу данных.

+ **Carga**. Consiste en almacenar los datos en la base de datos final, por ejemplo el almacén de datos objetivo normal. -- Загрузка. Заключается в хранении данных в конечной базе данных, например, в обычном хранилище данных.

+ **Diseño de un almacén de datos**

Para construir un Data Warehouse se necesitan herramientas para ayudar a la migración y a la transformación de los datos hacia el almacén. Una vez construido, se requieren medios para manejar grandes volúmenes de información. Se diseña su arquitectura dependiendo de la estructura interna de los datos del almacén y especialmente del tipo de consultas a realizar. Con este criterio los datos deben ser repartidos entre numerosos data marts.

Для построения хранилища данных необходимы инструменты для миграции и преобразования данных в хранилище. После создания средства требуются для обработки больших объемов информации. Архитектура хранилища разрабатывается в зависимости от внутренней структуры данных хранилища и, в частности, от типа запросов. С этим критерием данные должны быть распределены по многочисленным витринам данных.

+ **Tipo de consultas** -- тип запросов

+ **Distribuir, repartir** -- распределять

+ **Data marts** -- витрины данных
+ **Las bases de datos** -- базы данных
+ **Bus de datos** -- шина данных -- El bus de datos permite el intercambio de datos entre la CPU y el resto de unidades. Ademas, controla el uso y acceso a el bus de datos y el bus de direcciones. -- Шина данных позволяет обмениваться данными между процессором и остальными устройствами. Кроме того, контролирует использование и доступ к шине данных и адресной шине.

+ **Una red de computadoras** (también llamada red de ordenadores, red de comunicaciones de datos o red informática) es un conjunto de equipos nodos y software conectados entre sí por medio de dispositivos físicos o inalámbricos que envían y reciben impulsos eléctricos, ondas electromagnéticas o cualquier otro medio para el transporte de datos, con la finalidad de compartir información, recursos y ofrecer servicios.

+ **CPU**, del inglés: central processing unit -- La unidad central de procesamiento o unidad de procesamiento central (conocida por las siglas CPU, del inglés: central processing unit), es el hardware dentro de un ordenador u otros dispositivos programables, que interpreta las instrucciones de un programa informático mediante la realización de las operaciones básicas aritméticas, lógicas y de entrada/salida del sistema.

+ La memoria de acceso aleatorio (Random Access Memory, RAM) se utiliza como memoria de trabajo de computadoras y otros dispositivos para el sistema operativo, los programas y la mayor parte del software. En la RAM se cargan todas las instrucciones que ejecuta la unidad central de procesamiento (procesador) y otras unidades del computador, además de contener los datos que manipulan los distintos programas. -- Оперативная память (Random Access Memory, RAM) используется в качестве рабочей памяти компьютеров и других устройств для операционной системы (ОС) и большинства программ. В RAM загружаются все инструкции, выполняемые центральным процессором (ЦП, CPU) и другими дисками компьютера, а также данные, обрабатываемые различными программами.


