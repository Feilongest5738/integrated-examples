一、回落终极部署（配置1/配置2/配置3套娃方式）

v2ray 前置（监听443端口），vless+tcp 以 h2 或 http/1.1 自适应协商连接，分流 ws（WebSocket）连接，非 v2ray 的 http/1.1 连接直接回落给 nginx，而余下的 h2 连接回落给 trojan+tcp，trojan+tcp 处理后再回落给 nginx。其应用如下：

1、vless+tcp+tls（回落/分流配置。）

2、vless+ws+tls（tls由vless+tcp+tls提供及处理，不需配置；另可改成或添加vmess+ws+tls、SS+v2ray-plugin+tls、trojan+ws+tls应用。）

3、trojan+tcp+tls（tls由vless+tcp+tls提供及处理，不需配置。）

利用 vless 强大的回落/分流特性，实现了共用 443 端口，支持 vless+tcp 与任意 ws（WebSocket） 类及 trojan+tcp 应用完美共存。

注意：

1、v2ray v4.31.0 版本及以后才支持 trojan 协议。 

2、nginx 支持 h2c server，但不支持 http/1.1 server 与 h2c server 共用一个端口或一个进程（Unix Domain Socket 应用）；故此 vless+tcp 应用中的回落端口或进程必须分开，分别对应。

3、trojan+tcp 目前不支持回落端口或进程分离 ，故 trojan+tcp 连接及回落只能二选一。此次 vless+tcp 应用默认采用 h2 连接及回落，故套娃 trojan+tcp 只能仅采用 h2 连接及回落。

4、nginx 预编译程序包可能不带支持 PROXY protocol 协议的模块。如要使用此项协议应用，需加 http_realip_module（必须加） 及 stream_realip_module（可选加） 两模块构建自定义模板，再进行源代码编译和安装。另编译时选取源代码版本建议不要低于1.13.11。

5、配置1：端口转发、端口回落，没有启用 PROXY protocol。配置2：进程转发、进程回落，没有启用 PROXY protocol。配置3：进程转发、进程回落，启用了 PROXY protocol。

二、nginx SNI 分流优化共用443端口（配置4/配置5/配置6） 

利用 nginx 支持 SNI 分流特性，对 vless+tcp 与 trojan+tcp 进行端口分流（四层转发），实现共用443端口。vless+tcp 以 h2 或 http/1.1 自适应协商连接，分流 ws（WebSocket）连接，非 v2ray 的 web 连接回落给 nginx。trojan+tcp 以 h2 协商连接，非 v2ray 的 web 连接也回落给 nginx。其应用如下：

1、vless+tcp+tls（回落/分流配置。）

2、vless+ws+tls（tls由vless+tcp+tls提供及处理，不需配置；另可改成或添加vmess+ws+tls、SS+v2ray-plugin+tls、trojan+ws+tls应用。）

3、trojan+tcp+tls（回落配置。）

注意：

1、v2ray v4.31.0 版本及以后才支持 trojan 协议。 

2、nginx 支持 h2c server，但不支持 http/1.1 server 与 h2c server 共用一个端口或一个进程（Unix Domain Socket 应用）；故此 vless+tcp 应用中的回落端口或进程必须分开，分别对应。

3、nginx 支持 h2c server，但不支持 http/1.1 server 与 h2c server 共用一个端口或一个进程（Unix Domain Socket 应用），故回落端口或进程必须分开。而 trojan+tcp 目前不支持回落端口或进程分离 ，故 trojan+tcp 连接及回落只能二选一。本次示例 trojan+tcp 采用 h2 连接及回落，毕竟 h2 连接自带链路复用，且延迟小一点。

4、nginx 预编译程序包一般不带支持 SNI 分流协议的模块。如要使用此项协议应用，需加 stream_ssl_preread_module 模块构建自定义模板，再进行源代码编译和安装。

5、nginx 预编译程序包可能不带支持 PROXY protocol 协议的模块。如要使用此项协议应用，需加 http_realip_module 与 stream_realip_module 两模块构建自定义模板，再进行源代码编译和安装。另编译时选取源代码版本建议不要低于1.13.11。

6、配置4：端口转发、端口回落及端口分流，没有启用 PROXY protocol。配置5：进程转发、进程回落及进程分流，没有启用 PROXY protocol。配置6：进程转发、进程回落及进程分流，启用了 PROXY protocol。
