Python 2 & 3 Interpreter для Apache Zeppelin
--------------------------------------------

Настройка конфигурации
^^^^^^^^^^^^^^^^^^^^^^

Для настройки конфигурации **Python** для **Apache Zeppelin** необходимо задать параметры, представленные в таблице.


.. csv-table:: Параметры конфигурации
   :header: "Параметр", "Значение по умолчанию", "Описание"
   :widths: 45, 10, 45

   "zeppelin.python", "python", "Путь к уже установленному бинарному Python (может быть *python2* или *python3*). Если *python* нет в  *$PATH*, можно установить полный каталог (например: */usr/bin/python*)"
   "zeppelin.python.maxResult", "1000", "Максимальное количество строк данных для отображения"


Включение Python Interpreter
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

В блокноте, чтобы включить интерпретатор **Python**, необходимо нажать значок "Gear" и выбрать "Python".


Использование Python Interpreter
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Для выбора интерпретатора **Python** необходимо в параграфе указать *%python*, а затем ввести все команды. Интерпретатор может работать, только если уже установлен *python* (интерпретатор не предоставляет собственные бинарные файлы *python*). Для получения доступа к справке, следует ввести *help()*.


Экосистема Python
^^^^^^^^^^^^^^^^^

+ По умолчанию

По умолчанию *PythonInterpreter* использует команду *python*, определенную в свойстве *zeppelin.python* для запуска процесса *python*. Интерпретатор может использовать все установленные модули (с помощью *pip*, *easy_install* и других)

+ Conda

**Conda** -- это система управления пакетами и экосистемой для *python*. Интерпретатор *%python.conda* позволяет переключаться между средами.

Перечень экосистем:

  :command:`%python.conda`

Активация экосистемы:

  :command:`%python.conda activate [ENVIRONMENT_NAME]`

Деактивация экосистемы:

  :command:`%python.conda deactivate`

+ Докер

Интерпретатор *%python.docker* позволяет *PythonInterpreter* создавать процесс *python* в указанном докер-контейнере. 

Активация экосистемы:

  ::
    
   %python.docker activate [Repository]
   %python.docker activate [Repository:Tag]
   %python.docker activate [Image Id]

Деактивация экосистемы:

  :command:`%python.docker deactivate`

Пример:

  ::
    
   # activate latest tensorflow image as a python environment
   %python.docker activate gcr.io/tensorflow/tensorflow:latest


Использование Zeppelin Dynamic Forms
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Динамическую форму **Zeppelin** можно использовать внутри кода **Python** (см. `подробное описание <https://zeppelin.apache.org/docs/0.7.3/manual/dynamicform.html>`_).

**Zeppelin Dynamic Form** может использоваться только в том случае, если в системе установлена *py4j Python library*. Библиотеку можно установить с помощью *pip install py4j*.

Пример:

  ::
  
   %python
   ### Input form
   print (z.input("f1","defaultValue"))

   ### Select form
   print (z.select("f1",[("o1","1"),("o2","2")],"2"))

   ### Checkbox form
   print("".join(z.checkbox("f3", [("o1","1"), ("o2","2")],["1"])))


Интеграция Matplotlib
^^^^^^^^^^^^^^^^^^^^^

Интерпретатор **Python** может автоматически отображать графики библиотеки *matplotlib* с помощью встроенного модуля *pyplot*:

  ::
  
   %python
   import matplotlib.pyplot as plt
   plt.plot([1, 2, 3])

Это рекомендуемый метод использования *matplotlib* из блокнота **Zeppelin**. Выходные данные этой команды по умолчанию преобразовываются в HTML, используя *%html*. Дополнительную конфигурацию можно выполнить с помощью встроенного метода *z.configure_mpl()*. Например:

  ::
  
   z.configure_mpl(width=400, height=300, fmt='svg')
   plt.plot([1, 2, 3])

В данном примере изображение задается в формате *SVG 400x300*, которое по умолчанию обычно *600x400* и *PNG* соответственно. В дальнейшем можно использовать другую опцию, называемую *angular*, чтобы сделать возможным обновление графика, созданного одним параграфом, непосредственно из другого (выходные данные в таком случае *%angular* вместо *%html*). Эта функция уже доступна в интерпретаторе *pyspark*. 

Если **Zeppelin** не может найти файлы базы данных *matplotlib* (которые обычно находятся в *$ZEPPELIN_HOME/interpreter/lib/python*) в *PYTHONPATH*, то программа автоматически устанавливается в *agg* и инструкции могут иметь ограниченное применение. 

Если не удается загрузить встроенную серверную часть, необходимо использовать *z.show(plt)*:

  :command:`python %python import matplotlib.pyplot as plt plt.figure() (.. ..) z.show(plt) plt.close()`
  
Данная функция *z.show()* может задавать дополнительные параметры для адаптации размеров графика (ширина и высота), а также его выходного формата -- *png* или опционально *svg* (:numref:`Рис.%s.<zeppelin_python_matplotlib>`):

  ::
  
   %python
   z.show(plt, width='50px')
   z.show(plt, height='150px', fmt='svg')



.. _zeppelin_python_matplotlib:

.. figure:: ../imgs/zeppelin_python_matplotlib.*
   :align: center

   Интеграция Matplotlib

 
 Интеграция Pandas
 ^^^^^^^^^^^^^^^^^
 
Система отображения таблиц **Apache Zeppelin** предоставляет встроенные возможности визуализации данных. Интерпретатор **Python** использует его для визуализации **Pandas DataFrames**, аналогично функции *z.show()* интеграциb библиотек *matplotlib* (`Интеграция Matplotlib`_). Например:

  ::
  
   import pandas as pd
   rates = pd.read_csv("bank.csv", sep=";")
   z.show(rates)
 
 
 SQL поверх Pandas DataFrames
 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
 
Существует удобный интерпретатор *%python.sql*, который соответствует опыту **Apache Spark** в **Zeppelin** и позволяет использовать язык **SQL** для запроса **Pandas DataFrames** и визуализации результатов через встроенную систему отображения таблиц `Table Display System <https://zeppelin.apache.org/docs/0.7.3/displaysystem/basicdisplaysystem.html#table>`_.
 
Предварительные запросы:

+ Pandas *pip install pandas*
+ PandaSQL *pip install -U pandasql*
 
В случае, если по умолчанию выбран интерпретатор **Python** (первый в списке интерпретаторов под значком шестеренки), можно его указывать просто *%sql*:

+ Первый параграф:

   ::
   
    import pandas as pd
    rates = pd.read_csv("bank.csv", sep=";")
 
+ Следующий параграф:

   ::
   
    %sql
    SELECT * FROM rates WHERE age < 40
 
В противном случае -- *%python.sql*.
 
 
