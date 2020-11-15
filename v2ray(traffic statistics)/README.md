一、流量统计介绍

开启流量统计需消耗服务器资源，严重时可能影响速度。非必要不必配置，且此流量统计配置，不支持重启及修改配置后，保持之前流量统计记录。

二、此配置可以统计情况

1、入站的统计（不推荐，一般建议关闭。），根据 tag 来统计入站流量。

2、用户的统计（推荐），依据 email 区分及统计用户流量。socks, http 等其他协议内的用户不支持被统计。  

三、流量信息的处理

1、把 traffic.sh 脚本上传到服务器 root 目录，并授予执行权限（chmod +x traffic.sh）。

2、执行 ./traffic.sh 即可查看流量统计。