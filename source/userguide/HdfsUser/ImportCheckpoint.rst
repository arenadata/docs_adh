Import Checkpoint
==================

В случае потери всех копий образов и файлов транзакций в NameNode может быть импортирована последняя контрольная точка. Для этого необходимо:

* Создать пустой каталог, указанный в переменной конфигурации *dfs.namenode.name.dir*;
* Указать местоположение каталога контрольных точек в переменной конфигурации *dfs.namenode.checkpoint.dir*;
* Запустить NameNode с параметром ``-importCheckpoint``.

При этом NameNode загружает контрольную точку из каталога *dfs.namenode.checkpoint.dir*, а затем сохраняет ее в каталог NameNode, заданный в *dfs.namenode.name.dir*. Если в *dfs.namenode.name.dir* содержится допустимый образ, NameNode выдает ошибку. NameNode проверяет консистентность образа в *dfs.namenode.checkpoint.dir*, но не изменяет его каким-либо образом.

Пример использования приведен в главе :ref:`Checkpoint Node <usage_checkpoint>`.
