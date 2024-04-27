# blob 视频下载



{% code fullWidth="true" %}
```
//fang
1. 在视频页面，按F12，在 network filter里输入.m3u8， 
   刷新浏览器， 出现的就是m3u8地址。

2. 复制 m3u8 地址。

3. ffmpeg 下载


ffmpeg -i "https://video-hw.qianliaowang.com/asset/..." -bsf:a aac_adtstoasc -vcodec copy -c copy -crf 50 file.mp4
```
{% endcode %}

{% code fullWidth="true" %}
```
// 方法2
pip install you-get
you-get [url] 

```
{% endcode %}
