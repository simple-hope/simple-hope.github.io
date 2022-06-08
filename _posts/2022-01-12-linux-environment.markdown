---
layout: post
title:  "我的linux环境搭建"
date:   2022-01-12 22:08:12 +0800
categories: jekyll update
---

## 覆盖/home后要做
```shell
# 导入并信任gpg密钥
gpg --import e1024.pub
gpg --edit-key e@e.com
 trust
 5
 y
 save
gpg --import e1024.pri

# 生成ssh密钥
ssh-keygen -t ed25519 -C "Lenvo"




# 按3次回车
cat ~/.ssh/id_ed25519.pub
# 需手动把公钥放上github或gitee
ssh -T git@gitee.com
yes
# 输出 "... GitHub does not provide shell access." 是正常的，错误输出是：
# Agent admitted failure to sign using the key.
# debug1: No more authentication methods to try.
# Permission denied (publickey).
```

## 不改变/home重装ubuntu后要做
```shell

# 使用本地时区可避免与windows时间错乱
timedatectl set-local-rtc 1

sudo echo 获取临时sudo免密权限

sudo apt -y update 
sudo apt -y install vim git

cd ~/文档/
git clone git@gitee.com:ubuntubackup/dot-file.git
cd /usr/local/
sudo rm -r games
sudo ln -s ~/文档/dot-file/bin games
cd ~

git config --global filter.gpg-a.smudge "gpg -d -a -r e@e.com"
git config --global filter.gpg-a.clean  "gpg -e -a -r e@e.com"
git config --global diff.plaintext.textconv cat
git config --global user.name "e1024"
git config --global user.email "e@e.com"
git config --global core.editor vim
git config --global core.quotepath false
git config --global alias.goa 'log --graph --pretty=oneline --abbrev-commit'
git config --global alias.uiau "update-index --assume-unchanged"

```

## 搭建加密环境

### 在私密设备 Ubuntu 20.04.3 LTS 中生成密钥并导出
```shell
$ gpg --full-generate-key
 2
 1024
 0
 y
 e1024
 e@e.com
 forever
 o
$ gpg --export-secret-keys -a -v -o e1024.pri
$ gpg --export -a -v -o e1024.pub
$ cat e1024.pub
-----BEGIN PGP PUBLIC KEY BLOCK-----

mQGiBGHYSyYRBAC7scRYhEU2PSKrXdmixUOaelsKzV+NmHdSWbAKyM6n0X6/zVpi
8kE3x4TAp9dBGWXb/w6Hnpy59E4SXUNrV5mpnVWXrFukJVWkOEUxssTARTtEw1S3
u9sKzr5RRQpRgJvb4mGketn1E5F28ZWy4xJ1KdwPodk49lxqIWC4VECYmwCgo+HL
/kN0G+lqgDMpkwQguZ58JSMD/iq4NXltNWZFQc5Astkx35ZJKoXaL4ysXDAkZmqr
LM/QrRoLAu4EenVwYnfFPzThOPcuJa+OpuyKeF0y5rXm7BEse+hDZiqXQDpe/e1H
u5pINNM8uQmWfmzeYALXgyGl9jOzFm5Jykwpmb2fekZvwueoMwbjkfGBzstCasra
nHt5A/4uIdJTkIqEtf+iD/m5JyKNXkvRCIEAPoUqivWyjlsBdYnkuXcrZ0ViOxlH
w9ZD3qO0nN5VU/TNbG/QEdBLEofa388V1S46YSzkHiqasnE7ASXDPWnTXHjMZxk/
wlbQPD/W1nDWz7eNoNqUeGrGQRnpsHcUwJo5Vbb5ptCxzvMaOLQZZTEwMjQgKGZv
cmV2ZXIpIDxlQGUuY29tPoh4BBMRAgA4FiEEp0jgwHNPGLcKChLm7i1DgXRGzi8F
AmHYSyYCGwMFCwkIBwIGFQoJCAsCBBYCAwECHgECF4AACgkQ7i1DgXRGzi/vMwCf
Vcd1Nj7Nlkv1ll5KsmGanA5T1vIAnRlO28Zvqn61T/IJhL5qHD1ZNKDouQENBGHY
SyYQBADk4vgGGwU6Tl4aiQU1KX+EzBb2xBhidOwnCRoYX0SuOn1ZW0QvJvu7cyou
1Tqt286Iex4Ea/KVdBPR3KfGKJRCDaDQ0TTWLjg1O76d8NmtK130fE7iTweGnjzL
lS6h7mqRjl11OmteIW1Ad3ZY3IbBMIywVgwiRRfe4qnMcHRQZwADBQP/XujrpoJT
/t2+4MZtJJvCHaa84Nb2IJuzMXdUem/uDkFMHPGbHAgZqXjWi/HA33lHjtiy0SlX
GsQCYFCzhVeHxA11sPf4DI9sG4ywkr5g84B/ZqthSoLFqVFEDXHsdqyCTU9x8aO9
uC6ET+hBJKBPo5ysYKMKIqQGxWBn/P8N5gCIYAQYEQIAIBYhBKdI4MBzTxi3CgoS
5u4tQ4F0Rs4vBQJh2EsmAhsMAAoJEO4tQ4F0Rs4vb8cAn1faVUxNyGLonoKsiTAA
22djdQyvAJ9p/GpyerhbfuucX2eQfDzqoVS7sg==
=iwCj
-----END PGP PUBLIC KEY BLOCK-----

```

### 在.gitattributes中配置filter和diff
```
*.md filter=gpg-a
*.md diff=plaintext
```

## cygwin中的vim无法用鼠标复制的问题
普通模式下输入“:set mouse-=a”

## 把 omnibus gitlab 私有化部署到 Ubuntu 20.04 LTS
先手动将“软件和更新”的源改为阿里云
```
wget https://omnibus.gitlab.cn/ubuntu/focal/gitlab-jh_14.9.0-jh.0_amd64.deb &
sudo apt-get update
sudo apt-get install -y curl openssh-server ca-certificates tzdata perl
sudo apt-get install -y postfix
# 在安装 Postfix 的过程中可能会出现一个配置界面，在该界面中选择“Internet Site”并按下回车。把“mail name”设置为 gitlab.sincerity.com
export EXTERNAL_URL=https://gitlab.sincerity.com
sudo dpkg -i gitlab-jh_14.9.0-jh.0_amd64.deb
sudo vi /etc/gitlab/gitlab.rb
# gitlab_rails['initial_root_password'] = 'password'
# external_url "http://gitlab.sincerity.com"
sudo gitlab-ctl reconfigure

sudo mkdir /secret/gitlab/backups/
sudo crontab -e -u root
# 15 04 * * 2-6  gitlab-ctl backup-etc && cd /etc/gitlab/config_backup && cp $(ls -t | head -n1) /secret/gitlab/backups/
```
最后手动登录root账户并修改密码，偏好设置->本地化->语言->中文 

### 1.备份
gitLab备份的默认目录是`/var/opt/gitlab/backups`，若想要主动执行备份操作，可以通过
```
gitlab-rake gitlab:backup:create
```
命令会在备份目录下创建一个以时间戳开头的xxxxxxxx_gitlab_backup.tar的压缩包，这个压缩包包括整个完整的gitlab。

若需要修改默认的备份目录，可以通过修改`/etc/gitlab/gitlab.rb`主配置文件来设置
```
gitlab_rails['backup_path'] = '/data/backups'
```

### 2.恢复
指定恢复文件，gitlab会自动去查找备份目录。

指定文件名的格式类似：1535861590_2018_09_02_11.2.3，文件名后缀gitlab_backup.tar不需要添加”
```
gitlab-rake gitlab:backup:restore BACKUP=1535861590_2018_09_02_11.2.3
```

### docker部署
[安装docker](https://www.runoob.com/docker/ubuntu-docker-install.html)
[搭建gitlab](https://zhuanlan.zhihu.com/p/49499229)
