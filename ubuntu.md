# Ubuntu
## 1. 指令安装deb包
	sudo dpkg -i package.deb

## 2. 一些ffmpeg指令
### 截取视频
	ffmpeg  -i input.mp4 -vcodec copy -acodec copy -ss 00:00:10 -to 00:02:10 outcut.mp4 -y // 截取input.mp4从10秒到2分10秒中间的两分钟视频，并保存为outcut.mp4
    
### 转换视频格式
	ffmpeg -i  testrmvb.rmvb  -c:v libx264 -strict -2 testrmvb.mp4
	
### 转换视频和音频格式
	ffmpeg -i test.mp4 -c:v h264 -c:a aac -strict -2 test_convert.mp4 // 将test.mp4的视频编码转换为h.264,音频编码转换为aac
	
### 仅转换视频的音频格式
	ffmpeg -i test.mp4 -c:v copy -c:a aac -strict -2 test_convert.mp4 // 将test.mp4的音频编码转换为aac,视频编码保持原来的
    
### 修改视频分辨率
	ffmpeg -i 1.mp4 -strict -2 -vf scale=720:480 4.mp4 
    // 其中 scale 后面为要修改的分辨率
    
    // 出现错误 Unable to parse option value "-1" as pixel format. 加上 -analyzeduration 2147483647 -probesize 2147483647 试试，可能成功。即：
    ffmpeg -analyzeduration 2147483647 -probesize 2147483647 -i 1.p4 -strict -2 -vf scale=720:480 4.mp4

### 修改视频比特率
	ffmpeg -i 1.mp4 -b:v 512k -strict -2 -vf scale=720:480 2.mp4 
    // 512k为比特率
    
### 视频合并
	ffmpeg -f concat -i list.txt -c copy concat.mp4 
    // 在list.txt文件中，对要合并的视频片段进行描述
	// 内容如下:
	file inputcut1.mp4
	file inputcut2.mp4
    file inputcut3.mp4
    
### 修改帧率
	ffmpeg -i input.mp4 -qscale 0 -r 24 -y output.mp4
    // 统一修改为24帧
    
### 图片合成视频
	 ffmpeg  -framerate 1  -i img%d.jpg -r 25  output.mp4
	// -framerate 1 输入时帧率，即输入时每秒显示几张图片，此时每秒显示一张图片
    // -r 25 输出时帧率，即播放时每秒显示帧率。此时为25帧，对应输入，即每张图片扩展为25帧
    
## 3. 当遇到以下类似情况时，请使用Tab键选择确定或取消
![](/home/ckx/Moeditor/Ubuntu/img_choose.jpg)

## 4. 修改文件权限
	chmod -R +777 WhoJoy/
    // 修改整个文件夹的WhoJoy(包括内部的所有文件）的权限
    // +777 加上所有权限
    // -777 去掉所有权限
    
## 5. 脚本文件执行 sudo 命令（在脚本中输入密码）
	#! /bin/bash
	echo “password” | sudo apt-get update
    // 注意：这样脚本中会显示你的密码，可能会暴露密码
    
## 6. 重命名文件
	mv oldname newname
    
## 7. 
