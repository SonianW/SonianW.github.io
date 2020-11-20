---
title: 在 python 中调用 ffmpeg 做视频处理
top: false
cover: false
toc: true
mathjax: true
date: 2020-11-20 18:30:12
password:
summary:
tags:
categories:
---

# 在 Python 中调用 ffmpeg 进行视频处理
## 前言  
ffmpeg 是一款强力的开源多媒体处理工具。对于已安装 ffmpeg 的系统，可以直接在 Python 中通过 `os.popen(cmd)` 调用 ffmpeg。  
使用 [ffmpeg-python](https://github.com/kkroening/ffmpeg-python) 是另一种 ffmpeg 调用方法。该库将执行相应 cmd 的过程封装在其函数中，可以进行更为优雅的调用。  
ffmpeg-python 的具体使用方法可见其[官方文档](https://kkroening.github.io/ffmpeg-python/)，本文仅分享一些基础的使用方法的个人理解与几个实际应用中的场景。  
## 基本类与方法  
### Stream  
### input  
### output  
### probe  
### run  