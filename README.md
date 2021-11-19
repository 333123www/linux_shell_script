[linux_shell.md]
# linux_shell_script
## mariadb_sql.sh 

介绍：脚本的功能对linux系统上的mariadb数据库进行快速操作所编写，具备的功能为: 

1. 安装数据库并启动，设置密码为123456
2. 开启数据库的通用记录日志
3. 开启mariadb的远程访问功能
4. 在不知道原密码的情况下，修改mariadb数据库密码功能
5. 在服务器1上开启mariadb上主主热备功能
6. 在服务器2上开启mariadb上主主热备功能，同服务器1mariadb互为主主热备
7. 查询slave状态功能
8. 验证主主热备功能，在样例数据库中创建test1数据表进行验证
9. 验证主主热备功能，查询样例数据库中的test1数据表

使用方式：

```
chmod +x mariadb_sql.sh 

#将服务器1上将地址填写为服务器2的地址，执行back ip1功能
sed -i 's/ip_back_1/10.10.10.10/' sql 	

#在服务器2上将地址填写为服务器1的地址，执行back up2功能
sed -i 's/ip_back_2/10.10.10.11/' sql
```



截图：
![image](https://user-images.githubusercontent.com/71164067/142596954-e390cb9e-c10a-4f4a-be45-b8576729c917.png)



------



## ssh_login.sh

介绍：脚本的功能为在linux系统上使用ssh快速登录，并修改root密码。 

使用方式为：运行脚本参数:第一个参数为ssh连接ip地址,第二个参数为登录密码,第三个参数为修改后的密码

截图
![image](https://user-images.githubusercontent.com/71164067/142596976-f46c7286-6785-43a1-9cb2-59c97a3f0a0e.png)




