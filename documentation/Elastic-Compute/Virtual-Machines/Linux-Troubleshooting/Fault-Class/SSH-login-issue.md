# SSH多次输入root密码后无法登录，如下报错：Too many authentication failures for root

注意：本文相关 Linux 配置及说明已在 CentOS 6.5 64 位操作系统中进行过测试。其它类型及版本操作系统配置可能有所差异，具体情况请参阅相应操作系统官方文档。



**问题描述：**

使用 SSH 登录Linux 云主机时，当多次重试输入密码错误后，服务端返回类似如下错误信息，而后连接中断，登录失败提示：

*Too many authentication failures for root*



**问题原因：**

SSH 服务可以配置密码重试策略。多次连续错误输入密码，触发策略，导致连接被中断，登录失败。

注意：该配置不会导致相关账号被锁定，只会断开相应会话。客户端 SSH 重新登录时，可以再次进行密码尝试。



**处理方法：**

1.通过云主机控制台vnc进入系统。

2.通过 cat 等指令查看 */etc/ssh/sshd_config* 中是否包含类似如下配置：


*cat /etc/ssh/sshd_config*

*MaxAuthTries 6*

说明：该参数默认未启用。用于限制用户在每次 SSH 登录的时候，能够连续错误输入密码的次数。超过错误输入次数会断开 SSH 连接，并显示相关错误信息。但是，相应用户账户并不会被锁定，还可以重新进行 SSH 登录。

3.相关策略可以提高服务器的安全性。请用户基于安全性和易用性权衡后，确定是否需要修改相关配置。

4.如果需要修改相关策略配置，在继续之前建议进行文件备份。

5.使用 vi 等编辑器，按需修改相关参数值，或者整个删除或注释（在最开头添加 # 号）整行配置。比如：


*#MaxAuthTries 6*

如无法解决您的问题，请向我们提工单。
