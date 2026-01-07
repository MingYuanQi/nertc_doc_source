<!--keywords: 音视频通话，RTC，场景模式，分辨率，帧率 -->

本文介绍如何使用超过 JavaScript 语法 Number 数据类型范围的 userID 加入房间。在 userID 的基础上增加 userStringID，如果 userID 的实际值超过 Number 范围，其精度会缺失，这种情况下，SDK 会使用 userStringID 来代替 userID，其 String 格式不用担心数值对应不准的问题，用户业务上需要 userStringID 做逻辑的时候，注意其 String 格式的特征即可。

## 主要API增加 userStringID 参数

### 加入房间（`joinChannel`）

``` javascript
// 假设用户需要使用19位数的 userID
const myStringUid = '1234567890123456789';
this.engine.joinChannel({
    token: '',
    channelName: 'xxx',
    myUid: BigInt(myStringUid), // 超过的 number 范围，精度不会缺失
    myStringUid // 使用字符串格式传递 userID，可以不担心精度缺失的问题
});
```
### 设置和订阅远端视频

- 设置远端视频画布（`setupRemoteVideoCanvas`）
- 订阅远端视频（`subscribeRemoteVideo`）

```javaScript
//SDK会增加userStringID参数用于表示字符串格式的userID，用于userID超过number范围的场景
this.engine.addEventListener("onUserVideoStart", (userID, maxProfile, userStringID) => {
	
	const message = `onUserVideoStart通知：对方开启视频，userID = ${userID}, userStringID = ${userStringID}, maxProfile = ${maxProfile}`
	console.log(message)
	//设置对端的视频画布
	this.engine.setupRemoteVideoCanvas({
		renderMode: NERTCRenderMode.Fit, // Fit表示应区域。视频尺寸等比缩放,保证所有区域被填满,视频超出部分会被裁剪
		mirrorMode: NERTCMirrorMode.AUTO, //AUTO表示使用默认值,由SDK控制
		isMediaOverlay: false, //表示小画布置于大画布上面
		userID, //对方userID
		userStringID //对方userID，如果超过number范围，使用字符串格式传递userID，可以不担心精度缺失的问题
	})
	//主动去订阅对端视频
	this.engine.subscribeRemoteVideo({
		userID,
		userStringID, //对方userID，如果超过number范围，使用字符串格式传递userID，可以不担心精度缺失的问题
		streamType: NERtcRemoteVideoStreamType.HIGH, //HIGH: 大流, LOW: 小流 
		SUBscribe: true //true表示订阅，false表示取消订阅
	})
});
```

## 主要回调事件增加 userStringID 参数

### 本端加入房间结果的通知（`onJoinChannel`）

``` javascript
// SDK会增加userStringID参数用于表示字符串格式的userID，用于userID超过number范围的场景
this.engine.addEventListener("onJoinChannel", (result, channelId, elapsed, userID, userStringID) => {
    let message = `onJoinChannel通知：自己加入房间状况，result = ${result}, channelId = ${channelId}, elapsed = ${elapsed}, userID = ${userID}, userStringID = ${userStringID}`;
    this.engine.nertcPrint(message);
    console.log(message);
});
```

### 远端加入房间结果的通知（`onUserJoined`）

- `onUserLeave` 同理。

``` javaScript
//SDK会增加userStringID参数用于表示字符串格式的userID，用于userID超过number范围的场景
this.engine.addEventListener("onUserJoined", (userID, customInfo, userStringID) => {
	const message = `onUserJoined通知：有人加入房间，userID = ${userID}, userStringID = ${userStringID}, customInfo = ${customInfo}`
	this.engine.nertcPrint(message)
	console.log(message)
});
```

### 远端加入房间结果的通知（`onUserVideoStart`）

- `onUserAudioStart` 等同理。

``` javaScript
//SDK会增加userStringID参数用于表示字符串格式的userID，用于userID超过number范围的场景
this.engine.addEventListener("onUserVideoStart", (userID, maxProfile, userStringID) => {
	const message = `onUserVideoStart通知：对方开启视频，userID = ${userID}, userStringID = ${userStringID}, maxProfile = ${maxProfile}`
	this.engine.nertcPrint(message)
	console.log(message)
});
```

### 网络状态的通知（`onNetworkQuality`）

- `onRemoteAudioVolumeIndication` 等同理。

``` javaScript

this.engine.addEventListener("onNetworkQuality", (statsArray) => {
	statsArray.forEach((user)=>{
		//SDK会增加userStringID参数用于表示字符串格式的userID，用于userID超过number范围的场景
		console.log(`用户 userID=${user.userID}, userStringID=${user.userStringID} 的上行网络质量为: ${user.upStatus}, 下行网络质量为: ${user.downStatus} `)
		this.engine.nertcPrint(`用户 userID=${user.userID}, userStringID=${user.userStringID} 的上行网络质量为: ${user.upStatus}, 下行网络质量为: ${user.downStatus} `)
	})
});
```

## 完整的示例代码

```javaScript
import permision from "@/NERtcUniappSDK-JS/permission.js";
import NERTC from "@/NERtcUniappSDK-JS/lib/index";
import NertcLocalView from "@/NERtcUniappSDK-JS/nertc-view/NertcLocalView";
import NertcRemoteView from "@/NERtcUniappSDK-JS/nertc-view/NertcRemoteView";

export default {
    components: {
        NertcRemoteView: NertcRemoteView,
        NertcLocalView: NertcLocalView
    },
    data() {
        engine: null,
        remoteUserIdVideoList: [],
    },
    methods: {
        createEngine() {
            console.log('初始化NERTC引擎');
            this.engine = NERTC.setupEngineWithContext({
                appKey: appkey, // your appkey
                logDir: logDir || '', // expected log directory
                logLevel: logLevel || 3
            });
            //初始化引擎完成，先监听SDK的重要事件
			this.addEventListener()
            console.log('初始化引擎完成，开始设置本地视频画布');
			this.engine.setupLocalVideoCanvas({
                renderMode: 0, // 0表示使用视频，视频等比缩放，1表示适应区域，会裁剪，2：折中方案 
                mirrorMode: 1, //1表示启用镜像，2表示不启用
                isMediaOverlay: false //表示小画布置于大画布上面
            })
            
            console.log('初始化引擎完成，启动视频')
            this.engine.enableLocalVideo({
                enable: true, //true表示设置启动摄像头，false表示关闭摄像头
                videoStreamType: 0 //0表示视频，1表示屏幕共享 
            })

            //判断权限
            if (uni.getSystemInfoSync().platform === "android") {
                permision.requestAndroidPermission(
                    "android.permission.RECORD_AUDIO"
                );
                permision.requestAndroidPermission(
                    "android.permission.CAMERA"
                );
            }
        },
        addEventListener(){

				//注册NERTC的事件
				this.engine.addEventListener("onError", (code, message, extraInfo) => {
					const imessage = `onError通知：code = ${code}, message = ${message}, extraInfo = ${extraInfo}`
					this.engine.nertcPrint(imessage)
					console.log(imessage)
				});
				this.engine.addEventListener("onWaring", (code, message, extraInfo) => {
					const imessage = `onWaring通知：code = ${code}, message = ${message}, extraInfo = ${extraInfo}`
					this.engine.nertcPrint(imessage)
					console.log(imessage)
				});
				//重要：UI视频view渲染的时间点不确定，有可能会出现设置videoCanvas时，对应的视频view还没有渲染好，导致设置videoCanvas失败，用户需要监听该事件去重新设置对应的videoCanvas
				this.engine.addEventListener("onVideoCanvas", (direction, videoStreamType, userID, userStringID) => {
                    //userStringID为userID的String格式，如果userID的实际值超过了Number范围，请使用userStringID进行相关逻辑业务

					const imessage = `onVideoCanvas通知：direction = ${direction}, videoStreamType = ${videoStreamType}, userID = ${userID}, userStringID = ${userStringID}`
					this.engine.nertcPrint(imessage)
					console.log(imessage)
					if (direction == 'local') {
						if (videoStreamType == 'video') {
							//重新设置本地Video（即摄像头）的画布
							this.engine.setupLocalVideoCanvas({
								renderMode: 0, // 0表示使用视频，视频等比缩放，1表示适应区域，会裁剪，2：折中方案
								mirrorMode: 1, //1表示启用镜像，2表示不启用
								isMediaOverlay: false //表示小画布置于大画布上面
							})
						} else if(videoStreamType == 'subStreamVideo') {
							//重新设置本地subStramVideo(即屏幕共享)的画布
							this.engine.setupLocalSubStreamVideoCanvas({
								renderMode: 0, // 0表示使用视频，视频等比缩放，1表示适应区域，会裁剪，2：折中方案
								mirrorMode: 1, //1表示启用镜像，2表示不启用
								isMediaOverlay: false //表示小画布置于大画布上面
							})
						}
						
					} else if(direction == 'remote'){
						if (videoStreamType == 'video') {
							//重新设置远端Video（即摄像头）的画布
							this.engine.setupRemoteVideoCanvas({
                                renderMode: NERTCRenderMode.Fit, // Fit表示应区域。视频尺寸等比缩放,保证所有区域被填满,视频超出部分会被裁剪
                                mirrorMode: NERTCMirrorMode.AUTO, //AUTO表示使用默认值,由SDK控制
                                isMediaOverlay: false, //表示小画布置于大画布上面
                                userID,
                                userStringID //SDK优先采用userStringID格式的数字
                            })
						} else if(videoStreamType == 'subStreamVideo') {
							//重新设置远端subStramVideo(即屏幕共享)的画布
							this.engine.setupRemoteSubStreamVideoCanvas({
                                renderMode: NERTCRenderMode.Fit, // Fit表示应区域。视频尺寸等比缩放,保证所有区域被填满,视频超出部分会被裁剪
                                mirrorMode: NERTCMirrorMode.AUTO, //AUTO表示使用默认值,由SDK控制
                                isMediaOverlay: false, //表示小画布置于大画布上面
                                userID,
                                userStringID //SDK优先采用userStringID格式的数字
                            })
						}
					}
				});
				this.engine.addEventListener("onJoinChannel", (result, channelId, elapsed, userID, userStringID) => {
                    //userStringID为userID的String格式，如果userID的实际值超过了Number范围，请使用userStringID进行相关逻辑业务

					let message = `onJoinChannel通知：自己加入房间状况，result = ${result}, channelId = ${channelId}, elapsed = ${elapsed}, userID = ${userID}, userStringID = ${userStringID}`
					this.engine.nertcPrint(message)
					console.log(message)
				});
				
				this.engine.addEventListener("onClientRoleChange", (oldRole, newRole) => {
					const message = `onClientRoleChange通知：oldRole=${oldRole}, newRole=${newRole}`
					this.engine.nertcPrint(message)
					console.log(message)
				});
				
				
				this.engine.addEventListener("onReconnectingStart", (result) => {
					const message = `onReconnectingStart通知：开始重连`
					this.engine.nertcPrint(message)
					console.log(message)
				});
				this.engine.addEventListener("onReJoinChannel", (result) => {
					const message = `onReJoinChannel通知：自己重新加入房间状况，result = ${result}`
					this.engine.nertcPrint(message)
					console.log(message)
				});
				this.engine.addEventListener("onDisconnect", (reason) => {
					const message = `onDisconnect通知：断开reason = ${reason}`
					this.engine.nertcPrint(message)
					console.log(message)
				});
				this.engine.addEventListener("onConnectionTypeChanged", (newConnectionType) => {
					const message = `onConnectionTypeChanged通知：newConnectionType=${newConnectionType}`
					this.engine.nertcPrint(message)
					console.log(message)
				});
				this.engine.addEventListener("onConnectionStateChanged", (state, reason) => {
					const message = `onConnectionStateChanged通知：state=${state}, reason = ${reason}`
					this.engine.nertcPrint(message)
					console.log(message)
				});
				this.engine.addEventListener("onLeaveChannel", (result) => {
					const message = `onLeaveChannel通知：自己离开房间状况，result = ${result}`
					this.engine.nertcPrint(message)
					console.log(message)
				});
				
				this.engine.addEventListener("onUserJoined", (userID, customInfo, userStringID) => {
                    //userStringID为userID的String格式，如果userID的实际值超过了Number范围，请使用userStringID进行相关逻辑业务
					const message = `onUserJoined通知：有人加入房间，userID = ${userID}, userStringID = ${userStringID}, customInfo = ${customInfo}`
					this.engine.nertcPrint(message)
					console.log(message)
				});
				
				this.engine.addEventListener("onUserLeave", (userID, reason, userStringID) => {
                    //userStringID为userID的String格式，如果userID的实际值超过了Number范围，请使用userStringID进行相关逻辑业务
					const message = `onUserLeaved通知：有人离开房间，userID = ${userID}, userStringID = ${userStringID}, reason = ${reason}`
					this.engine.nertcPrint(message)
					console.log(message)
					//建议在当前页面上销毁 nertc-remote-view组件
					const remoteUserID = userStringID
					let list = this.remoteUserIdVideoList.filter(val => val !== remoteUserID);
					this.remoteUserIdVideoList = list;
					list = this.remoteUserIDSubStreamVideoList.filter(val => val.viewID !== remoteUserID);
					this.remoteUserIDSubStreamVideoList = list;
				});
				
				this.engine.addEventListener("onUserAudioStart", ( userID, userStringID ) => {
					//SDK自动订阅对端音频，这里仅仅是通知信息，开发者不需要做什么逻辑
                    //userStringID为userID的String格式，如果userID的实际值超过了Number范围，请使用userStringID进行相关逻辑业务

					const message = `onUserAudioStart通知：对方开启音频，userID = ${userID}, userStringID = ${userStringID}`
					this.engine.nertcPrint(message)
					console.log(message)
				});
				
				this.engine.addEventListener("onUserAudioStop", (userID, userStringID) => {
                    //userStringID为userID的String格式，如果userID的实际值超过了Number范围，请使用userStringID进行相关逻辑业务

					const message = `onUserAudioStop通知：对方关闭音频，userID = ${userID}, userStringID = ${userStringID}`
					this.engine.nertcPrint(message)
					console.log(message)
				});
				
				this.engine.addEventListener("onUserVideoStart", (userID, maxProfile, userStringID) => {
                    //userStringID为userID的String格式，如果userID的实际值超过了Number范围，请使用userStringID进行相关逻辑业务
					const message = `onUserVideoStart通知：对方开启视频，userID = ${userID}, userStringID = ${userStringID}, maxProfile = ${maxProfile}`
					this.engine.nertcPrint(message)
					console.log(message)
					//nertc-remote-view组件 viewID和userID的数据格式是String类型，onUserVideoStart事件通知的userID是number，这里做一下数据格式转换
					const remoteUserID = userStringID // 这里可以直接使用userStringID
					if (!this.remoteUserIdVideoList.includes(remoteUserID)) {
						this.remoteUserIdVideoList.push(remoteUserID);
						//保证当前nertc-remote-view组件渲染完成，在执行设置画布的接口
						this.$nextTick(() => {
							console.warn('此时远端视频 nertc-remote-view 渲染完成')
							this.engine.nertcPrint('此时远端视频 nertc-remote-view 渲染完成')
							//需要开发者主动去做订阅对方视频的逻辑动作
							this.subscribeRemoteVideo(userID, userStringID) 
						})
					}
				});
								
				this.engine.addEventListener("onUserVideoStop", (userID, userStringID) => {
                    //userStringID为userID的String格式，如果userID的实际值超过了Number范围，请使用userStringID进行相关逻辑业务

					const message = `onUserVideoStop通知：对方关闭视频，userID = ${userID}, userStringID = ${userStringID}`
					this.engine.nertcPrint(message)
					console.log(message)
					//建议在当前页面上销毁对应的nertc-remote-view组件
					const remoteUserID = userStringID
					//对端关闭视频，建议主动调用接口释放对端视频画布相关的资源
					this.engine.destroyRemoteVideoCanvas({
						userID,
						userStringID
					})
					const list = this.remoteUserIdVideoList.filter(val => val !== remoteUserID);
					this.remoteUserIdVideoList = list;
				});
				
				this.engine.addEventListener("onUserSubStreamVideoStart", (userID, maxProfile, userStringID) => {
                    //userStringID为userID的String格式，如果userID的实际值超过了Number范围，请使用userStringID进行相关逻辑业务

					const message = `onUserSubStreamVideoStart通知：对方开启screen，userID = ${userID}, userStringID = ${userStringID}`
					this.engine.nertcPrint(message)
					console.log(message)
					//nertc-remote-view组件 viewID和userID的数据格式是String类型，onUserSubStreamVideoStart事件通知的userID是number，这里做一下数据格式转换
					const remoteUserID = userStringID 
					if (!this.remoteUserIDSubStreamVideoList.includes(remoteUserID)) {
						this.remoteUserIDSubStreamVideoList.push({viewID: remoteUserID, key : remoteUserID + 'screen'});
						//保证当前nertc-remote-view组件渲染完成，在执行设置画布的接口
						this.$nextTick(() => {
							console.warn('此时对端屏幕共享 nertc-remote-view 渲染完成')
							this.engine.nertcPrint('此时对端屏幕共享 nertc-remote-view 渲染完成')
							//需要开发者主动去做订阅对方屏幕共享的逻辑动作
							this.subscribeRemoteSubStreamVideo(userID, userStringID) 
						})
					}
				});
				
				this.engine.addEventListener("onUserSubStreamVideoStop", (userID, userStringID) => {
                    //userStringID为userID的String格式，如果userID的实际值超过了Number范围，请使用userStringID进行相关逻辑业务
					const message = `onUserSubStreamVideoStop通知：对方关闭屏幕共享，userID = ${userID}, userStringID = ${userStringID}`
					this.engine.nertcPrint(message)
					console.log(message)
					
					//建议在当前页面上销毁对应的nertc-remote-view组件
					const remoteUserID = userStringID //`${userID}`
					const list = this.remoteUserIDSubStreamVideoList.filter(item => item.viewID !== remoteUserID);
					this.remoteUserIDSubStreamVideoList = list;
					//对端关闭视频，建议主动调用接口释放对端视频画布相关的资源
					this.engine.destroyRemoteSubStreamVideoCanvas({
						userID: remoteUserID,
						userStringID
					})
				});
				
				this.engine.addEventListener("onUserAudioMute", (userID, eanble, userStringID) => {
                    //userStringID为userID的String格式，如果userID的实际值超过了Number范围，请使用userStringID进行相关逻辑业务
					const message = `onUserAudioMute通知：对方mute音频，userID = ${userID}, userStringID = ${userStringID}, eanble = ${eanble}`
					this.engine.nertcPrint(message)
					console.log(message)
				});
				
				this.engine.addEventListener("onUserVideoMute", (userID, eanble, userStringID) => {
                    //userStringID为userID的String格式，如果userID的实际值超过了Number范围，请使用userStringID进行相关逻辑业务
					const message = `onUserVideoMute通知：对方mute视频，userID = ${userID}, userStringID = ${userStringID}, eanble = ${eanble}`
					this.engine.nertcPrint(message)
					console.log(message)
				});
				
				this.engine.addEventListener("onAudioDeviceChanged", (selected) => {
					const message = `onAudioDeviceChanged通知：对方mute视频，selected = ${selected}`
					this.engine.nertcPrint(message)
					console.log(message)
				});
				
				this.engine.addEventListener("onAudioDeviceStateChange", (deviceType, deviceState) => {
					const message = `onAudioDeviceStateChange通知：对方mute视频，deviceType = ${deviceType}, deviceState = ${deviceState}`
					this.engine.nertcPrint(message)
					console.log(message)
				});
				this.engine.addEventListener("onFirstAudioDataReceived", (userID, userStringID) => {
                    //userStringID为userID的String格式，如果userID的实际值超过了Number范围，请使用userStringID进行相关逻辑业务
					const message = `onFirstAudioDataReceived通知：userID = ${userID}, userStringID = ${userStringID}`
					this.engine.nertcPrint(message)
					console.log(message)
				})
				this.engine.addEventListener("onFirstVideoDataReceived", (userID, videoStreamType, userStringID) => {
                    //userStringID为userID的String格式，如果userID的实际值超过了Number范围，请使用userStringID进行相关逻辑业务
					const message = `onFirstVideoDataReceived通知：userID = ${userID}, userStringID = ${userStringID}, videoStreamType = ${videoStreamType}`
					this.engine.nertcPrint(message)
					console.log(message)
				});
				this.engine.addEventListener("onFirstAudioFrameDecoded", (userID, userStringID) => {
                    //userStringID为userID的String格式，如果userID的实际值超过了Number范围，请使用userStringID进行相关逻辑业务
					const message = `onFirstAudioFrameDecoded通知：userID = ${userID}, userStringID = ${userStringID}`
					this.engine.nertcPrint(message)
					console.log(message)
				});
				this.engine.addEventListener("onFirstVideoFrameDecoded", (userID, videoStreamType, userStringID) => {
                    //userStringID为userID的String格式，如果userID的实际值超过了Number范围，请使用userStringID进行相关逻辑业务
					const message = `onFirstVideoFrameDecoded通知：userID = ${userID}, userStringID = ${userStringID}, videoStreamType = ${videoStreamType}`
					this.engine.nertcPrint(message)
					console.log(message)
				});
				this.engine.addEventListener("onLocalAudioVolumeIndication", (volume, vadFlag) => {
					const message = `onLocalAudioVolumeIndication通知：volume = ${volume}, vadFlag = ${vadFlag}`
					this.engine.nertcPrint(message)
					console.log(message)
				});
				this.engine.addEventListener("onRemoteAudioVolumeIndication", (volumeList,totalVolume) => {
					const message = `onRemoteAudioVolumeIndication通知：总音量totalVolume = ${totalVolume}`
					this.engine.nertcPrint(message)
					console.log(message)
					volumeList.forEach((user)=>{
                        //userStringID为userID的String格式，如果userID的实际值超过了Number范围，请使用userStringID进行相关逻辑业务
					    console.log(`用户 userID=${user.userID}, userStringID=${user.userStringID} 的音频音量为: ${user.volume},音频辅流的音量为: ${user.subStreamVolume} `)
						this.engine.nertcPrint(`用户 userID=${user.userID}, userStringID=${user.userStringID} 的音频音量为: ${user.volume},音频辅流的音量为: ${user.subStreamVolume} `)
					})
				});
				this.engine.addEventListener("onNetworkQuality", (statsArray) => {
					statsArray.forEach((user)=>{
                        //userStringID为userID的String格式，如果userID的实际值超过了Number范围，请使用userStringID进行相关逻辑业务
						console.log(`用户 userID=${user.userID}, userStringID=${user.userStringID} 的上行网络质量为: ${user.upStatus}, 下行网络质量为: ${user.downStatus} `)
						this.engine.nertcPrint(`用户 userID=${user.userID}, userStringID=${user.userStringID} 的上行网络质量为: ${user.upStatus}, 下行网络质量为: ${user.downStatus} `)
					})
				});
				this.engine.addEventListener("onRtcStats", (rtcStats) => {
					const message = `onRtcStats通知: rtcStats= ${JSON.stringify(rtcStats)} `
					this.engine.nertcPrint(message)
				});
				this.engine.addEventListener("onLocalAudioStats", (stats)=> {
					stats.audioLayers.forEach((item)=>{
						const message = `onLocalAudioStats 本地音频流统计信息: ${JSON.stringify(item)}`
						this.engine.nertcPrint(message)
						console.log(message)
					})
				});
				this.engine.addEventListener("onRemoteAudioStats", (stats)=> {
					this.engine.nertcPrint('onRemoteAudioStats通知 长度length ' + stats.length)
					stats.forEach((item)=>{
                        //userStringID为userID的String格式，如果userID的实际值超过了Number范围，请使用userStringID进行相关逻辑业务
						const message = `onRemoteAudioStats 用户 userID=${item.userID}, userStringID=${item.userStringID} 的音频统计`
						this.engine.nertcPrint(message)
						console.log(message)
						item.audioLayers.forEach((info)=>{
							const imessage = `onRemoteAudioStats 用户 userID=${item.userID}, userStringID=${item.userStringID} 的音频统计信息: ${JSON.stringify(info)} `
							this.engine.nertcPrint(imessage)
							console.log(imessage)
						})
					})
				});
				this.engine.addEventListener("onLocalVideoStats", (stats)=> {
					stats.videoLayers.forEach((item)=>{
						const message = `onLocalVideoStats 本地视频流统计信息: ${JSON.stringify(item)}`
						this.engine.nertcPrint(message)
						console.log(message)
					})
				});
				this.engine.addEventListener("onRemoteVideoStats", (stats)=> {
					this.engine.nertcPrint('onRemoteVideoStats通知 长度length ' + stats.length)
					stats.forEach((item)=>{
                        //userStringID为userID的String格式，如果userID的实际值超过了Number范围，请使用userStringID进行相关逻辑业务
						const message = `onRemoteVideoStats 用户 userID=${item.userID}, userStringID=${item.userStringID} 的视频统计`
						this.engine.nertcPrint(message)
						console.log(message)
						item.videoLayers.forEach((info)=>{
							const imessage = `onRemoteVideoStats 用户 userID=${item.userID}, userStringID=${item.userStringID} 的视频统计信息: ${JSON.stringify(info)} `
							this.engine.nertcPrint(imessage)
							console.log(imessage)
						})
					})
				});
			},

        destroyEngine() {
            console.log('销毁NERTC引擎')
            //清除注册的所有事件
            this.engine?.removeAllEventListener()
            //释放资源
            this.engine?.destroyEngine()
            this.engine = null
            this.remoteUserIdVideoList = []
        },

        joinChannel(){
            console.log('加入房间')
            this.engine.joinChannel({
                token: '',
                channelName: 'uniapp', //自定义房间名称
                myUid: 111 //该房间中个人userID标识，要求number类型（不要超出Number范围），房间中唯一
                myStringUid: '1111111111111111111' //SDK优先使用Sting格式的uid，如果超出了Number范围，使用myStringUid作为真实的uid，myUid值sdk会忽略
            });
        },

        leaveChannel(){
            console.log('离开房间')
            this.engine.leaveChannel();
            this.remoteUserIdVideoList = []
        },

        subscribeRemoteVideo(remoteUserID, remoteUserStringID) {
            let messge = `开始拉流: remoteUserID=${remoteUserID}, remoteUserStringID=${remoteUserStringID}, 设置对端视频画布`
            console.log(messge)
            this.engine.nertcPrint(messge)

            //设置远端视频的画布
            this.engine.setupRemoteVideoCanvas({
                renderMode: NERTCRenderMode.Fit, // Fit表示应区域。视频尺寸等比缩放,保证所有区域被填满,视频超出部分会被裁剪
                mirrorMode: NERTCMirrorMode.AUTO, //AUTO表示使用默认值,由SDK控制
                isMediaOverlay: false, //表示小画布置于大画布上面
                userID: remoteUserID,
                userStringID: remoteUserStringID //SDK优先采用userStringID格式的数字，如果远端的userID超出了Number范围，请填写userStringID作为真实的userID
            })

            messge = `subscribeRemoteVideo: ${remoteUserStringID}, 订阅对端视频`
            console.log(messge)
            this.engine.nertcPrint(messge)
            this.engine.subscribeRemoteVideo({
                userID: remoteUserID,
                userStringID: remoteUserStringID, //同上
                streamType: 0, //0表示大流，1表示小流
                subscribe: true //true表示订阅，false表示取消订阅
            })
        }
    }
}
```
