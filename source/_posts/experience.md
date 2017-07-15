---
title: 项目问题总结
---

问题1：使用eclipse连接数据库时，出现如下错误：
null,  message from server: "Host 'czkct-PC' is not allowed to connect to this MySQL server"

解决办法：
错误解释：服务器没有授权给你这个ip是不能连接的
你想root用户名使用root密码从任何主机连接到MySQL服务器的话。
运行命令：mysql>GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'root' WITH GRANT OPTION;
前一个root是登录名，后一个root是登录密码


问题2：解决上面的权限问题后，从其他主机连接本机数据库，出现错误Communications link failure
Last packet sent to the server was 0 ms ago.

解决办法：360强制默认开启了防火墙，关闭防火墙即可！


问题3：项目小组要统一编码环境，统一使用UTF-8进行编码，防止程序注释乱码