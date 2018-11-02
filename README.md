# mha-package

192.168.244.130		master
192.168.244.132		slave01
192.168.244.133		slave02
192.168.244.134		monitor


update mysql.user set authentication_string=password('qqqqqq') where user='root' ;
set password = password('123456');

grant replication slave on *.* to slave@'192.168.244.%' identified by "123";

CHANGE MASTER TO MASTER_HOST='192.168.244.130', MASTER_PORT=3306, 
MASTER_LOG_FILE='itpuxdb-binlog.000004', MASTER_LOG_POS=154, MASTER_USER='slave',
 MASTER_PASSWORD='123';

 grant all on *.* to mha@'192.168.244.%' identified by '123456';


STOP SLAVE;
SET GLOBAL  SQL_SLAVE_SKIP_COUNTER=1;
SHOW GLOBAL VARIABLES LIKE 'SQL_SLAVE_SKIP_COUNTER';

mha
yum install perl-DBD-MySQL -y
yum install perl-ExtUtils-CBuilder perl-ExtUtils-MakeMaker -y
yum install gettext-devel -y
yum install perl-CPAN -y



#wget https://storage.googleapis.com/google-code-archive-downloads/v2/code.google.com/mysql-master-ha/mha4mysql-node-0.53.tar.gz
#wget https://storage.googleapis.com/google-code-archive-downloads/v2/code.google.com/mysql-master-ha/mha4mysql-manager-0.53.tar.gz

perl Makefile.PL
make && make install


wget http://rpmfind.net/linux/dag/redhat/el6/en/x86_64/dag/RPMS/perl-Log-Dispatch-2.26-1.el6.rf.noarch.rpm
wget ftp://rpmfind.net/linux/dag/redhat/el6/en/x86_64/dag/RPMS/perl-Parallel-ForkManager-0.7.5-2.2.el6.rf.noarch.rpm
wget ftp://rpmfind.net/linux/dag/redhat/el6/en/x86_64/dag/RPMS/perl-Mail-Sender-0.8.16-1.el6.rf.noarch.rpm
wget ftp://rpmfind.net/linux/dag/redhat/el6/en/x86_64/dag/RPMS/perl-Mail-Sendmail-0.79-1.2.el6.rf.noarch.rpm
rpm -ivh perl-Mail-Sender-0.8.22-21.1.noarch.rpm --nodeps

set global relay_log_purge=0;

masterha_check_ssh --conf=/etc/masterha_default.cnf
masterha_check_repl --conf=/etc/masterha_default.cnf


nohup masterha_manager  --conf=/etc/masterha_default.cnf --remove_dead_master_conf --ignore_fail_on_start --ignore_last_failover > /mha/manager.log &
