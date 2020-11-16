一、v2ray 禁用 BT 配置方法

注：
1、仅服务器配置即可。

2、所有配置示例已都配置了禁用BT，此配置方法仅备份及参考等。

二、v2ray SNI 分流配置方法

注：相关配置示例已配置 v2ray SNI 分流共用端口 ，此配置方法仅备份及参考等。

三、v2ray 流量统计配置方法

1、流量统计介绍

开启流量统计需消耗服务器资源，严重时可能影响速度。非必要不必配置，且此流量统计，不支持重启及修改配置后保持之前流量统计记录。

2、此配置可以统计情况

1）、入站的统计（不推荐，一般建议关闭。），根据 tag 来统计入站流量。

2）、用户的统计（推荐），依据 email 区分及统计用户流量。socks, http 等其他协议内的用户不支持被统计。  

3、流量信息的处理

1）、把 traffic.sh 脚本上传到服务器 root 目录，并授予执行权限（chmod +x traffic.sh）。

2）、执行 ./traffic.sh 即可查看流量统计。