

innobackupex --user=DBUSER --password=DBUSERPASS /path/to/BACKUP-DIR/

innobackupex --apply-log /path/to/BACKUP-DIR


innobackupex --copy-back /path/to/BACKUP-DIR
chown -R mysql:mysql /var/lib/mysql



yum -y install policycoreutils-python

semanage fcontext -a -t mysqld_db_t "/mysql(/.*)?"
restorecon -R -v /var/lib/mysql
restorecon -R -v /var/lib/mysql