## 1. What can cause a stutter during LVB?
![](//mc.qcloudimg.com/static/img/b41b15c344f8c34011e4ee0e55db21e5/image.png)

Generally, there are three reasons for the stutter:
- **Reason A - Low frame rate**
If the VJ uses a low-end phone, or there are CPU intensive applications running at the background, the frame rate of the video could be low. Typically, for an LVB to play smoothly, the frame rate of the video stream should be higher than 15 FPS. A frame rate lower than 10 FPS is **too low**, and can cause a stutter at all the viewer ends.

- **Reason B - Upstream Clog**
When pushing, VJs' phones generate audio and video data constantly. If the upstream bandwidth of a phone is too low, the generated audio and video data could clog the phone network and fails to be pushed, causing the stutter at all the viewer ends.

 Even though **domestic ISPs** offer broadband packages with a downstream bandwidth as fast as 10 Mbps, 20 Mbps or even 100 Mbps, the upstream bandwidth is highly limited. In many small cities, the upstream bandwidth is limited to 512 Kbps (i.e. a maximum of 64 KB data can be uploaded per second).
 
 **WiFi** follows the IEEE 802.11 specification of carrier-sense multiple access and collision avoidance (CSMA/CA). To put it simply, a WiFi hot spot can communicate with only one phone at one time, and other phones must verify or query if communication is possible before initiating a connection to a hot spot. Therefore, the more people using a WiFi hot spot, the slower the connection is. Furthermore, WiFi signal decays greatly when passing through walls or obstacles, and most of the families seldom take the WiFi router position and the strength of WiFi signal across rooms into consideration during the design and decoration of their houses. Even the VJs themselves probably don't know how many walls are there between their routers and the rooms where they push video streams.

- **Reason C - Bad Downstream Connection**
That is, the viewer's downstream bandwidth is insufficient or the network condition is unstable. For example, suppose the bit rate of an LVB stream is 1 Mbps (i.e. every second 1 M bits of data need to be downloaded). If the bandwidth at viewer end is not fast enough, the viewer would experience serious stutter. Bad downstream connection only affects the viewers in the current network environment.

## 2. Status Monitor
The RTMP SDK provides a status feedback mechanism, by which the RTMP SDK reports various status parameters every 1-2 seconds. You can register the **TXLivePushListener** listener to obtain these status parameters.

![](//mc.qcloudimg.com/static/img/48fd46af4e17b0299fd00a0e661a16f0/image.png)

|  Push Status                   |  Description                    |   
| :------------------------  |  :------------------------ | 
| NET_STATUS_CPU_USAGE | The CPU utilization for current process and the overall CPU utilization for the machine |
| NET_STATUS_VIDEO_FPS | Current video frame rate, that is, the number of frames produced by video encoder per second |
| NET_STATUS_NET_SPEED | Current transmission speed (in Kbps) |
| NET_STATUS_VIDEO_BITRATE | The output bit rate of the current video encoder, i.e., the number of video data bits produced by the encoder per second  (in Kbps) |
| NET_STATUS_AUDIO_BITRATE | The output bit rate of the current audio encoder, i.e., the number of audio data bits produced by the encoder per second  (in Kbps) |
| NET_STATUS_CACHE_SIZE | Accumulated audio/video data size. A value ??? 10 indicates the current uplink bandwidth is not enough to consume the audio/video data produced |
| NET_STATUS_CODEC_DROP_CNT | The number of global packet drops. To avoid a vicious accumulation of delays, the SDK can actively drop packets when the accumulated data exceeds the threshold. A higher number of packet drops means a more severe network problem. |
| NET_STATUS_SERVER_IP     | The IP address of the connected push server. It is typically the nearest one within few hops from the client.   |


## 3. Low Frame Rate
### 3.1 How to verify if the frame rate is too low
We can obtain the video frame rate of current push from the **VIDEO_FPS** status data of TXLivePushListener. Typically, for an LVB to play smoothly, the frame rate of the video stream should be higher than 15 FPS. A frame rate lower than 10 FPS could cause an obvious stutter at the viewer end.

### 3.2 Solutions
- **3.2.1 Observe CPU_USAGE value**
You can obtain the **CPU utilization for the current push SDK** and **overall CPU utilization for the system** from **CPU_USAGE** status data of TXLivePushListener.

 If the overall CPU utilization for the system exceeds 80%, video capture and encoding may be affected; if the CPU utilization reaches 100%, the VJ end may be terribly stuck and it is impossible for the viewers to have a smooth viewing experience.

- **3.2.2 Identify the high CPU consumers**
On an LVB App, in addition to RTMP SDK, many other features such as on-screen comments, floating stars, and interactive text messages can consume some CPU resources. To test and evaluate the CPU utilization for just push SDK, use this [simple DEMO](https://cloud.tencent.com/document/product/454/6555).

- **3.2.3 Choose a reasonable resolution**
A higher resolution doesn't always come with better video quality: firstly, a high resolution needs a higher bit-rate to work; a definition with a low bit rate and high resolution is often inferior to that with a high bit rate and a low resolution. Secondly, a high resolution such as 1280*720 does not have an obvious advantage on a 5'' phone screen. Only when the LVB is played full-screen on a PC can the resolution of 1280*720 make a significant difference than 960 * 540.

 However, a higher resolution can bring about a big increase of CPU usage for SDK. Therefore, in most cases, it's recommended to simply set the video quality to **High Definition** with TXLivePush's setVideoQuality. A higher resolution isn't always the better.

- **3.3.4 Use hardware acceleration if appropriate**
Most smart phones support hardware encoding to lower the CPU usage for video encoding. 
When CPU usage is too high for an App, you can enable hardware encoding to lower the CPU usage.

 By default, the **High Definition** video quality option of TXLivePush's setVideoQuality uses software encoding (on some Android devices, hardware encoding doesn't function well due to the severe mosaics). You can use enableHWAcceleration of TXLivePushConfig to enable hardware encoding.

## 4. Upstream Clog
According to statistics, upstream clog at VJ end is responsible for over 80% of stutters among the video cloud's customers.

### 4.1 Identify Upstream Clog
- **4.1.1 : Relation between BITRATE and NET_SPEED**
 BITRATE (= VIDEO_BITRATE + AUDIO_BITRATE) refers to the number of audio/video data bits produced by the encoder for push per second; NET_SPEED refers to the number of data bits pushed actually per second. A long duration of BITRATE == NET_SPEED means a good push quality. However, a long duration of BITRATE >= NET_SPEED indicates a bad push quality.

- **4.1.2. CACHE_SIZE and DROP_CNT Values**
Once BITRATE >= NET_SPEED, the audio/video data produced by the encoder will build up on VJ's phone, with the severity indicated by the CACHE_SIZE value. When the CACHE_SIZE value exceeds the warning level, SDK will actively drop some audio/video data, thus triggering an increment of DROP_CNT.

 The figure below shows a typical upstream clog, with CACHE_SIZE remaining above the red warning level. This means that the upstream bandwidth doesn't meet the data transmission requirements (i.e. the upstream network is severely clogged).

![](//mc.qcloudimg.com/static/img/319d6197da603ca15ffc6e2afd778e48/image.png)

![](//mc.qcloudimg.com/static/img/e241222c0591e6b5ffa41738a8a35d62/image.png)
 > PS: The figure similar to the above can be found in ["LVB Console" -> "Quality Monitor"](https://console.cloud.tencent.com/live/livesdk).


### 4.2 Solutions
- **4.2.1 Notify VJ of the bad network condition**
In a scenario where video quality is important, it's the best practice to notify the VJ through appropriate UI interactions, such as **"The network condition is bad now. Please move closer to your router, and make sure the signal isn't blocked by any wall or obstacle."**  

 For more information on how to do this, please see the **Event Handling** section in RTMP SDK's documentation about push. VJs generally are not aware of the upstream clog until receiving a notification from the App or a viewer. Therefore, it is recommended to remind the VJ about the network condition if the App receives multiple **PUSH_WARNING_NET_BUSY** events from RTMP SDK in a short time.

- **4.2.2 Proper encoding settings**
The following shows the recommended encoding settings (suitable for Beauty Show LVB. For more information, please see [How to Improve Video Quality?](https://cloud.tencent.com/document/product/454/7955)). You can set deferent video quality options using API setVideoQuality of TXLivePush.

| Option  | Resolution | FPS | Bit Rate | Scenario | 
|:-------:|---------|---------|:-------:|---------|
| **Standard Definition** | 360\*640 | 15 | 400-800 Kbps | Choose this option if you are more concerned about bandwidth cost. This option can bring a blurred video quality but reduce bandwidth cost by 60% compared to a high definition.  |
| **High Definition**<br>(recommended) | 540\*960 | 15 | 1200 Kbps | If you're more focused on video quality, select this option, which allows most mainstream mobile phones to present clear pictures.  |
| **Ultra High Definition** | 720\*1280 | 15 | 1800 Kbps | This option is not recommended for scenarios where videos are mostly viewed in small screens. You can consider using this option if videos are viewed in large screens and the VJ has a great network. |

- **4.2.3 Enable traffic control**
Some clients may complain: "Any user can use our App. It's impossible to control their network conditions."

 If VJs' upstream bandwidth varies significantly, it's recommended to enable network adaption by referring to the documentation for [iOS](https://cloud.tencent.com/document/product/454/7884#4.-.E6.99.BA.E8.83.BD.E6.8E.A7.E9.80.9F) and [Android](https://cloud.tencent.com/document/product/454/7890#4.-.E6.99.BA.E8.83.BD.E6.8E.A7.E9.80.9F).

 However, the solution described in **4.2.1 Notify VJ of network condition** is the preferred one. After all, it's unrealistic to ensure the smoothness and high definition without a high upstream bandwidth. You cannot eat your cake and have it too.

| Field | Definition | Recommended Value |
|---------|------------|--------------|
| MinVideoBitrate | The minimum video bit rate | 400 |
| MinVideoBitrate | The maximum video bit rate | 1,000 (the recommended value depends on the resolution) |
| AutoAdjustBitrate | Bit rate adaption | Enabled |
| setAutoAdjustStrategy | Bit rate adjustment strategy | AUTO_ADJUST_BITRATE_STRATEGY_2 |


## 5. Player Optimization
![](//mc.qcloudimg.com/static/img/9ccfcf56c0993232cc5637f306c21ba5/image.png)

### 5.1 Stutter & Lag
As shown in the figure above, instable downstream network or insufficient downstream bandwidth could cause **starvation periods** during playback. During these periods, the App doesn't receive enough audio and video data to play. To minimize the incidence of stutters at viewer end, you need to make the App cache as much video data as possible to survive the "starvation periods".

However, caching too much audio and video data brings a new problem - **high delay**, a bad news for scenarios with a high requirement for interactions between VJ and viewers. Moreover, if the delay caused by stutter goes uncontrolled without any correction, it could *accumulate over time* (i.e. the longer the playback lasts, the higher the delay). Delay correction is a key indicator of an excellent player.

Therefore, **delay and smoothness** are the two ends of a balance, focusing too much on low delay will lead to slight network fluctuations that produce significant stutter at the player side. Conversely, overemphasis on smoothness will cause high delay. A typical case is the HLS (m3u8) protocol, which ensures a smooth playback experience by introducing a delay of 10-30 seconds.

### 5.2 Solutions
To allow you to get a better playback experience without the need to have much knowledge about traffic control and processing, Tencent Cloud RTMP SDK, being optimized over several versions, features a set of automatic adjustment technologies, based on which three excellent delay control schemes are introduced:

- **Auto*: use this mode if you are unsure about what is your main scenario.
> You can enter the Auto mode by turning on the setAutoAdjustCache switch in TXLivePlayConfig. In this mode, the player automatically adjusts delay based on current network condition (by default, the player automatically adjusts delay within the range of 1-5 seconds. You can use setMinCacheTime and setMaxCacheTime to modify the default) to minimize the delay between VJ and viewer while ensuring sufficient smoothness, and thus to deliver a good interactive experience.

- **Speedy**: Suitable for **Showtime LVB** and other scenarios with a high requirement for delay.
> Speedy mode (set by making **SetMinCacheTime = setMaxCacheTime = 1 second**) and Auto mode only differ in MaxCacheTime value (generally, MaxCacheTime is lower in Speedy mode and is higher in Auto mode). This flexibility can be largely attributed to the automatic control technology within the SDK, which automatically adjusts delay without causing stutter. MaxCacheTime is used to indicate the adjustment speed - the higher the MaxCacheTime value is, the more conservative the adjustment speed is, and therefore the lower the probability of stutter becomes.
 
- **Smooth**: Suitable for **Game LVB** and other HD (high bit rate) LVB scenarios.
> You can enter the Smooth mode by turning off setAutoAdjustCache switch in the player. In this mode, the player uses a processing strategy similar to the caching strategy of the Adobe FLASH kernel. When stutter occurs in a video, the video will go into the loading status until the cache is full; then it will go into the playing status until the next network fluctuation that can't be resisted. By default, the cache time is 5 seconds, which you can change using setCacheTime.
> 
> This seemingly simple mode will be more reliable in scenarios with a low requirement for delay, because the mode in essence trades off delay slightly for a reduced stutter rate.






