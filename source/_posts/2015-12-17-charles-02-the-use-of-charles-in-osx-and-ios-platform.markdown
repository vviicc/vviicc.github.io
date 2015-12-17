---
layout: post
title: "Charles-01-the use of Charles in OSX and iOS platform"
date: 2015-12-17 14:32:43 +0800
comments: true
categories: iOS开发
---
### OSX/iOS 环境下的charles使用篇

1. **查看网络包**

	 Charles提供了两种形式查看网络包：*Structure*和*Sequence*视图。
	
	*Structure*视图是根据*host name*以树状组织结构展示，便于查看同一主机名的网络包，每次有新的请求，相应主机名目录会变黄。
		
	*Sequence*视图是以网络请求的时间顺序展示的。
	
2. **过滤网络包**

	* 在*Proxy->Recording Settings*有*Include*和*Exclude*设置，*Include*中如果添加了请求，那仅限于这里添加的请求会被记录。如果*Include*列表为空，那就仅仅过滤掉*Exclude*列表的请求，每个请求可以指定协议，主机地址，端口，路径，参数等信息，支持通配符。
	
		![过滤网络包](http://7rfl57.com1.z0.glb.clouddn.com/QQ20151216-4.png?attname=&e=1450344744&token=5JO6EV56uqnbS1xtKLuQY8XNPSgzftW_t-GExHM9:0j-Av_IOGQAgPoH9LxYq8iKaRN8 =500x "过滤网络包")
		
		*以下所有请求接口来自于 [*openweathermap*](http://openweathermap.org/ "openweathermap") 提供的开放API 
		
	* 也可对记录结果进行过滤，1.Commend + F 查找页面，可以对URL,头部，内容进行过滤，支持通配符。 2.在*Sequence*视图中在*Filter*输入关键字过滤。
	
	* 选中某个请求包选择*Fouce*，这个请求包将与其他请求分开单独显示。
	
3. **修改网络包**

	修改网络包有两种形式，一种是静态对之前的网络包请求修改再执行，另一种是动态修改正在请求的网络包。
	
	* 静态修改（*Edit*）
	
		对要修改的请求记录选择 *Edit* ,修改请求，然后修改请求数据，点击执行。
		
		![Edit](http://7rfl57.com1.z0.glb.clouddn.com/QQ20151216-5.png?attname=&e=1450344744&token=5JO6EV56uqnbS1xtKLuQY8XNPSgzftW_t-GExHM9:mWo6Hy2-1pQ0LSpik_SKBCL90gc =500x "Edit")
		
	* 动态修改（*Breakpoints*, *Rewrite*, *Map Loacl*, *Map Remote*）
	
		* Breakpoints
		
			添加了Breakpoints的请求发生时，会截取改请求，可以对请求和返回数据进行修改，适合临时性修改请求和返回数据。添加方法是在选中某个请求点击*Breakpoints*，选择*Proxy->Enable Breakpoints*打开Breakpoints功能，也可以在*Proxy->Breakpoints Settings*设置。
			
			![Breakpoints](http://7rfl57.com1.z0.glb.clouddn.com/QQ20151216-8.png?attname=&e=1450358893&token=5JO6EV56uqnbS1xtKLuQY8XNPSgzftW_t-GExHM9:_u_5Jf_FZhFFAdb6VxPwHxekXk4 "Breakpoints")
				
				Breakpoints截取网络过程中网络请求并没有暂停，长时间的等待可能导致请求超时。
			
		* Rewrite
		
			Rewrite是将请求数据或返回结果替换，点击*Tools->Rewrite*，选中*Enable Rewrite*，添加*Sets*，根据需要添加*Locations*信息，添加了的话*Rewrite*将只针对*Locations*列表请求有效，再添加*Rules*规则，可以对请求和返回数据替换，支持通配符。有一个*Degbug in Error Log*的选项，作用是如果有匹配到的话会在Log记录里显示出来，Log记录是在*Window->Error Log*中。
			
			![Rewrite](http://7rfl57.com1.z0.glb.clouddn.com/QQ20151217-1.png?attname=&e=1450404100&token=5JO6EV56uqnbS1xtKLuQY8XNPSgzftW_t-GExHM9:qef3o6IMyhpTslqCcKJTAfIPZiQ =500x "Rewrite")
			
		* Map Local
		
			Map Local 是将本地文件作为请求网络的返回数据，这样就可以在部署到远程网络前进行测试。
			
		* Map Remote
		
			Map Remote 是将一个请求从某个服务器转成另一个服务器去处理。
			
4. **重复请求**

	* Repeat(重复一次)
		
		选中一个请求，选择Repeat，将重复执行一次。
			
	* Repeat Advanced（负载测试）
	
		Repeat Advanced可以设置并发数以及重复次数，对于负载测试很有帮助。
		
		![Repeat Advanced](http://7rfl57.com1.z0.glb.clouddn.com/QQ20151216-6.png?attname=&e=1450357558&token=5JO6EV56uqnbS1xtKLuQY8XNPSgzftW_t-GExHM9:5nnRmi_6sk9ZWjignqtuedzFEdA "Repeat Advanced") 
		
		![Repeat Advanced Result](http://7rfl57.com1.z0.glb.clouddn.com/QQ20151216-7.png?attname=&e=1450357558&token=5JO6EV56uqnbS1xtKLuQY8XNPSgzftW_t-GExHM9:zHfMoMg_8VMWPnqpjBh41Ilu_y0 =750x "Repeat Advanced Result")
	
5. **模拟网速**

	Charles还支持模拟不同网速功能，在*Proxy->Throttle Settings*选中*Enable Throttling*，根据需要添加*Locations*，如果选中了*Only for selected hosts*，并且*Locations*中有数据，则只有*Locations*列表中的请求会被限速。*Throttle Configuration*中对网速进行配置。
	
	![Throttle Configuration](http://7rfl57.com1.z0.glb.clouddn.com/QQ20151217-2.png?attname=&e=1450405497&token=5JO6EV56uqnbS1xtKLuQY8XNPSgzftW_t-GExHM9:VYWsm7pY1J9QTh0FTnsbpT-OcSQ "Throttle Configuration")
	
	> 如果仅仅是需要模拟网速，[Network Link Conditioner](https://developer.apple.com/downloads/index.action?q=Hardware%20IO%20Tools) 也是个不错的选择。--> [相关教程](http://nshipster.com/network-link-conditioner/)
	
6. **自动保存**

	自动保存 (*Auto Save*) 能在设定的时间间隔里自动保存Session记录，对监控长时间网络请求很有帮助。到了设定的时间间隔Charles会保存之前的记录在设定的本地目录文件，文件会以请求时间命名，保存后会把之前Charles在内存里的记录清除掉，这样长时间的请求也不会超出内存。设置方法是*Tools->Auto Save*，选择*Enable Auto Save*，设定时间间隔、保存路径和保存格式。
	
	![Auto Save](http://7rfl57.com1.z0.glb.clouddn.com/QQ20151217-4.png?attname=&e=1450406615&token=5JO6EV56uqnbS1xtKLuQY8XNPSgzftW_t-GExHM9:Dd9BdxEgogNfGgEMxmb7RBSTML0 "Auto Save")
	
	![Auto Save](http://7rfl57.com1.z0.glb.clouddn.com/QQ20151217-5.png?attname=&e=1450406990&token=5JO6EV56uqnbS1xtKLuQY8XNPSgzftW_t-GExHM9:Dbx69cJARmZs7b_iMV5ND8LQ-_o "Auto Save")
	
7. **反向代理**

	反向代理（*Reverse Proxy*）可以将本地可用端口替换成远程web服务器端口（一般为80端口）
	
8. **DNS欺骗**

	DNS欺骗（*DNS Spoofing*）可以将域名直接替换成想要的IP地址，正常的DNS解析更换需要较长时间才生效，而使用DNS欺骗可以立即更换IP地址，不用等待DNS解析生效就可以进行测试。

9. **HTTPS/SSL代理**
	
	* 原理：*Charles*能够支持HTTPS代理的原理是*Charles*充当了中间人角色，Charles会为web服务器动态生成一个使用自己根证书（*Charles CA Certificate*）签名的证书，*Charles*会接收web服务器的证书，而客户端浏览器就接收Charles生成的证书。因此在在使用*Charles*作为HTTPS代理时客户端在请求HTTPS接口时会弹出安全警告，提示*Charles*根证书不被信任，所以需要添加*Charles*根证书为信任证书中，就不会出现类似的警告了。
	
	* 使用：开启SSL代理功能，在*Proxy->SSL Proxying Setting*选中*Enable SSL Proxying*，在*Locations*添加要使用SSL代理的网站，如果需要匹配所有的HTTPS网站则输入 * 号即可，另外还可以上传客户端证书和根证书。
	
	* SSL证书：为了消除浏览器的安全警告，需要添加*Charles*根证书到信任的证书列表中。
	
		OSX环境：选择Charles的*Help->SSL Proxying->Install Charles Root Certificate* 菜单，*Keychain Access* 会打开，点击*永远信任*按钮，输入管理员密码就可以将*Charles*证书添加到信任列表中，需要关闭后重新打开Safari才能生效。
		
		iOS设备：首先将iOS设备网络代理设好（参考Charles配置篇），然后用Safari打开 [*http://www.charlesproxy.com/getssl*]( http://www.charlesproxy.com/getssl) 链接，按照提示下载安装SSL证书。
		
		> iOS9以上系统要使用Charles作为SSL代理的话要关闭 *[APP Transport Security](https://developer.apple.com/library/prerelease/mac/technotes/App-Transport-Security-Technote/index.html)* ，关闭方法为在APP的*info.plist*文件添加以下key。APP发布前记得开启 *APP Transport Security* 以便获取安全网络通信支持。
		
			<key>NSAppTransportSecurity</key>
			<dict>
  				<key>NSAllowsArbitraryLoads</key>
  				<true/>
			</dict>

10. **Tips**

	* 可以同时运行多个Charles，在Charles安装目录下用终端执行```open charles.app -n```，为每个Charles设置不同的监听端口，比如一个监听iOS设备的端口，一个监听OSX的端口。

	








		
