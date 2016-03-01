---
layout: post
title: "Objective-C环境的protobuf3.0.0使用"
date: 2016-03-01 20:11:52 +0800
comments: true
categories: 
---
### 说明
------
*	[**protobuf**](https://github.com/google/protobuf) 3.0.0（包括）以上才官方支持Objective-C，低于3.0.0的请忽略或使用第三方转换工具
*	Objective C 2.0运行时环境 (32bit & 64bit iOS, 64bit OS X)
*	XCode 7.0及以上
*	基于性能原因没有使用ARC，但可以被ARC代码调用

### 步骤
------
主要包括两个步骤：

1. **转换**：将我们的XXX.proto文件转成Objective C文件，也就是XXX.h和XXX.m文件，转换的工具是使用**protoc**这种二进制文件来生成的，稍后会介绍如何生成**protoc**这个文件(这个文件需要自己生成)以及如果使用它来转换成Objective C文件。
2. **集成**：在项目中加入protobuf库以及步骤1生成的Objective C文件。

### 详细步骤
##### 步骤1：转换
------
**生成protoc**

1.	如果没有装autoconf automake libtool需要先装这几个，这里使用brew来安装，在shell执行 `brew install autoconf automake libtool`即可，如果没有brew请自行先安装brew。
2.	下载面向Objective-C的protobuf库，地址为[https://github.com/google/protobuf/releases](https://github.com/google/protobuf/releases),要下载对应Objective-C的版本比如 protobuf-objectivec-3.0.0-beta-2.zip，解压。
3.	cd到下载的目录，执行./autogen.sh
4.	依次执行 
		
		$ ./configure
		$ make
		$ make check
		$ sudo make install
	
5.	再执行`objectivec/DevTools/full_mac_build.sh`，执行完后会看到src目录下生成了protoc二进制文件

**使用protoc转换**

1.	创建proto文件，比如Person.proto

			syntax = "proto2";
			
			message Person
			{	
				required int32 id = 1;
				required string username = 2;
				optional string phone = 3;
			}
	需要注意的是要指明proto的语法规则是proto2还是proto3。
	
2.	在src目录(protoc所在目录)执行

		protoc --proto_path=... --objc_out=... XXX.proto
其中proto_path是我们创建的proto文件所在目录，objc_out为Objective-C文件输入路径,XXX.proto是我们创建的proto文件，可以一次转换多个proto文件，加在XXX.proto后面即可。

	举例：我们在src目录下新建两个文件夹，gen和protocols文件夹，gen为输出目录，protocols用于存放proto文件，将创建的Person.proto放在protocols文件夹下，执行命令
	
		protoc --proto_path=protocols --objc_out=gen protocols/Person.proto
	然后在gen文件夹下就会生成Person.pbobjc.h和Person.pbobjc.m文件。
	
#####	步骤2：集成
------
1.	将生成的Ojective-C文件（上面例子的Person.pbobjc.h和Person.pbobjc.m）放到项目中，如果项目使用了ARC,要将.m（例子的Person.pbobjc.m）的Complier Flags设为`-fno-objc-arc`。（protobuf基于性能原因没有使用ARC）
2.	加入protobuf库，有两种方式，第一种是使用CocoaPods集成，第二种是把相关文件拖入项目中。

	*	使用CocoaPods集成，有一个现成的pod可以使用--Protobuf,可以`pod search Protobuf`搜索查看详情，pod内容为
			
			platform :ios, '7.1'
			pod 'Protobuf', '~> 3.0.0-beta-2'
需要注意的是 `platform :ios, '7.1'` 7.1及以上才能导入这个库,这种方式优点是操作简单，缺点是`platform :ios, '7.1'` 要7.1或以上

	*	拖入相关文件到项目中，将objectivec文件夹下的所有的.h文件和.m文件（除了GPBProtocolBuffers.m）(GPB开头的那些文件)以及整个google文件夹add到项目中，如果项目中使用了ARC需要将以上所有.m文件的的Complier Flags设为`-fno-objc-arc`。这种方法的优点是灵活性强，没有7.1的束缚。缺点是操作麻烦点，如果用了ARC的话还要手动添加`-fno-objc-arc`（使用CocoaPods集成会自动添加）
	
###	简单使用
直接上代码
	
		- (void)viewDidLoad {
    	[super viewDidLoad];
    	Person *person = [[Person alloc] init];
   	 	person.id_p = 100;  // id可能是和库重复了，自动转换成了id_p
    	person.username = @"Li";
    	person.phone = @"10086";
    	NSData *data = [person data];
    	Person *p = [Person parseFromData:data error:nil];
    	NSLog(@"person:%@",p);
	}
	