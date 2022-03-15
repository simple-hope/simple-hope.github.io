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
