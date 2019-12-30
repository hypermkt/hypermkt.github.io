---
layout: post
title: my.cnfのread_onlyについて調べた
post_id: 550
categories: 
- MySQL
---

## 概要


某所でMySQLのスレーブサーバーを構築することになり、友人から
read_only権限について教えてもらったので改めて自分で調べなおしてみた。


## read_onlyとは？



*  MySQLのスレーブサーバーでは
my.cnfで
read_only設定をするとSUPER権限を持つアカウント以外の更新クエリーは実行できなくなる


### 疑問



*  rootアカウントが書き込み権限を持つアカウントを発行しようとするとどうなるのか
予測1. 作成できない
予測2. SUPER権限有りで作成できる


## 検証環境



*  Vagrant


*  CentOS release 6.5 (Final)


*  Server version: 5.5.46 MySQL Community Server (GPL)


## テスト環境を用意した



### 初期状態



mysql> select * from user\G
*  ************************** 1. row ***************************
                  Host: localhost
                  User: root
              Password:
           Select_priv: Y
           Insert_priv: Y
           Update_priv: Y
           Delete_priv: Y
           Create_priv: Y
             Drop_priv: Y
           Reload_priv: Y
         Shutdown_priv: Y
          Process_priv: Y
             File_priv: Y
            Grant_priv: Y
       References_priv: Y
            Index_priv: Y
            Alter_priv: Y
          Show_db_priv: Y
            Super_priv: Y
 Create_tmp_table_priv: Y
      Lock_tables_priv: Y
          Execute_priv: Y
       Repl_slave_priv: Y
      Repl_client_priv: Y
      Create_view_priv: Y
        Show_view_priv: Y
   Create_routine_priv: Y
    Alter_routine_priv: Y
      Create_user_priv: Y
            Event_priv: Y
          Trigger_priv: Y
Create_tablespace_priv: Y
              ssl_type:
            ssl_cipher:
           x509_issuer:
          x509_subject:
         max_questions: 0
           max_updates: 0
       max_connections: 0
  max_user_connections: 0
                plugin:
 authentication_string:
1 row in set (0.00 sec)

mysql> select * from db\G
*  ************************** 1. row ***************************
                 Host: localhost
                   Db: root
                 User: root
          Select_priv: Y
          Insert_priv: Y
          Update_priv: Y
          Delete_priv: Y
          Create_priv: Y
            Drop_priv: Y
           Grant_priv: N
      References_priv: Y
           Index_priv: Y
           Alter_priv: Y
Create_tmp_table_priv: Y
     Lock_tables_priv: Y
     Create_view_priv: Y
       Show_view_priv: Y
  Create_routine_priv: Y
   Alter_routine_priv: Y
         Execute_priv: Y
           Event_priv: Y
         Trigger_priv: Y
1 row in set (0.00 sec)

mysql>


### grant all privilegesのアカウントを作成する


この時点では
test_userアカウントは
hogeデータベースに対して全権限があるとしてアカウントは作成された。ここは普通なんだね。


GRANT ALL PRIVILEGES ON hoge.* TO test_user@localhost;
SET PASSWORD FOR 'test_user'@'localhost' = PASSWORD('fugafuga');
flush privileges;


mysql> GRANT ALL PRIVILEGES ON hoge.* TO test_user@localhost;
Query OK, 0 rows affected (0.00 sec)

mysql> SET PASSWORD FOR 'test_user'@'localhost' = PASSWORD('fugafuga');
Query OK, 0 rows affected (0.00 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from user\G
*  ************************** 1. row ***************************
                  Host: localhost
                  User: root
              Password:
           Select_priv: Y
           Insert_priv: Y
           Update_priv: Y
           Delete_priv: Y
           Create_priv: Y
             Drop_priv: Y
           Reload_priv: Y
         Shutdown_priv: Y
          Process_priv: Y
             File_priv: Y
            Grant_priv: Y
       References_priv: Y
            Index_priv: Y
            Alter_priv: Y
          Show_db_priv: Y
            Super_priv: Y
 Create_tmp_table_priv: Y
      Lock_tables_priv: Y
          Execute_priv: Y
       Repl_slave_priv: Y
      Repl_client_priv: Y
      Create_view_priv: Y
        Show_view_priv: Y
   Create_routine_priv: Y
    Alter_routine_priv: Y
      Create_user_priv: Y
            Event_priv: Y
          Trigger_priv: Y
Create_tablespace_priv: Y
              ssl_type:
            ssl_cipher:
           x509_issuer:
          x509_subject:
         max_questions: 0
           max_updates: 0
       max_connections: 0
  max_user_connections: 0
                plugin:
 authentication_string:
*  ************************** 2. row ***************************
                  Host: localhost
                  User: test_user
              Password: *574991E975571281C22C99705978B14740B142B7
           Select_priv: N
           Insert_priv: N
           Update_priv: N
           Delete_priv: N
           Create_priv: N
             Drop_priv: N
           Reload_priv: N
         Shutdown_priv: N
          Process_priv: N
             File_priv: N
            Grant_priv: N
       References_priv: N
            Index_priv: N
            Alter_priv: N
          Show_db_priv: N
            Super_priv: N
 Create_tmp_table_priv: N
      Lock_tables_priv: N
          Execute_priv: N
       Repl_slave_priv: N
      Repl_client_priv: N
      Create_view_priv: N
        Show_view_priv: N
   Create_routine_priv: N
    Alter_routine_priv: N
      Create_user_priv: N
            Event_priv: N
          Trigger_priv: N
Create_tablespace_priv: N
              ssl_type:
            ssl_cipher:
           x509_issuer:
          x509_subject:
         max_questions: 0
           max_updates: 0
       max_connections: 0
  max_user_connections: 0
                plugin:
 authentication_string: NULL
2 rows in set (0.00 sec)

mysql> select * from db\G
*  ************************** 1. row ***************************
                 Host: localhost
                   Db: hoge
                 User: test_user
          Select_priv: Y
          Insert_priv: Y
          Update_priv: Y
          Delete_priv: Y
          Create_priv: Y
            Drop_priv: Y
           Grant_priv: N
      References_priv: Y
           Index_priv: Y
           Alter_priv: Y
Create_tmp_table_priv: Y
     Lock_tables_priv: Y
     Create_view_priv: Y
       Show_view_priv: Y
  Create_routine_priv: Y
   Alter_routine_priv: Y
         Execute_priv: Y
           Event_priv: Y
         Trigger_priv: Y
*  ************************** 2. row ***************************
                 Host: localhost
                   Db: root
                 User: root
          Select_priv: Y
          Insert_priv: Y
          Update_priv: Y
          Delete_priv: Y
          Create_priv: Y
            Drop_priv: Y
           Grant_priv: N
      References_priv: Y
           Index_priv: Y
           Alter_priv: Y
Create_tmp_table_priv: Y
     Lock_tables_priv: Y
     Create_view_priv: Y
       Show_view_priv: Y
  Create_routine_priv: Y
   Alter_routine_priv: Y
         Execute_priv: Y
           Event_priv: Y
         Trigger_priv: Y
2 rows in set (0.00 sec)


### hogeデータベースにusersテーブルを作った



CREATE TABLE users (
  id INT NOT NULL AUTO_INCREMENT,
  name VARCHAR(50) NOT NULL,
  PRIMARY KEY (id)
) ENGINE=InnoDB;


## test_userでinsertを実行すると


insertが拒否された。なるほど、
read_onlyが有効になっているから更新系クエリーが実行できないということなんだね。


[vagrant@db ~]$ mysql -utest_user -p -Dhoge
Enter password:
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 7
Server version: 5.5.46 MySQL Community Server (GPL)

Copyright (c) 2000, 2015, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> insert into users (id, name) values (1, 'hoge');
ERROR 1290 (HY000): The MySQL server is running with the --read-only option so it cannot execute this statement
mysql>


### my.cnfから
read_onlyを外したらinsert出来た


設定が外れた瞬間普通に実行できる。


[vagrant@db ~]$ mysql -utest_user -p -Dhoge
Enter password:
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 3
Server version: 5.5.46 MySQL Community Server (GPL)

Copyright (c) 2000, 2015, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> insert into users (id, name) values (1, 'hoge');
Query OK, 1 row affected (0.01 sec)

mysql>


## 結論


自分の予想は全て外れたｗ


*  rootアカウントで
grant allのアカウント作成を実行すると、一般権限で
grant allなアカウントが作成される。


*  但し
read_only設定につき更新系クエリーが実行できない。設定がなければ更新系クエリーが実行できる。


### 参考



*  [* MySQL - 一般ユーザにSUPER権限を付与！ - mk-mode BLOG](http://www.mk-mode.com/octopress/2012/02/10/10002040/)