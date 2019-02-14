# Ubuntu
## 1. 指令安装deb包
	sudo dpkg -i package.deb

## 2. 一些ffmpeg指令
### 截取视频
	ffmpeg  -i input.mp4 -vcodec copy -acodec copy -ss 00:00:10 -to 00:02:10 outcut.mp4 -y // 截取input.mp4从10秒到2分10秒中间的两分钟视频，并保存为outcut.mp4
    
### 转换视频格式
	ffmpeg -i  testrmvb.rmvb  -c:v libx264 -strict -2 testrmvb.mp4
    
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
    
## 3. 当遇到以下类似情况时，请使用Tab键选择确定或取消
![](/home/ckx/Moeditor/Ubuntu/img_choose.jpg)
