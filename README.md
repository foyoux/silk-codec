# silk-codec
来源 https://github.com/kn007/silk-v3-decoder ，改用 cmake 编译系统，添加 中文路径支持 ，提供 64 位版本，详细使用说明，当然也支持微信语音

关于 Python 的 silk 绑定，请看我的另一个项目 **[pilk](https://github.com/foyoux/pilk)**

## 简单说明
> 细节可参考我的另一个项目 **[pilk](https://github.com/foyoux/pilk)** README

**Release** 中提供的可执行程序是命令行工具，它的作用是将 **PCM文件** 与 **SILK 格式文件** 进行互转，至于 **PCM文件** 哪里来，就是其他工具的问题了。

可以用各种工具，但是比较通用的就是 **[ffmpeg](https://www.ffmpeg.org/)** , 下文以此介绍


## PCM 文件的转换
1. 音(视)频文件 ➜ PCM
   > 借助 ffmpeg，你当然需要先有 [ffmpeg](https://www.ffmpeg.org/download.html)
    ```bat
    ffmpeg -y -i <音(视)频输入文件> -vn -ar <采样率> -ac 1 -f s16le <PCM输出文件>
    ```
    1. `-y`: 可加可不加，表示 <PCM输出文件> 已存在时不询问，直接覆盖
    2. `-i`: 没啥好说的，固定的，后接 <音(视)频输入文件>
    3. `-vn`: 表示不处理视频数据，建议添加，虽然不加也不会处理视频数据（视频数据不存在转PCM的说法），但可能会打印警告
    4. `-ar`: 设置采样率，可选的值是 [8000, 12000, 16000, 24000, 32000, 44100, 48000], 这里你可以直接理解为声音质量
    5. `-ac`: 设置声道数，在这里必须为 **1**，这是由 **SILK** 决定的
    6. `-f`: 表示强制转换为指定的格式，一般来说必须为 **s16le**, 表示 `16-bit short integer Little-Endian data`
    7. example1: `ffmpeg -y -i mv.mp4 -vn -ar 44100 -ac 1 -f s16le mv.pcm`
    8. example2: `ffmpeg -y -i music.mp3 -ar 44100 -ac 1 -f s16le music.pcm`


2. PCM ➜ 音频文件
    ```bat
    ffmpeg -y -f s16le -i <PCM输入文件> -ar <采样率> -ac <声道数> <音频输出文件>
    ```
    1. `-f`: 这里必须为 `s16le`, 同样也是由 **SILK** 决定的
    2. `-ar`: 同上
    3. `-ac`: 含义同上，值随意
    4. `<音频输出文件>`: 扩展名要准确，没有指定格式时，**ffmpeg** 会根据给定的输出文件扩展名来判断需要输出的格式
    5. example3: `ffmpeg -y -f s16le -i test.pcm test.mp3`


## silk 命令行命令行的使用

1. 编码工具
    ```bash
    silk-encoder in.pcm out.silk -Fs_API <必须是ffmpeg命令中 -ar 参数值> -rate <随意 这里可以理解为声音质量, 建议与 -Fs_API 参数相同>
    ```
    1. 其他参数见命令行帮助信息

2. 解码工具
   ```bash
   silk-decoder in.silk out.pcm
   ```
   1. 详细信息见命令行帮助信息

https://user-images.githubusercontent.com/35125624/150665992-8c7ef940-71bb-42c5-b7a1-7e0559ae539c.mp4
