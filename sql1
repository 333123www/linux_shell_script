#!/bin/bash 
#安装函数
function sql_install {
dnf -y install mariadb mariadb-server
systemctl start mariadb.service
systemctl enable mariadb.service
mysqladmin -u root password 123456
echo
echo  "----------mariadb 安装完成----------"
echo
}

#查询 sql slave 状态函数
function sql_query {
mysql -uroot -p123456 <<EOF
show databases;
select substring('------------------------',1,20);
show slave status\G;
EOF
echo
echo  "----------slave 查询完成----------"
echo
}

#查询热备数据库库中的数据表，普通查询
function sql_table {
mysql -uroot -p123456 <<EOF
use test;
show tables;
EOF
echo
echo  "----------数据库test中所有数据表 查询完成----------"
echo
}

#数据库开启all远程函数
function sql_remote {
mysql -uroot -p123456 <<EOF
select substring('------------------------',1,20);
use mysql;
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%'IDENTIFIED BY '123456' WITH GRANT OPTION;
flush privileges;
select *from user\G
select substring('------------------------',1,20);
EOF
echo 
echo "----------sql remote 任务结束----------"
echo
}


#sql1 开启主主热备函数
function sql_back1 {
mysql -uroot -p123456 <<EOF
create database test;
show databases;
grant replication slave on *.* to 'root'@'ip_back_1' identified by '123456';
flush privileges; 
select * from mysql.user where user='root'\G;
EOF
sed -i '22a server-id=1
22a log-bin=mysql-bin
22a binlog-ignore-db=mysql
22a log-slave-updates
22a sync_binlog=1
22a auto_increment_offset=1
22a auto_increment_increment=2
22a replicate-ignore-db=mysql,information_schema
' /etc/my.cnf.d/mariadb-server.cnf
systemctl restart mariadb.service
mysql -uroot -p123456 <<EOF
show master status\G;
change master to master_host='ip_back_1',master_user='root',master_password='123456', master_log_file='mysql-bin.000001',master_log_pos=328;
start slave;
show slave status\G;
EOF
	echo 
	echo "----------back up1 任务完成----------"
	echo 
}


#sql2 开启主主热备函数
function sql_back2 {
mysql -uroot -p123456 <<EOF
create database test;
show databases;
grant replication slave on *.* to 'root'@'ip_back_2' identified by '123456';
flush privileges; 
select * from mysql.user where user='root'\G;
EOF
sed -i '22a server-id=2
22a log-bin=mysql-bin
22a binlog-ignore-db=mysql
22a log-slave-updates
22a sync_binlog=1
22a auto_increment_offset=2
22a auto_increment_increment=2
22a replicate-ignore-db=mysql,information_schema
' /etc/my.cnf.d/mariadb-server.cnf
systemctl restart mariadb.service
mysql -uroot -p123456 <<EOF
show master status\G;
change master to master_host='ip_back_2',master_user='root',master_password='123456', master_log_file='mysql-bin.000001',master_log_pos=328;
start slave;
show slave status\G;
EOF
	echo
	echo "----------back up2 任务完成----------"
	echo 
}

#修改数据库密码函数
function sql_passwd {
systemctl stop mariadb.service
sed -i '22a\skip-name-resolve
22a\skip-grant-tables' /etc/my.cnf.d/mariadb-server.cnf
systemctl restart mariadb.service
mysql <<EOF
update mysql.user set authentication_string=password('123456') where user='root';
flush privileges;
EOF
sed -i 's\skip-name-resolve\#skip-name-resolve\
s\skip-grant-tables\#skip-grant-tables\' /etc/my.cnf.d/mariadb-server.cnf
systemctl restart mariadb.service
mysql -uroot -p123456 <<EOF
select substring('------------------------',1,20);
show databases;
select substring('------------------------',1,20);
EOF
echo
echo "++++++++++mariadb id:root passwd:123456++++++++++"
echo 
echo "----------modify passwd 任务完成----------"
echo
}

#创建数据表函数，验证热备
function sql_create {
mysql -uroot -p123456 <<EOF
show databases;
use test;
create table test1 (id int,id1 int,id2 int);
show tables;
EOF
echo
echo  "----------test数据库 中 test1数据表 创建完成----------"
echo
}

#mariadb 开启日志功能函数
function sql_log {
mysql -uroot -p123456 <<EOF
set global general_log=1;
show global variables like '%general%';
EOF
echo 
ls -l /var/lib/mysql/*.log
echo 
echo "----------sql log 任务完成----------"
echo
}

#程序退出函数
function sql_exit {
echo 
echo "bye bye ~~~"
}

#程序异常状态函数
function sql_wrong {
echo 
echo "----------输入错误，请重新输入!----------"
echo
}

#脚本开始锚点
PS3="ENTER OPTIONS:"
select option in "sql install" "sql remote" "sql log" "sql query" "sql table" "sql create table" "back up1" "back up2" "modify passwd" "exit"
do
	case $option in 

#安装功能模块
	"sql install" )
		sql_install;;

#开启数据库远程模块
	"sql remote" )
		sql_remote;;
		
#查询slave 状态功能模块
	"sql query" )
		sql_query;;

#主主备份,数据库1 模块
	"back up1" )
		sql_back1;;

#主主备份,数据库2 模块	
	"back up2" )
		sql_back2;;
		
#查询热备数据库中的数据表
	"sql table" )
		sql_table;;
		
#创建数据表验证
	"sql create table" )
		sql_create;;

#修改sql密码模块
	"modify passwd" )
		sql_passwd;;
		
#开启mariadb 通用日志功能
	"sql log" )
		sql_log;;

#退出脚本模块
	"exit" )
		sql_exit
		break;;

#程序异常状态模块
	* )
		sql_wrong;;
	esac
done
