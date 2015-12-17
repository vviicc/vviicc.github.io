---
layout: post
title: "Charles-01-the configuration of Charles in OSX and iOS platform"
date: 2015-12-17 14:24:06 +0800
comments: true
categories: iOS开发
---
### OSX/iOS 环境下的Charles配置篇
1.	**Mac OSX 代理设置**

	首次安装会弹出是否允许Charles自动设置代理，选择允许就会自动设置，也可以到*Proxy->Proxy Settings*设置，设置后其实就是自动设置了网络代理。
	
	![网络代理设置](http://7rfl57.com1.z0.glb.clouddn.com/QQ20151216-1.png?attname=&e=1450332386&token=5JO6EV56uqnbS1xtKLuQY8XNPSgzftW_t-GExHM9:XYXP1J9LtsN7jNL68dK_dqUnxeA =500x "网络代理设置")
	
2.	**iOS模拟器**

	iOS模拟器也是使用Mac OSX的系统代理设置
	> *XCode6*以后要先运行Charles并设置好Mac OSX系统代理再运行iOS模拟器。

3.	**iOS设备**

	选择和Mac同一个WIFI网络，选择HTTP代理为*手动*。
	
	![HTTP代理](http://7rfl57.com1.z0.glb.clouddn.com/FullSizeRender.jpg?attname=&e=1450344950&token=5JO6EV56uqnbS1xtKLuQY8XNPSgzftW_t-GExHM9:gzIUZ2ybFOQE-KH_Vh2whrsrSuU =500x 'HTTP代理')
	
	* 服务器：设为Mac的IP地址，可以通过Charles的*Help->Local IP Address*查看， 或者通过Mac的*设置->网络->TCP/IP*查看IP地址。
	
			PS:后者会更准确些，前者遇到过错误的情况。
		
	* 端口号：设置为*Proxy Setting*中相同的端口号即可（通常为8888）。
	
	![端口号](http://7rfl57.com1.z0.glb.clouddn.com/QQ20151216-2.png?attname=&e=1450335575&token=5JO6EV56uqnbS1xtKLuQY8XNPSgzftW_t-GExHM9:2rQsCMoBH6HtI1lThQuQRA4_5FM =500x)
	
	* 鉴定：选择不鉴定
	
	首次连接时Charles会弹出是否信任这个连接，选择信任，这个IP地址就会被添加到*Access Control list*中，你也可以到*Proxy->Access Control Setting*中修改。
	
		PS：如果没有弹出那个提示框，一般设置不正确，重现检查设置或更换网络（有的网络比如需要用户密码登录的可能不行）
		PS：在你不用Charles的时候要记得关闭HTTP代理，否则会上不了网。
	
