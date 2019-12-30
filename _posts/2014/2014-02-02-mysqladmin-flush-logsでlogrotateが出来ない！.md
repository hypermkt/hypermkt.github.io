---
layout: post
title: mysqladmin flush-logsでlogrotateが出来ない！
post_id: 209
categories: 
- MySQL
---

[![mysql_logo](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/02/mysql_logo-300x155.png)](https://hypermkt-blog.lolipop.io/wp-content/uploads/2014/02/mysql_logo.png)

なぜだ・・・なぜなんだー！

事の発端はある日のAM4:00に発生したmysqldのメモリー超過でした。4:00ちょうどから急激に実メモリー、スワップメモリーが上昇しはじめ、一時間後あたりでOOMキラーでプロセスがkillされていました。調査した結果、cron.dailyでmysqlのログをlogrotateする際にpostrotateでmysqldを再起動していました。恐らく過去の経緯を考えるに毎朝メモリの解放をする為に再起動にしていたのかと思いますが、mysqldを毎朝再起動するというのもリスクが大きいです。本来、logrotateする際はflush logsでログを切り替えるのが正しいとの事なので早速設定してみた


## /etc/logrotate.d/mysqld


まずはlogrotate.dディレクトリ内に設定ファイルを配置します。


/var/log/mysql/*.log {
    create 640 mysql mysql
    notifempty
    daily
    rotate 60
    missingok
    compress
    postrotate
        # just if mysqld is really running
        if test -x /usr/bin/mysqladmin && \
            /usr/bin/mysqladmin ping &>/dev/null
        then
            /usr/bin/mysqladmin flush-logs
        fi
    endscript
}


## /root/.my.cnf



/etc/logrotate.d/mysqld とサンプルファイルによると下記の通りroot権限でmysqladminを操作する場合は/root/.my.cnfファイルを配置し、その中にmysqlのrootのパスワードを設定するようにと指示がありますのでその通りに設定します。


# If the root user has a password you have to create a
# /root/.my.cnf configuration file with the following
# content:
#
# [mysqladmin]
# password = <secret> 
# user= root
#
# where "<secret>" is the password. 
#
# ATTENTION: This /root/.my.cnf should be readable ONLY
# for root !


$ cat /root/.my.cnf 
[mysqladmin]
password = #{password}
user= root

$ chmod 600 .my.cnf


## デバッグモードで動作を確認する



logrotate -dv /etc/logrotate.d/mysqld


## 手動実行してみた


下記コマンドで実行した所、/var/log/mysqld内の既存のログファイルは全て.gzに切り替わったけど、新規で作成されたクエリログ、エラーログ、スロークエリログが０バイトのまま何も書き込まれない。


logrotate /etc/logrotate.d/mysqld

はて、なぜに・・・

もしかしてmysqladminが実行されてないのか？


mysqladmin flush-logs

を手動実行しても何も変わらず・・・


>MySQL に新しいログ ファイルの使用を強制するには、mysqladmin flush-logs または mysqladmin refresh を使用するか、SQL コマンド FLUSH LOGS を使用します。


とのこなとので、


mysqladmin refresh

を実行してもログが書き込まれず・・・

なぜだー！

mysqldを再起動するとログは切り替わるんだが・・・なんでだろう・・・。