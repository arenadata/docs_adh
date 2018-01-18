Интерфейс Apache Zeppelin
-------------------------

+ `Главная страница`_
+ `Меню`_
+ `Набор инструментов`_


Главная страница
^^^^^^^^^^^^^^^^

При первом подключении к **Apache Zeppelin** открывается главная страница, представленная на :numref:`Рис.%s.<zeppelin_ui_main-home>`.

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


Это меню дает вам доступ к настройкам и отображает информацию о Zeppelin. Имя пользователя задано как *anonymous*, если вы используете конфигурацию по умолчанию. Если вы хотите настроить аутентификацию, см. Широ аутентификацию.

This menu gives you access to settings and displays information about Zeppelin. User name is set to anonymous if you use default shiro configuration. If you want to set up authentification, see Shiro authentication.



Набор инструментов
^^^^^^^^^^^^^^^^^^


