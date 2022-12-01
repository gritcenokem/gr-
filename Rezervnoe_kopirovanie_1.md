#  Резервное копирование
### Задание 1
Создайте базу данных и таблицу в ней с несколькими строками.
```sh
=> CREATE DATABASE backup_overview;
=> \c backup_overview
=> CREATE TABLE t(n integer);
=> INSERT INTO t VALUES (1), (2), (3);
```
<br>
Результат:
![alt-текст](https://sun9-64.userapi.com/impg/XyQg_-ZbFt7i1_v9_Z34-_dG-oevGZn30dCacw/KH5e45BqR9Q.jpg?size=434x100&quality=96&sign=ad049ece4b50a85fa70a09c254b413c8&type=album "Текст заголовка логотипа 1")<br>
###  Задание 2
Сделайте логическую копию базы данных с помощью утилиты pg_dump. Удалите базу данных и восстановите ее из сделанной копии.
Создаем резервную копию и удаляем базу данных и восстанавливаем ее из копии:
```sh
=> \c postgres
=> DROP DATABASE backup_overview;
student$ psql -f ~/backup_overview.dump
=> \c backup_overview
=> SELECT * FROM t;
```
Результат:
![alt-текст](https://sun9-34.userapi.com/impg/u4pvP15DRlphkMCyhEe6kYcSOqDazxazoF5ilg/yy23TtuVC8g.jpg?size=614x392&quality=96&sign=ef80fadecd756dc9924b1d53ffbc925e&type=album "Текст заголовка логотипа 1")
![alt-текст](https://sun9-82.userapi.com/impg/AsDj_iJJGgsbnjDBTCc73U1i6gp8iWeMr0gaKw/8JLcYY4ZT6o.jpg?size=521x164&quality=96&sign=9559ab45b1b490068ae950ad7205aa4b&type=album "Текст заголовка логотипа 1")<br>

###  Задание 3
Сделайте автономную физическую резервную копию кластера с помощью утилиты pg_basebackup. Измените таблицу. Восстановите новый кластер из сделанной резервной копии и проверьте, что база данных не содержит более поздних изменений
```sh
student$ sudo rm -rf /home/student/basebackup
student$ pg_basebackup --pgdata=/home/student/basebackup
---Убеждаемся, что второй сервер остановлен, и выкладываем резервную копию:---
student$ sudo pg_ctlcluster 13 replica status
student$ sudo rm -rf /var/lib/postgresql/13/replica
student$ sudo mv /home/student/basebackup/ /var/lib/postgresql/13/replica
student$ sudo chown -R postgres:postgres /var/lib/postgresql/13/replica
---Изменяем таблицу:---
=> DELETE FROM t;
---Запускаем сервер из резервной копии:---
student$ sudo pg_ctlcluster 13 replica start
student$ /usr/lib/postgresql/13/bin/psql -p 5433 -d backup_overview
=> SELECT * FROM t;
```
<br>
Результат:
![alt-текст](https://sun9-62.userapi.com/impg/vUDzS6XCO1_u56g3MNnGxoT9IV-9CccCgtkyPg/HNIc-0iIO6A.jpg?size=435x157&quality=96&sign=c7438097cff8c69f2439b7e4cb23c18a&type=album "Текст заголовка логотипа 1")<br>

###  Практика +
Настройте аутентификацию, лишенную этого недостатка.
Убедитесь в правильности изменений
Сохраним исходный файл настроек:
```sh
student$ sudo cp -n /etc/postgresql/13/main/pg_hba.conf ~/pg_hba.conf.orig
```
Будем управлять аутентификацией пользователей, добавляя их в группу locals.
Запишем файл pg_hba.conf с нуля:
```sh
student$ sudo tee /etc/postgresql/13/main/pg_hba.conf << EOF
local all student trust
local all +locals trust
EOF
student$ sudo pg_ctlcluster 13 main reload
=> CREATE ROLE locals;
```
Проверка
```sh
=> CREATE ROLE alice LOGIN;
=> GRANT locals TO alice;
=> CREATE ROLE bob LOGIN;
student$ psql "dbname=student user=alice" -c
student$ psql "dbname=student user=bob" -c "\conninfo""\conninfo"
=> GRANT locals TO bob;
student$ psql "dbname=student user=bob" -c "\conninfo"
---Восстановление исходных настроек---
student$ sudo cp ~/pg_hba.conf.orig /etc/postgresql/13/main/pg_hba.conf
student$ sudo pg_ctlcluster 13 main reload
```
<br>



