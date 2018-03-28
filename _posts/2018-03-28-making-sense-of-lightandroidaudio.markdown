---
layout: post
title: "Making sense of light-android-audio"
date: "2018-03-28 18:21:38 +0200"
---

Notes while reading the [code for decoding mp3 in android with ffmpeg](https://github.com/Mercandj/light-android-audio)


## Main Components

- Filemanager
- SoundSystem
- AudioFeaturesManager

## Decoding flow (mp3 file -> fill PCM buffer)

Click on button  *btn_extract_file_ffmpeg_native_thread* --> loadTrackFFmpegNativeThreadOrAskPermission()

*MainActivity:*
```java
path = fileManager.getFile().getAbsolutePath();
soundSystem.loadFileFFmpegNativeThread(path);
```

*SoundSystemImpl:*
```java
public void loadFileFFmpegNativeThread(final String filePath) {
    soundSystemEntryPoint.native_load_file_ffmpeg(filePath);
}
...
/* package */
native void native_load_file_ffmpeg(String filePath);

```

***entering JNI land***

*SoundsystemEntrypoint.cpp:*
```c
void Java_com_mercandalli_android_sdk_audio_SoundSystemEntryPoint_native_1load_1file_1ffmpeg(
        JNIEnv *env,
        jclass jclass1,
        jstring filePath) {
    if (!isSoundSystemInit()) {
        return;
    }
    const char *urf8FileURLString = env->GetStringUTFChars(filePath, NULL);
    _ffmpegSingleThreadExtractor->extract(urf8FileURLString);
}
```
*FFmpegSingleThreadExtractor.h:*

```cpp
typedef struct {
    AVFormatContext *format;
    AVStream *stream;
    AVCodecContext *codec;
    unsigned short _device_frame_rate;

    bool sawInputEOS;
    bool sawOutputEOS;

    SoundSystem *soundSystem;
    short *extractedData;

    double extractionTimeStart;
    unsigned int extractionPosition;
    unsigned short numberChannel;

} WorkerData;
```


*FFmpegSingleThreadExtractor.cpp:*
```cpp
oid *FFmpegSingleThreadExtractor::doExtraction(void *) {
    WorkerData *d = &workerData;
...

// append resampled frames to data
 d->extractedData = (short *) realloc(
         d->extractedData,
         (size + frame->nb_samples) * sizeof(short) * 2);
 memcpy(d->extractedData + size,
        buffer,
        frame_count * sizeof(short) * 2);
 size += frame_count * 2;
```

## Play music

*MainActivity:*
```java
private CompoundButton.OnCheckedChangeListener mOnCheckedChangeListener = new CompoundButton.OnCheckedChangeListener() {
    @Override
    public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
        switch (buttonView.getId()) {
            case R.id.toggle_play_pause:
                soundSystem.playMusic(isChecked);
                break;
        }
    }
};
```
*SoundSystemEntryPoint:*
```java
/* package */
native void native_play(boolean play);
```

```c++
void Java_com_mercandalli_android_sdk_audio_SoundSystemEntryPoint_native_1play(
        JNIEnv *env,
        jclass jclass1,
        jboolean play) {
    if (!isSoundSystemInit()) {
        return;
    }

#ifdef AAUDIO
    if (play) {
        _aaudio_manager->start();
    } else {
        _aaudio_manager->stop();
    }
#else
    _soundSystem->play(play);
#endif
}
```
