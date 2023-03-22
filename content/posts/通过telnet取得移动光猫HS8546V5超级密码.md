---
title: '通过telnet取得移动光猫HS8546V5超级密码'
date: 2022-08-10
tags: [技术笔记]
slug: tong-guo-telnet-qu-de-yi-dong-guang-mao-hs8546v5-chao-ji-mi-ma
featuredImage: /images/tong-guo-telnet-qu-de-yi-dong-guang-mao-hs8546v5-chao-ji-mi-ma.jpg
---

# 需要的工具

* [ONT维修使能工具](https://pan.baidu.com/s/170toeJhYVTu7CDk2C92SKQ) 提取码：8cb2 解压密码:W06J-HUUs-hzF0-syPN （去掉横杠）

* [华为配置加解密工具](https://pan.baidu.com/s/170toeJhYVTu7CDk2C92SKQ) 提取码：8cb2 解压密码:W06J-HUUs-hzF0-syPN（去掉横杠）

* 任意一种telnet客户端 [XSHELL](https://www.xshell.com/zh/xshell/)

* 一截普通网线（用于直接连接光猫）

* 一个FTP服务器（可选） [TFTP服务器](https://www.tftp-server.com/)

<!--more-->

# 1 telnet使能

首先，用网线连接电脑和光猫，保证电脑分配到一个可访问光猫的IP地址。打开ONT维修使能工具（ONT V5.exe），选择“V5使能”。在“网卡配置”一栏选择连接光猫的网卡（如果你有多张网卡的话请注意甄别）。点击右下角的“启动”，等待左下角“当前成功总数”变为1，同时你可以看到光猫指示灯全亮，点击“停止”，并关闭ONT维修使能工具，接下来将不再需要它。

# 2 telnet接入光猫

打开Xshell，新建空白标签页。输入以下telnet命令:

```bash
telnet 192.168.1.1 # 根据你的具体情况，这个IP地址可能不同
```

随后需要输入账户和密码，这个账户通常是 **root** ，在我这里密码为 **adminHW**，当然你可能会需要尝试以下**其他密码**：

* Hw8@CMCC

* Hw8@cMcc

* useradmin

* admin

* hg2x0
  如果这里的密码都不能成功登录，你可能需要自己在网上搜索更新的密码。
  
  # 3 启动Shell
  
  在成功连接的telnel中，输入下面多条命令将启动shell并到达配置文件的存储目录:
  
  ```bash
  su
  ```
  
  你应该看到“success!”字样在屏幕出现，左侧的提示符页变为
  
  ```bash
  SU_WAP>
  ```
  
  随后输入：
  
  ```bash
  shell
  ```
  
  你将看到如下的BusyBox启动的相关提示：
  
  ```bash
  BusyBox v1.30.1 () built-in shell (ash)
  Enter 'help' for a list of built-in commands.
  ```

profile close core dump

```
同时左侧的提示符页变为：
``` bash
WAP(Dopra Linux) # 
```

这时候cd到需要的目录中：

```bash
cd /mnt/jffs2
```

你可以通过 *ls* 命令来查看该目录下的一些文件，保存有超级密码的配置文件名为 **hw_ctree.xml** ，这是一份加密的配置文件。

# 4 解密配置文件

为了保证安全，首先需要将配置文件复制一份。提示：如果你遇到如下找不到命令情况：

```bash
/bin/sh: aescrypt2: not found
/bin/sh: cp: not found
```

可以通过退出shell，重新启动shell的方式来解决。在成功之前，你可能需要多次尝试。

```bash
exit
shell
cd /mnt/jffs2
```

现在，复制配置文件：

```bash
cp hw_ctree.xml myconf.xml.gz # 待会会解密出一个压缩格式文件,所以直接在这里命名为.gz
```

然后使用aescrypt2解密这个后缀为.gz的xml文件:

```bash
aescrypt2 1 myconf.xml.gz tmp
# aescrypt2 <mode> <input file> <outputfilename>
# 值得注意的是，aescrypt2似乎会无视 <outputfilename> 参数，输出在原来的文件名上，这个实例中就是myconf.xml.gz
```

接下来解压这个文件：

```bash
gzip -d myconf.xml.gz
```

将得到一个名为 **myconf.xml** 的文件，接下来cat出加密的Password字符串：

```bash
cat myconf.xml|grep CMCCAdmin
```

在输出中找到类似下方内容的字符串，他们通常位于输出的第一行，应该很容易找到：

```
UserName="CMCCAdmin" Password="$2@uIc/AkarL8Iwo)FLER@lHy_Y&ltXdfs%orQ9g4yw4$"
```

双引号内的字符串就是加密后的密码，复制他们。打开华为配置加解密工具(huawei.exe), 粘贴刚刚复制的字符串到“密文解密”一栏，点击“$2解密”,即可得到超级密码。**注意**：在某些地区，你可能需要在解密出的这个字符串前面加上 **CMCCAdmin#** 或 **CMCCAdmin** 才能组成正确的超级密码。如果解密出的字符串为 **1234#1234** ，那么除了这个字符串本身就是超级密码以外，还有以下两种可能：

> CMCCAdmin#1234#1234
> CMCCAdmin1234#1234

# 5 写在后面

如果你有一个支持匿名的FTP服务器，你也可以将解密后的myconf.xml传输到电脑上进一步研究:

```bash
tftp -p -l myconf.xml -r myconf.xml <FTP服务器地址>  
```

如果你的光猫***在公共场所提供互联网服务***，在不采取其他措施的情况下，为了你的网络安全，请一定记得通过你刚刚取得超级密码登陆光猫，**在 “安全”——“广域网访问设置”——“ONT访问控制配置” 中关闭通过Telnet访问设备。**

# 6 参考

> * [华为光猫HS8546V5获取超级密码\改华为界面](https://www.right.com.cn/FORUM/thread-4070210-1-1.html)
> * [华为 HG8245C 光猫 修改无线用户数限制+hw_ctree.xml 文件解密](https://www.cnblogs.com/myzerg/p/3622134.html)
