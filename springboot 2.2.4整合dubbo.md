## springboot 2.2.4整合dubbo

由于spring-boot-starter-dubbo版本为1.0导致运行报错

![](C:\Users\LIU\AppData\Roaming\Typora\typora-user-images\image-20200217111858194.png)

之后经过查找资料找到对应关系

![image-20200217111948754](C:\Users\LIU\AppData\Roaming\Typora\typora-user-images\image-20200217111948754.png)

否则只能将spring-boot版本调回1.5.*



调整后的dubbo依赖版本及jar包

```
 <dependency>
                <groupId>org.apache.dubbo</groupId>
                <artifactId>dubbo-spring-boot-starter</artifactId>
                <version>${dubbo-starter.version}</version>
                <exclusions>
                    <exclusion>
                        <groupId>org.slf4j</groupId>
                        <artifactId>slf4j-log4j12</artifactId>
                    </exclusion>
                </exclusions>
            </dependency>

            <dependency>
                <groupId>org.apache.dubbo</groupId>
                <artifactId>dubbo</artifactId>
                <version>${dubbo.version}</version>
            </dependency>

            <dependency>
                <groupId>org.apache.curator</groupId>
                <artifactId>curator-framework</artifactId>
                <version>4.0.1</version>
            </dependency>

            <dependency>
                <groupId>org.apache.curator</groupId>
                <artifactId>curator-recipes</artifactId>
                <version>2.8.0</version>
            </dependency>

            <dependency>
                <groupId>org.apache.zookeeper</groupId>
                <artifactId>zookeeper</artifactId>
                <version>3.4.13</version>
                <type>pom</type>
                <exclusions>
                    <exclusion>
                        <groupId>org.slf4j</groupId>
                        <artifactId>slf4j-log4j12</artifactId>
                    </exclusion>
                </exclusions>
            </dependency>
```

## springboot整合dubbo，zookeeper

####       	 遇到Opening socket connection to server 192.168.xx.12/192.168.xx.12:2181.Will not attempt to authentic



1.  zookeeper的服务没有开启

    执行./zkServer.sh start

2.  访问不到zookeeper中心是因为   需要关闭防火墙：service iptables stop 否则访问不到2181端口

3. 注意配置中的zookeeper的ip地址是否写错，测试环境ip地址写错也有可能。

### service和web分离后，web调用service报错

首先是tk.mybatis版本问题不一致

运行后报下图错

![image-20200218012136570](C:\Users\LIU\AppData\Roaming\Typora\typora-user-images\image-20200218012136570.png)

该情况是由于jdbc **存在时区** 错误在jdbc：mysql：//localhost:3306/test?characterEncoding=UTF-8

在末尾加上serverTimezone=UTC	



