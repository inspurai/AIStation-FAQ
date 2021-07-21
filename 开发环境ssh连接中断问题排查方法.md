提供如下四种方式。
第一种：
1.修改容器sshd配置文件/etc/ssh/sshd_config：
ClientAliveInterval 60 ＃server每隔60秒发送一次请求给client，然后client响应，从而保持连接
ClientAliveCountMax 3 ＃server发出请求后，客户端没有响应得次数达到3，就自动断开连接，正常情况下，client不会不响应

重启服务：service ssh restart
最好保存镜像，后续用新镜像连接。
2. 修改ssh client端/etc/ssh/sshd_confi（发起ssh请求的机器，可能是客户的笔记本电脑或者跳板机）的配置。这个是建议配置，如果上面的生效，该配置可以不用修改
ServerAliveInterval 60
ServerAliveCountMax 3
重启服务：service ssh restart

第二种：
有一部分服务器硬件时钟可能会出现时差偏大，导致ntp时钟同步问题，需要修改计算节点的时钟同步定时任务，修改定时任务10分钟同步一次修改为1分钟同步一次。
crontab –e
*/1 * * * * /usr/sbin/ntpdate -u 10.151.11.53 >> /var/log/ntpdate/ntpdate.log && /usr/sbin/hwclock –w
重启服务：systemctl restart crond

第三种：
连接ssh命令增加一个参数
ssh -o ServerAliveInterval=60 user@ip –p 22

第四种：
如果是工具连接，需要配置一下shell工具的长连接会话，比如MoBaXterm
setting -> configuraton ->  ssh

 ![](data\MobaXterm1.png)
 <p><img src="/data\MobaXterm1.png" alt="foo" title="title" /></p>

勾选keepalive然后保存。

备注：一般以上四种只要有一种生效，其他都不用配置。往往出现又连接中断的问题都是以前配置的丢失或者镜像更改了导致。
