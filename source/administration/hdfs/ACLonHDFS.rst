ACL на HDFS
=============


В главе описывается использование списков контроля доступа (**ACL**) в распределенной файловой системе **Hadoop** --
**HDFS**. **ACL** расширяет модель разрешения **HDFS** для поддержки более детального доступа к файлам на основе произвольных
комбинаций пользователей и групп.



Настройка ACL на HDFS
-----------------------

По умолчанию **ACL** отключены и **NameNode** отклоняет все попытки установить их. Например:
::

  <property>
      <name>dfs.namenode.acls.enabled</name>
      <value>true</value>
  </property>

Для включения **ACL** в **HDFS** необходимо в файле *hdfs-site.xml* установить свойству *dfs.namenode.acls.enabled* значение *true*.



Использование команд CLI для создания ACL
-------------------------------------------

В **FsShell** используется две подкоманды: ``setfacl`` и ``getfacl``. Они моделируются после одних и тех же команд **Linux**, но при этом реализуют меньше флагов. Поддержка дополнительных флагов может быть добавлена позже.

Setfacl
^^^^^^^^

Устанавливает **ACL** для файлов и каталогов. Применение:

  ::

   -setfacl [-bkR] {-m|-x} <acl_spec> <path> -setfacl --set <acl_spec> <path>

Функции команды описаны в таблице.

.. csv-table:: Функции команды setfacl
   :header: "Функция", "Описание"
   :widths: 30, 70

   "``-b``", "Удаление всех записей, но с сохранением записей ACL. Записи для пользователей и групп сохраняются для совместимости с разрешениями"
   "``-k``", "Удаление ACL по умолчанию"
   "``-Р``", "Применение операции ко всем файлам и каталогам рекурсивно"
   "``-m``", "Изменение ACL. Новые записи добавляются в ACL, а существующие записи сохраняются"
   "``-x``", "Удаление указанных записей ACL. Все остальные записи ACL сохраняются"
   "``--set``", "Полная замена ACL и сброс всех существующих записей. Функция ``«acl_spec»`` включает записи для пользователей и групп для совместимости с разрешениями"
   "``<acl_spec>``", "Список записей ACL, разделены запятыми"
   "``<path>``", "Путь к файлу или директории для изменения"


Например:
::

  hdfs dfs -setfacl -m user:hadoop:rw- /file
  hdfs dfs -setfacl -x user:hadoop /file
  hdfs dfs -setfacl -b /file
  hdfs dfs -setfacl -k /dir
  hdfs dfs -setfacl --set user::rw-,user:hadoop:rw-,group::r--,other::r-- /file
  hdfs dfs -setfacl -R -m user:hadoop:r-x /dir
  hdfs dfs -setfacl -m default:user:hadoop:r-x /dir

**Код выхода**: при успехе *0* и ненулевое значение при ошибке.


Getfacl
^^^^^^^^

Отображает **ACL** файлов и каталогов. Если каталог имеет **ACL** по умолчанию, ``getfacl`` также его отображает. Применение:

  ::

   -getfacl [-R] <path>

Функции команды описаны в таблице.

.. csv-table:: Функции команды getfacl
   :header: "Функция", "Описание"
   :widths: 30, 70

   "``-R``", "Список ACL всех рекурсивных файлов и каталогов"
   "``<path>``", "Путь к файлу или директории списка"

Например:
::

  hdfs dfs -getfacl /file
  hdfs dfs -getfacl -R /dir

**Код выхода**: при успехе *0* и ненулевое значение при ошибке.
