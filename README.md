# AudioQueue
1、录制的音频编码格式是aac_ld

2、使用AudioQueue进行播放和录制

3、项目是根据苹果官方demo-SpeakHere改制而成

4、传输使用的是TCP协议

5、为了测试实时传输，我写了一个简单的服务器程序TestSV是服务器项目，做测试需要先启动TestSV项目，修改里面监听的ip和端口为你电脑的本
地ip,然后通过macox运行项目，项目运行起来后会有监听成功的log。

6、确保你的手机和电脑在同一个局域网内，启动RealTimeTransport
iPhone端项目，修改里面服务器地址和端口，同服务器端的一致,然后在iphone手机上运行项目，这时候自己说话自己就可以听到了。

7、客户端程序需要真机运行！否则无法录制音频。

8、主要修改了播放实时音频流有时候数据中断再有数据过来，播放器始终无声音。修复了此bug。测试方式：可以通过打断点的方式，截断几秒钟音频数据流，再把断点去掉播放器依然有效；或者在程序里面加一个计数，手动丢掉一些数据包，audioqueue无数据可播，然后再有数据过来时候播放器依然有效。

9、AudioMedia里面是libAudioMedia.a静态库的源码，里面主要是音频播放队列的维护以及音频播放代码。
其它地方所有人的代码应该都大同小异，都是来源于官方的demo SpeakHere，关键是添加静音包的一段代码：

// 填空数据
UInt32 allDataSize=fillBuf->mAudioDataByteSize;
BYTE buf[512];
memset(buf, 0, 512);
if (pDestData!=NULL) {
    memcpy(pDestData, buf, allDataSize);
    OSStatus err = AudioQueueEnqueueBuffer(mQueue, fillBuf, 0, NULL);
    if (err)
    {
        NSLog(@"没有数据了，填充空数据，填充失败");
    }
}
