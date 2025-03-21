```bash
#【详见：https://blog.csdn.net/hl_java/article/details/90609516】
#只抓目的ip
sudo /usr/sbin/tcpdump -i eth0 tcp dst host 1.2.3.4 -c 100 -w 0515.pacp
 
#只抓目的port
sudo /usr/sbin/tcpdump -i eth0 tcp dst port 9030 -c 100 -w 0515.pacp
 
#只抓目的ip&目的port
sudo /usr/sbin/tcpdump -i eth0 tcp dst host 1.2.3.4 and dst port 9030 -c 100 -w 0515.pacp
 
#只抓源port(通常是本机的监听端口)
sudo /usr/sbin/tcpdump -i eth0 tcp src port 9030 -c 100 -w 0515.pacp
 
#附加参数说明
-c 100 只抓100个包
-w 0515.dump 抓包的内容存储到0515.pacp
```

https://linux.cn/article-10191-1.html  
https://www.cnblogs.com/ggjucheng/archive/2012/01/14/2322659.html  
https://blog.csdn.net/hl_java/article/details/90609516  
