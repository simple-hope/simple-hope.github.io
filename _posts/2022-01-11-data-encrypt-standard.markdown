---
layout: post
title:  "数据加密标准"
date:   2022-01-11 22:36:25 +0800
categories: jekyll update
---

## 个人数据存储标准
1. 非机密热数据：gitee 备份
1. 非机密冷数据：多地备份
1. 机密热数据：gitee 备份配合 gpg 文本化加密
1. 机密冷数据：
+ 敏感设备中用 gpg 二进制加密
+ 私密设备中用分区级加密

## 搭建加密环境

### 在私密设备 Ubuntu 20.04.3 LTS 中生成密钥并导出
```bash
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

### 在敏感设备中导入并信任公钥
```bash
$ gpg --import e1024.pub
$ gpg --edit-key e@e.com
 trust
 5
 y
 save
```

### 在git中配置filter和diff
```bash
# 敏感设备中的 git 配置用于加密
echo '*.md filter=gpg-a' >> .gitattributes
git config filter.gpg-a.clean  "gpg -e -a -r e@e.com"

# 私密设备中的 git 配置用于解密
git config filter.gpg-a.smudge "gpg -d -a -r e@e.com"
echo '*.md diff=keep' >> .gitattributes
git config diff.keep.textconv cat

```
