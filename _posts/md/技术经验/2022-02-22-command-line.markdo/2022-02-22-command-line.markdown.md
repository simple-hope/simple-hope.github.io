# 2022-02-22-command-line.markdown

layout: post
title:  "命令行经验"
date:   2022-02-22 11:57:01 +0800
categories: jekyll update

***

## 快速创建空白占位文件

```powershell
@echo off
:1
fsutil file createnew .\%random%.fsutil 10737418240
echo 注意磁盘空间变化
pause
goto :1
```

## Win10系统BitLocker解锁后再次快速锁定办法

<http://blog.csdn.net/ttljtw/article/details/54022241>

```powershell
manage-bde -lock E:
```

## cygwin中的vim无法用鼠标复制的问题

普通模式下输入“:set mouse-=a”

## ffmpeg

```bash
#裁剪视频
ffmpeg -i 输入文件名 -ss 1:02:03 -t 持续时长 -c copy 输出文件名

#合并视频：需要额外的文本描述文件

```

### 随机打乱文件

```bash
for i in *; do mv "$i" $RANDOM; done
```
