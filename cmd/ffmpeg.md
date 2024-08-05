# FFmpeg

## Compress video by setting scale, rate(fps) and bit-rate:
```
ffmpeg -i "input.mp4"  -vf scale=-1:720 -r 25  -b:v 0.5M "output.mp4"

ffmpeg -ss 00:12:01 -to 00:15:21 -i input.mp4 -c:v copy output.mp4
```

## Concat video, file_name.txt likes:
```
file 'video/mov_00_op.webm'
file 'video/mov_00_title.webm'
file ...
```
```
ffmpeg -f concat -i file_names.txt -c copy -safe 0 concat.mp4
```

## Convert audio file format
```
ffmpeg -i audio.ogg -acodec libmp3lame audio.mp3
```