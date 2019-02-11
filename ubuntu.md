# Ubuntu
## 1. 指令安装deb包
	sudo dpkg -i package.deb

## 2. ffmpeg截取一段视频
	ffmpeg  -i input.mp4 -vcodec copy -acodec copy -ss 00:00:10 -to 00:02:10 outcut.mp4 -y // 截取input.mp4从10秒到2分10秒中间的两分钟视频，并保存为outcut.mp4
