---
title: ffmpeg常用命令
date: 2019-04-27 18:25:49
tags:
---

记录下工作中常用的ffmpeg命令，有些网络上不好查找到，需要总结。用md不知道怎么排版比较合适

---

## 1. 编码

u_p444_b10的YUV→100M的YUV420的24fps的HDR10码流（单帧，每帧YUV重复2秒）

```bash
# u_p444_b10的YUV→100M的YUV420的24fps的HDR10码流（单帧，每帧YUV重复2秒）
# "如果要非单帧的，将前面一个""-r 0.5""去掉即可；重复10s，用""""-r 0.1""""；如果yuv仅有一帧，编码会有错误，所以至少2帧进行repeat"""
ffmpeg -threads 16 -r 0.5 -s 3840x2160 -pix_fmt yuv444p10 -i input.yuv -b:v 100M -vf scale=out_h_chr_pos=0:out_v_chr_pos=0,format=yuv420p10 -c:v libx265 -preset ultrafast -x265-params "keyint=24:colorprim=bt2020:transfer=smpte2084:colormatrix=bt2020nc:master-display=G(13250,34500)B(7500,3000)R(34000,16000)WP(15635,16450)L(10000000,50):max-cll=0,0" -an -y -r 24 output.mp4

# u_p444_b10的YUV→100M的YUV420的24fps的SDR码流（单帧，每帧YUV重复2秒）
ffmpeg -threads 16 -r 0.5 -s 3840x2160 -pix_fmt yuv444p10 -i input.yuv -b:v 100M -vf scale=out_color_matrix=bt709:out_h_chr_pos=0:out_v_chr_pos=0,format=yuv420p10 -c:v libx265 -preset ultrafast -x265-params "colorprim=bt709:transfer=bt709:colormatrix=bt709" -an -y -r 24 output.mp4

# u_p444_b10的YUV→100M的YUV420的24fps的HLG码流（单帧，每帧YUV重复2秒）
ffmpeg -threads 16 -r 0.5 -s 3840x2160 -pix_fmt yuv444p10 -i input.yuv -b:v 100M -vf scale=out_h_chr_pos=0:out_v_chr_pos=0,format=yuv420p10 -c:v libx265 -preset ultrafast -x265-params "keyint=24:colorprim=bt2020:transfer=arib-std-b67:colormatrix=bt2020nc" -an -y -r 24 output.mp4

# u_p420_b8的YUV→100M的YUV420p10的24fps的SDR码流（单帧，每帧YUV重复2秒）
ffmpeg -threads 16 -r 0.5 -s 1920x1080 -pix_fmt yuv420p -i input.yuv -b:v 100M -s 3840x2160 -vf scale=out_color_matrix=bt709:out_h_chr_pos=0:out_v_chr_pos=0,format=yuv420p10 -c:v libx265 -preset ultrafast -x265-params "colorprim=bt709:transfer=bt709:colormatrix=bt709" -an -y -r 24 output.mp4

# 720x480_p422_b8的YUV→100M的YUV422p8的24fps的SDR码流（单帧，每帧YUV重复2秒）
# 一帧即可完成，但是V811不支持这个格式，需要更换编码库
ffmpeg -threads 16 -r 0.2 -s 720x480 -pix_fmt yuv422p -i input.yuv -b:v 100M -c:v libx265 -preset ultrafast -x265-params -an -y -r 24 output.mp4

# 720x480_p422_b8的YUV→100M的YUV422p8的24fps的SDR码流（单帧，每帧YUV重复10秒）
# 注意至少需要两帧，但第二帧不会被repeated，总共只有10这里采用MPEG4编码库，不知道为何X265时V811支持不了
ffmpeg -r 0.1 -s 720x480 -pix_fmt yuv422p -i input.yuv -b:v 100M -vcodec mpeg4 -an -y -r 24 output.mp4

# 编码时重新添加旋转的metadata
ffmpeg -I input.mp4 -c copy -metadata:s:v:0 rotate=90 output.mp4
```

## 2. 解码

```bash
# 直接解码出YUV444的码流
ffmpeg -i input.mp4 -pix_fmt yuv444p10 -y output.yuv

# 解码YUV444的码流，并指定30帧和分辨率
ffmpeg -i input.mp4 -frames:v 30 -pix_fmt yuv444p10 -s 3840x2160 -y output.yuv

# 解码YUV444的码流，并指定时间段和分辨率
ffmpeg -i input.mp4 -ss 00:00:54 -t 00:00:10 -pix_fmt yuv444p10 -s 3840x2160 -y output.yuv

# 将码流分段解出yuv，每段持续2秒，总共最多解500帧，指定输出分辨率为f_p444_b10
# （ 注意直接使用命令行时用一个""%""，在脚本中要转义用""%%""）"
ffmpeg -i input.mp4 -frames:v 500 -pix_fmt yuv444p10 -s 1920x1080 -f segment -segment_time 2 -y output_%%04d.yuv

# 从制定时间开始截取一帧，并制定分辨率和格式
ffmpeg -i input.mp4 -ss 00:00:54 -frames:v 1 -pix_fmt yuv444p10 -s 3840x2160 -y output.yuv

# 解码时进行反交错deinterlace（有两种方法，但是主观效果差异应该不大）
ffmpeg -i input.mp4 -deinterlace -pix_fmt yuv444p10 output.yuv

# 缩放
ffmpeg -s 3840x2160 -pix_fmt yuv444p10 -i input.yuv -vf scale=iw*2:ih*2 output.yuvffmpeg -i input.jpg -vf scale=iw*w:ih*2 output.jpg
ffmpeg -i input.avi -vf scale=320:240 output.avi
ffmpeg -i input.avi -vf scale=320:-1 otuput.avi
ffmpeg -i input.avi -vf "scale=iw/2:ih/2" output.avi
ffmpeg -s 1920x1080 -pix_fmt yuv444p10 -i input.yuv -vf scale=iw*2:ih*2 output.yuv
ffmpeg -s 1920x1080 -pix_fmt gray10 -i input.yuv -vf scale=iw*2:ih*2 output.yuv
```

## 3. 拼接

```bash
# 将多段ts流拼接
ffmpeg -i concat:"u_r24_sdr_bt709.ts|u_r24_sdr_bt2020.ts|u_r24_sdr_pq.ts|u_r24_sdr_hdr10.ts|u_r24_sdr_hlg10.ts" -c copy -an -y u_r24_b10_sscimf.mp4

# 同时拼接4路；做8K用
ffmpeg -i 1.mp4 -i 2.mp4 -i 3.mp4 -i 4.mp4 -filter_complex "[0:v]pad=iw*2:ih*2[a];[a][1:v]overlay=w[b];[b][2:v]overlay=0:h[c];[c][3:v]overlay=w:h" out.mp4
```

## 4. 截取

```bash
# 从指定时间开始截取一段码流
ffmpeg -i input.mp4 -ss 00:00:05.2 -t 00:00:10 -c copy output.mp4

# 从指定时间开始截取一帧
ffmpeg -i input.mp4 -ss 00:00:05 -frames:v 1 -pix_fmt yuv444p10 -y output.yuv
```

## 5. 转码/scaler

```bash
# 将码流转码成420p10
ffmpeg -i input.mp4 -vf format=yuv420p10 output.mp4

# 将码流转码成fhd
ffmpeg -i input.mp4 -vf scale=1920:1080 output.mp4

# 将码流转码成uhd
# 但是HDR的metadata会丢失掉
ffmpeg -i input.mp4 -vf scale=3840:2160 output.mp4

# 重新拷贝音视频流，可修复视频时间戳不正确等问题
ffmpeg -i input.mp4 -c:v copy -c:a copy output.mp4

# 将UHD YUV转成FHD YUV
ffmpeg -s 3840x2160 -pix_fmt yuv444p10 -i input.yuv -vf scale=1920:1080 -pix_fmt yuv444p10 output.yuv

# 将UHD YUV444转成V210；"跟scalar效果基本一致，UV的值略偏右，目前没有看出来差异"
ffmpeg -s 3840x2160 -pix_fmt yuv444p10 -i input.yuv -vcodec v210 output.yuv

# 将V210转成YUV444 FHD
# 5529600刚好是一帧V210格式的大小，可能与标准的YUV422计算有一定的偏差，因为可能有偏移或者头 https://trac.ffmpeg.org/ticket/1869
ffmpeg -f image2pipe -vcodec v210 -s 1920x1080 -frame_size 5529600 -i 1920x1080_ducks_v210.yuv
```

## 6. 其他

```bash

# "将u_p444_b10的yuv图像以50M的码率编成420的1fps的SDR码流
# 5秒对应1帧原图，改变了帧率，STB播放异常，但是编码速度较快）
ffmpeg -threads 16 -r 0.2 -s 3840x2160 -pix_fmt yuv444p10 -i input.yuv -b:v 50M -vf scale=out_color_matrix=bt709:out_h_chr_pos=0:out_v_chr_pos=0,format=yuv420p10 -c:v libx265 -preset medium -x265-params  "colorprim=bt709:transfer=bt709:colormatrix=bt709" -an -y -r 1 output.mp4

# "将u_p444_b10的yuv图像以近无损方式编成10bit420的24fps的SDR码流（pq=0）
# 不过码流通常会过高，可能达到400M，一般不可行"""
ffmpeg -threads 16 -pix_fmt yuv444p10 -s 3840x2160 -r 24 -i input.yuv -vf scale=out_h_chr_pos=0:out_v_chr_pos=0,format=yuv420p10 -c:v libx265 -preset medium -x265-params qp=0:keyint=24:colorprim=bt709:transfer=bt709:colormatrix=bt709 -an -y otuput.mp4
```

## 7. 备注

1. 在编码的时候如果指定-preset ultrafast则速度会比-preset medium快不少，大约只要1/3的时间（单帧编码测试结果）。官方文档表示图像质量是一致的，文件大小有差异，但实测文件大小差异不大

## 8. ffprobe

```bash
# 获取帧数（速度很慢，需要解出来才能获取）
ffprobe -v error -count_frames -select_streams v:0 -show_entries stream=nb_read_frames -of default=nokey=1:noprint_wrappers=1 input.mp4

# 说是上面一个方法的快速版本，直接查询容器
ffprobe -v error -count_frames -select_streams v:0 -show_entries stream=nb_frames -of default=nokey=1:noprint_wrappers=1 input.mp4
```

## 9. mediainfo

```bash
# 获取帧数（速度比ffprobbe快很多）
mediainfo --Output="Video;%FrameCount%" input.mp4
```

## 10. ref

- [多个视频文件合成画中画效果（Python、ffmpeg）](https://www.cnblogs.com/wolf-song/p/7708364.html)
- [ffmpeg 命令行实现多路视频拼接播放](https://blog.csdn.net/weiwen12138/article/details/70344681)]