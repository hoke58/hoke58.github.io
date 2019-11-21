---
title: Youku 路由器刷 Padavan
date: 2019-11-18 11:19:14
categories: 系统运维
tags: 
  - router
  - padavan
  - youku
---

{% note warning %}
警告：刷机有风险，由此产生的一切后果请自行承担！！！
{% endnote %}

## require

1. 官改固件：YKL1_2.1.0613.8617_root_telnet.bin
2. Breed：breed-mt7620-youku-yk1.bin
3. Padavan 固件：RT-N14U-GPIO-1-youku1-128M_3.4.3.9-099.trx (2019-11-13版本)

所需文件下载链接： http://pan.hoke58.cn/index.php/s/5GNNJkwrtJDPe4f

## 升级官改固件

Breed 是必备，但是要刷 Breed 就得先刷已 root 和开启 Telnet 的固件。

1. 登录路由器管理界面，`「更多设置」` -> `「系统升级」` -> `「手动升级」`，选择前面下载的 `YKL1_2.1.0613.8617_root_telnet.bin`，点击 `「上传文件」`，等待升级包验证通过，点击`「立即升级」`，然后就等待路由器重启，这个过程中不要断开网线也不要断开电源。

2. 重启完成后，按住路由器上 `RESET` 按键重置路由器。

> 第2步必须，否则还是无法 `Telnet` 连接路由器

## 刷入 Breed
1. 使用 shell 客户端 `telnet 192.168.11.1` 登录路由器, 如果有登录身份验证，用户名和密码都是 admin ，或者试试 root/admin 组合
```sh
[c:\~]$ telnet 192.168.11.1
Connecting to 192.168.11.1:23...
Connection established.
To escape to local shell, press 'Ctrl+Alt+]'.
 === IMPORTANT ============================
  Use 'passwd' to set your login password
  this will disable telnet and enable SSH
 ------------------------------------------


BusyBox v1.22.1 (2016-05-31 09:37:31 CST) built-in shell (ash)
Enter 'help' for a list of built-in commands.

  _______                     ________        __              
 |       |.-----.-----.-----.|  |  |  |.----.|  |_      
 |   -   ||  _  |  -__|     ||  |  |  ||   _||   _|        
 |_______||   __|_____|__|__||________||__|  |____|       
          |__| W I R E L E S S   F R E E D O M  

 -----------------------------------------------------
 BARRIER BREAKER (Barrier Breaker, unknown)
 -----------------------------------------------------
  * 1/2 oz Galliano         Pour all ingredients into
  * 4 oz cold Coffee        an irish coffee mug filled
  * 1 1/2 oz Dark Rum       with crushed ice. Stir.
  * 2 tsp. Creme de Cacao
 -----------------------------------------------------
 customized by youku, 
 copyright (c) youku, 2015. all rights reserved.
```

2. 导入 breed-mt7620-youku-yk1.bin 文件至路由器

<p align="center">以上步骤之后有 <b>两种选择</b>，请<b>任选其一</b>然后<b>继续后面的步骤</b>。</p>

* **选择 1：U盘拷入**：

  1. 将 breed-mt7620-youku-yk1.bin 文件复制到U盘，U盘插上路由器
  2. `ls /mnt/sda1` 查看U盘里 breed-mt7620-youku-yk1.bin 文件是否存在

**注意：**U盘未必能挂载成功。可以执行 df -h 看看有没有类似「/dev/sda1」这样的设备，然后挂载U盘：`mount /dev/sda1 /mnt/sda1` ，若挂载成功，在检查一下U盘是否有 Breed 文件。

* **选择 2：`wget` 下载**：

  1. 确保路由器能访问外网
  2. 执行以下命令，下载 Breed 至 `/tmp`
```bash
cd /tmp && wget --user-agent="Mozilla/5.0 (Windows; U; Windows NT 6.1; en-US) AppleWebKit/534.16 (KHTML, like Gecko) Chrome/10.0.648.204 Safari/534.16" -O breed-mt7620-youku-yk1.bin 'http://pan.hoke58.cn/index.php/s/5GNNJkwrtJDPe4f/download?path=%2F&files=breed-mt7620-youku-yk1.bin'
``` 

3. 检查 Breed 的 MD5
```shell
[root@Youku-router]md5sum breed-mt7620-youku-yk1.bin 
44379c40f77d3d75bcba97ce136a880d  breed-mt7620-youku-yk1.bin
```
如 MD5 不一致就重新下载！

4. 解锁 Bootloader
```sh
[root@Youku-router]mtd unlock Bootloader
Unlocking Bootloader ...
```

5. 刷 Breed
```sh
[root@Youku-router]mtd -r write /tmp/breed-mt7620-youku-yk1.bin Bootloader
Unlocking Bootloader ...

Writing from /tmp/breed-mt7620-youku-yk1.bin to Bootloader ...     
Rebooting ...
```

然后路由器会自动重启, 至此 Breed 成功刷入，恭喜变成不死路由

## 刷入 Padavan 固件
1. 需要有线电脑，IP设为`192.168.1.2`（新版本 Breed 设自动获取就行）
2. 路由器断电，按住复位键，上电，10来秒后松手
3. 电脑浏览器访问 `192.168.1.1` 进入 Breed 页面
4. 固件更新，刷入老毛子固件 RT-N14U-GPIO-1-youku1-128M_3.4.3.9-099.trx 即可
5. 刷好固件以后，手动断电，等待几秒，再通电开机！(路由可能无法自动开机重启)

## Enjoy it
Padavan 默认管理地址: `http://192.168.123.1`
默认密码: `admin / admin`
默认 SSID: `PDCN / 1234567890`