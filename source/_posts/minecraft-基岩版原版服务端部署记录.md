---
title: minecraft 基岩版原版服务端部署记录
top: false
cover: false
toc: true
mathjax: true
date: 2021-06-14 14:43:34
password:
summary:
tags:
    - Minecraft
    - 游戏
categories:
    - 踩坑记录
---
# minecraft 基岩版原版服务端部署记录  
## 1. 租一个 vps  
[vultr](https://my.vultr.com/)，[阿里云](https://www.aliyun.com/)，[腾讯云](https://cloud.tencent.com/)都可，想顺便搞个 vpn 的话可租一个海外的服务器。  
单核心 2 G 内存足以满足几个人的小圈子自己玩了，系统用的是 Ubuntu 18.04，其他版本的可能对编译过的 minecraft_bedrock_server 不支持。  
## 2. 服务端下载与配置  
通过 ssh 连接到服务器上，新建一个文件夹作为服务端的根目录。  
```bash
mkdir minecraft_bedrock
```
在[官网下载页面](https://www.minecraft.net/en-us/download/server/bedrock)复制客户端的下载链接到服务器上下载。  
```bash
cd minecraft_bedrock
wget https://minecraft.azureedge.net/bin-linux/bedrock-server-1.17.0.03.zip
```
解压客户端文件，删除压缩包。  
```bash
unzip bedrock-server-1.17.0.03.zip
rm bedrock-server-1.17.0.03.zip
```
如果 `unzip` 命令没安装的话安装一下。  
```bash
apt-get install unzip
```
看一下 `server.properties`，确认一下端口号之类的。基本没有什么需要更改的地方，想改的话看看文件里的注释或者[官方 wiki 中的对应词条](https://minecraft.fandom.com/zh/wiki/Server.properties)。  
## 3. 服务器启动  
为了保证 ssh 连接断开后服务器仍能运行，使用 `screen` 建立一个不依赖于当前 ssh 连接的 session，并在其中运行服务端。  
在根目录建立一个 shell 脚本 `start.sh`，内容如下  
```bash
#!/bin/bash
SCREEN_NAME="minecraft_bedrock"
screen -dmS ${SCREEN_NAME}
CMD="cd /root/minecraft_bedrock"
screen -x -S ${SCREEN_NAME} -p 0 -X stuff "${CMD}"
screen -x -S ${SCREEN_NAME} -p 0 -X stuff "\n"
screen -x -S ${SCREEN_NAME} -p 0 -X stuff "LD_LIBRARY_PATH=. ./bedrock_server"
screen -x -S ${SCREEN_NAME} -p 0 -X stuff '\n'
```
欲启动服务器，只需在根目录下运行  
```bash
bash ./start.sh
```
欲查看状态服务器或使用指令，只需运行
```bash
screen -r minecraft_bedrock
```
进入相应的 session，用快捷键 <kbd>Ctrl</kbd>+<kbd>A</kbd>+<kbd>D</kbd> 即可将 session 挂起。  
欲关闭服务器，只需在对应 session 中 <kbd>Ctrl</kbd>+<kbd>C</kbd>，再 <kbd>Ctrl</kbd>+<kbd>D</kbd> 结束 session。
