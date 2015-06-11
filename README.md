# zookeeper_demo
我的zookeeper示例

zookeeper  step by step
下载：
	网址：http://zookeeper.apache.org/releases.html
	下载时找稳定版本下载,即 stable/。我演示使用的是zookeeper-3.4.6.tar.gz
参考文档：
	http://zookeeper.apache.org/doc/r3.4.6/zookeeperStarted.html
	http://zookeeper.apache.org/doc/r3.4.6/zookeeperAdmin.html
安装：
	将下载的压缩包解压，提取zookeeper-3.4.6.jar放到项目中
系统要求：
	1. GNU/Linux is supported as a development and production platform for both server and client.
	2. Sun Solaris is supported as a development and production platform for both server and client.
	3. FreeBSD is supported as a development and production platform for clients only. Java NIO selector support in the FreeBSD JVM is broken.
	4. Win32 is supported as a development platform only for both server and client.
	5. MacOSX is supported as a development platform only for both server and client.
 软件要求：
 	1：java 1.6及以上
 	2：最少创建3个ZooKeeper服务器
 	3：可选，最好运行在3台独立物理服务器上
 	4：参考，在Yahoo，ZooKeeper被部署在专用的服务器上，双核处理器，2G内存，80G物理硬盘。
 	5：最好部署奇数台机器，5台机器可以容灾2台机器出问题。
 执行步骤：
 	以下步骤在每台服务器上都要执行。
 	1： 安装jdk 1.6+;
 	2: 设置堆内存的大小。保守建议：4G内存的机器最大的内存使用为3G。
 			参考网址：http://zhli986-yahoo-cn.iteye.com/blog/1149233。
 		
 			对于单独的.class，可以用下面的方法对Test运行时的jvm内存进行设置。
			java -Xms64m -Xmx256m Test
			-Xms是设置内存初始化的大小
			-Xmx是设置最大能够使用内存的大小
	3:程序中部署ZooKeeper安装包。此次我们例子就使用zookeeper-3.4.6.jar
	4：创建一个可以被所有允许调用的程序调用的配置文件。文件内容开始部分如下：
		tickTime=2000
		dataDir=/var/lib/zookeeper/
		clientPort=2181
		initLimit=5
		syncLimit=2
		server.1=zoo1:2888:3888
		server.2=zoo2:2888:3888
		server.3=zoo3:2888:3888
	每个参数的意思在下面网址可见。http://zookeeper.apache.org/doc/r3.4.6/zookeeperAdmin.html#sc_configuration。
	这里简单讲解一下,ZooKeeper集成中每台服务器都需要知道其他所有服务器的存在。使用server.id=host:port:port.来实现这个目标。
	通过配置文件里dataDir指定的目录里创建一个名称为 myid的文件，通过该文件实现把本机的id通知其他服务器。
	5： myid文件内只允许有该机器的id，没有其他文字。例如id为1的服务器，myid文件内只有文字1.注意集成环境中id是唯一的，允许值为1-255.
	6: 启动zookpeer.
		java -cp zookeeper.jar:lib/slf4j-api-1.6.1.jar:lib/slf4j-log4j12-1.6.1.jar:lib/log4j-1.2.15.jar:conf \ org.apache.zookeeper.server.quorum.QuorumPeerMain zoo.cfg
	说明：
		6.1 zoo.cfg是4中创建的配置文件
		6.2 QuorumPeerMain 类启动zookpeer服务器
		6.3 可以使用jmx进行管理。详细内容可以参考下列网址：http://zookeeper.apache.org/doc/r3.4.6/zookeeperJMX.html
		6.4 <zookpeer_home>/bin/zkServer.sh是启动应用的例子。
	7：通过链接主机的方式验证我们的部署结果。
		有C和java两种语言版本，我们这里以java为例子。执行下面的语句可以执行简单的操作。
		java -cp zookeeper.jar:lib/slf4j-api-1.6.1.jar:lib/slf4j-log4j12-1.6.1.jar:lib/log4j-1.2.15.jar:conf:src/java/lib/jline-0.9.94.jar \ org.apache.zookeeper.ZooKeeperMain -server 127.0.0.1:2181
	8 结束
	  后记：如果开发阶段想进行单机测试，官方也有文档说明，网址为：http://zookeeper.apache.org/doc/r3.4.6/zookeeperStarted.html#sc_InstallingSingleMode
		
 	

