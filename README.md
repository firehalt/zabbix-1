## 项目说明
```
主要用来扩展zabbix的功能，增加对Tomcat/JVM/MYSQL/Redis/Memcache/Mongodb/Nginx等的监控
```

## 目录结构说明
```
为方便使用，项目中所有中间的监控都移到了All In One目录，其目录下有readme.md说明文件，使用该目录下的配置文件、模板文件、脚本文件即可，具体按照说明文件操作即可；  
除了All In One目录外其余目录保留用作参考；  
```

## 变更历史
```
20170119：  
应朋友需求，增加tomcat监控，用于替换zabbix自带的tomcat监控。此监控实现应用了zabbix LLD功能，能自动发现tomcat实例并添加监控。  
zabbix自带的tomcat监控，存在如下不足：  
1）每台主机的监控item具有唯一性，如果一台主机有多个tomcat实例，则需要配置多个host；  
2）需要安装、配置zabbix_java_gateway；  
20170207：  
1）增加All In One目录，将涉及到的中间件监控统一起来，并提供一配置脚本，通过脚本进行统一的配置；  
2）BUG修复；  
3）代码优化；  
```

## 效果
![image](https://github.com/qiueer/zabbix/raw/master/All%20In%20One/effects/p1.png)   
   
![image](https://github.com/qiueer/zabbix/raw/master/All%20In%20One/effects/p2.png)   
  
![image](https://github.com/qiueer/zabbix/raw/master/All%20In%20One/effects/p3.png)   
  
![image](https://github.com/qiueer/zabbix/raw/master/All%20In%20One/effects/p4.png)   


#######20190410##################

1.在zabbix服务器使用命令查看
  [root@3 ~]# zabbix_get -s 172.22.0.32  -k jmx.jvm.item["java.lang:type=Memory",HeapMemoryUsage.committed,12345]
   
2.监控JVM在jar包启动的时候需要添加下面的差数
   -Dcom.sun.management.jmxremote.port=12345  -Dcom.sun.management.jmxremote.ssl=false  -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote   -Djava.rmi.server.hostname=192.16.5.7


二、在zabbix执行命令出现错误
[root@SZ1PRDZBX00AP003 script]# zabbix_get -s 172.22.0.32  -k jmx.jvm.item["java.lang:type=Memory",HeapMemoryUsage.committed,12345]
 can not find java command, exit.
 
 修改jvm.py 脚本里面的113行，把java命令的路径写成绝对路径
  self._java_path = which("/usr/local/jdk1.8.0_111/jre/bin/java")
