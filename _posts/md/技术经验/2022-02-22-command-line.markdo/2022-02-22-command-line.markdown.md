# 2022-02-22-command-line.markdown

layout: post
title:  "命令行经验"
date:   2022-02-22 11:57:01 +0800
categories: jekyll update

***

### plex-web无管理权

当您登录到Plex服务器的基于Web的控制面板时，根本无法访问控制面板，并得到“您无权访问此服务器。“或者，如果您曾经涉足过多个服务器，或者已经使用其他帐户在同一台计算机上删除并安装了Plex服务器，则无法使用该帐户登录你想使用。

sudo systemctl status plexmediaserver.service	#验证，看plex服务是否启动

sudo systemctl restart plexmediaserver.service

重启后可在plex的设置->管理->插件下看到。

## cygwin中的vim无法用鼠标复制的问题

普通模式下输入“:set mouse-=a”

### 从仅能通过ssh远程控制的主机复制文件到本地

#### 通用但已弃用

```bash
#复制文件
scp username@ip_address:/home/username/filename .

#复制目录
scp -r source_dir username@ip_address:/home/username/target_dir
#跨防火墙复制大文件可能会stalled
```

#### 可增量式同步

```bash
#将文件从远程机器复制到本地机器
rsync username@ip_address:/home/username/filename .

#将文件从本地机器复制到远程机器
rsync filename username@ip_address:/home/username

```

#### 挂载

```bash
sudo apt install sshfs
在系统上安装 sshfs 后，您可以使用它来挂载远程目录，最好为挂载点创建一个专用目录。

mkdir mount_dir
现在以这种方式在远程机器上挂载所需的目录：

sshfs username@IP_address:path_to_dir mount_dir
挂载后，您可以将文件复制到该目录或从该目录复制，就好像它在本地计算机上一样。

cp local_file mount_dir
请记住，您已安装此文件，完成工作后，您还应该卸载它：

umount mount_dir
```

#### **使用基于 GUI 的 SFTP 客户端在远程系统之间传输文件**

FileZilla是最流行的跨平台 FTP 客户端之一

## 命令

```纯文本
ffmpeg -i 输入文件名 -ss 1:02:03 -t 持续时长 -c copy 输出文件名
robocopy、rsync
```
