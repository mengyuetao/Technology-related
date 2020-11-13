


```


docker run --name=mysql_for_nlp \
   --mount type=bind,src=/data/mysql_for_nlp/etc/my.cnf,dst=/etc/my.cnf \
   --mount type=bind,src=/data/mysql_for_nlp/var/lib/mysql,dst=/var/lib/mysql \
   -p 3306:3306 \
   -d mysql/mysql-server:5.7

docker logs mysql_for_nlp 2>&1 | grep GENERATED
docker exec -it mysql_for_nlp mysql -uroot -p

ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';
CREATE USER 'nlp'@'%' IDENTIFIED BY 'MyFirstNlp4!';
GRANT all ON *.* TO 'nlp'@'%';
flush privileges;

#导入数据
docker exec -it mysql_for_nlp bash
mysql -u root -p < mengyuetao_db_backup_without_music_and_mark_20201110.sql

```
