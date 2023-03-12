# 2022-02-22-storage-stable.markdown

layout: post
title:  "存储可靠性方案"
date:   2022-02-22 12:57:01 +0800
categories: jekyll update

***

本方案的目标是让我所有需持久存储的数据不受换电脑、摔坏手机和误删除的影响；且隐私数据能保密；且换电脑与重装系统时恢复方便。

小文件存储在gitee，机密部分用gpg-short加密。把账号13249600327用于工作，13527700051用于生活。

大文件存储在cifs-read-only，自动cloud-sync到群晖nas。如有需要就使用cryptomator和gitlab私有化部署版进行控制。

gitlab服务器定时备份数据到第二硬盘。注意，git-for-win通过http访问gitlab仓库时不经询问一律保存凭据，所以在不加保护的情况下http协议只能用于work等非敏感账户。

wolai的笔记自动在设备间同步，但要阶段性地导出到onedrive、发布到simple-hope.github.io
