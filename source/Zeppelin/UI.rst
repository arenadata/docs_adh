Интерфейс Apache Zeppelin
-------------------------

+ `Главная страница`_
+ `Меню`_
+ `Набор инструментов`_


Главная страница
^^^^^^^^^^^^^^^^

При первом подключении к **Apache Zeppelin** открывается главная страница, представленная на :numref:`Рис.%s.<zeppelin_ui_main-home>`

.. _zeppelin_ui_main-home:

.. figure:: ../imgs/zeppelin_ui_main-home.*
   :align: center

   Главная страница Apache Zeppelin


В левой части страницы перечислены все существующие заметки. По умолчанию заметки сохраняются в папке *$ZEPPELIN_HOME/notebook*. 

Существует возможность фильтровать заметки по имени, используя форму ввода текста. Также можно создавать новые, обновлять список существующих заметок (в случае если они вручную скопированы в папку *$ZEPPELIN_HOME/notebook*) и импортировать их.

При нажатии кнопки "Import Note" открывается новое диалоговое окно (:numref:`Рис.%s.<zeppelin_ui_import-note>`). Заметку можно импортировать с локального диска или из удаленного места по указанному URL-адресу. Название заметки можно изменить.

.. _zeppelin_ui_import-note:

.. figure:: ../imgs/zeppelin_ui_import-note.*
   :align: center

   Импорт новой заметки


Меню
^^^^^

Панель меню "Notebook" предлагает почти те же функции, что и раздел управления заметками на главной странице (:numref:`Рис.%s.<zeppelin_ui_main-home>`). В раскрывающемся контекстном меню можно выбрать (:numref:`Рис.%s.<zeppelin_ui_notebook>`):

+ Открыть выбранную заметку;
+ Фильтр заметок по имени;
+ Создать новую заметку.


.. _zeppelin_ui_notebook:

.. figure:: ../imgs/zeppelin_ui_notebook.*
   :align: center

   Панель меню Notebook


Меню настроек дает доступ к конфигурации и отображает информацию о **Zeppelin** (:numref:`Рис.%s.<zeppelin_ui_settings>`). При использовании настроек по умолчанию имя пользователя задается как *anonymous*. Конфигурация аутентификации описана в разделе `Аутентификация Apache Shiro для Apache Zeppelin <http://docs.arenadata.io/adh/v1.4/Zeppelin/Authentication.html>`_.

.. _zeppelin_ui_settings:

.. figure:: ../imgs/zeppelin_ui_settings.*
   :align: center

   Меню настроек


По ссылке "About Zeppelin" можно получить информацию об установленной версии **Apache Zeppelin** (:numref:`Рис.%s.<zeppelin_ui_about>`).

.. _zeppelin_ui_about:

.. figure:: ../imgs/zeppelin_ui_about.*
   :align: center

   About Zeppelin


По ссылке "Interpreter" можно выполнить следующие функции (:numref:`Рис.%s.<zeppelin_ui_interpreters>`):

+ Настроить существующий интерпретатор;
+ Добавить/удалить интерпретатор.


.. _zeppelin_ui_interpreters:

.. figure:: ../imgs/zeppelin_ui_interpreters.*
   :align: center

   Управление интерпретаторами


Ссылка "Credential" позволяет сохранять учетные данные для источников данных, которые передаются интерпретаторам (:numref:`Рис.%s.<zeppelin_ui_credentials>`).


.. _zeppelin_ui_credentials:

.. figure:: ../imgs/zeppelin_ui_credentials.*
   :align: center

   Учетные данные


Ссылка "Configuration" отображает всю конфигурацию **Apache Zeppelin**, которая задана в файле конфигурации *$ZEPPELIN_HOME/conf/zeppelin-site.xml* (:numref:`Рис.%s.<zeppelin_ui_configurations>`).


.. _zeppelin_ui_configurations:

.. figure:: ../imgs/zeppelin_ui_configurations.*
   :align: center

   Конфигурация Apache Zeppelin



Набор инструментов
^^^^^^^^^^^^^^^^^^


