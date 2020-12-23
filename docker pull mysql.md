### 1.当防火墙关闭时,指令执行报错:

[root@localhost /]# docker run -itd --name mysql-test -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql

#### error:

![image-20200925114603653](C:\Users\LIU\AppData\Roaming\Typora\typora-user-images\image-20200925114603653.png)

### 2.