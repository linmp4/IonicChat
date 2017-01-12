# 融云 RongCloud Cordova IM v2.0

## Environment Setup

### 初始化开发工具

```
npm install -g cordova ionic

如有必要，使用 `sudo npm`

### 安装依赖库

新建ionic空项目,将package.json和www目录覆盖到新项目下,再执行
ionic state restore
```
由于 ionic 版本问题,执行该命令可能会出现问题,主要问题如下
***** 如果提示缺少 .project 可以将 ionic.config.json 文件复制一份,改名为 ionic.project
***** Error: Cannot find module './project'  参考 https://github.com/driftyco/ionic-cli/issues/856


### 特别说明
1. demo 不可以直接运行,需要配置相关开发环境
2. 发送语音信息需要注意: demo 中用到了 cordova-plugin-media ,这个插件需要在 android 和 ios 的源码中修改几个参数
发送语音需要长按 喇叭按钮
3. demo在以下环境中分别在 android 和 ios 端测试通过
     node 4.4.3  
     cordova: 5.3.3
  主要相关的插件版本分别为
     cordova-plugin-media  1.0.1
     cordova-plugin-camera 2.2.0
     cordova-plugin-file   3.0.0
  如果您的demo中和以上插件版本不同,可能需要调整插件用法,请自行处理
4. demo 暂用老版本的后台,所以暂不支持注册和加好友,后续会改为新版本的后台,和融云官网的 SealTalk 互通
5. demo和插件仅支持在移动端运行,支持 android 和 ios

### ios  
appFolder/platforms/ios/appFolder/Plugins/cordova-plugin-media/CDVSound.m   audioFile.recorder 前加入

```
NSDictionary *recordSetting = @{AVFormatIDKey: @(kAudioFormatLinearPCM),
                               AVSampleRateKey: @8000.00f,
                               AVNumberOfChannelsKey: @1,
                               AVLinearPCMBitDepthKey: @16,
                               AVLinearPCMIsNonInterleaved: @NO,
                               AVLinearPCMIsFloatKey: @NO,
                               AVLinearPCMIsBigEndianKey: @NO};
```

改完后结果为

```
NSDictionary *recordSetting = @{AVFormatIDKey: @(kAudioFormatLinearPCM),
                               AVSampleRateKey: @8000.00f,
                               AVNumberOfChannelsKey: @1,
                               AVLinearPCMBitDepthKey: @16,
                               AVLinearPCMIsNonInterleaved: @NO,
                               AVLinearPCMIsFloatKey: @NO,
                               AVLinearPCMIsBigEndianKey: @NO};
audioFile.recorder = [[CDVAudioRecorder alloc] initWithURL:audioFile.resourceURL settings:recordSetting error:&error];

```

###  android
   appfoldere/platforms/android/src/org/apache/cordova/media/AudioPlayer.java 中  startRecording中  case NONE: 中加入并替换为

```
   this.recorder.setAudioSamplingRate(8000);
   this.recorder.setAudioEncodingBitRate(7950);
   this.recorder.setOutputFormat(MediaRecorder.OutputFormat.AMR_NB);
   this.recorder.setAudioEncoder(MediaRecorder.AudioEncoder.AMR_NB);
```
