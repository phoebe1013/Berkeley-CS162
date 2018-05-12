
* UNIX System Structure

![UNIX](images/01-019.png "UNIX System Structure")

* Kernel Mode is some sort of **monolithic(整体的) structure**, which means it's not protected from itself. If some part of the kernel over here has a bug, it can screw up another part of the kernel.
（ I/O system, file system, CPU scheduling ）


----------

* Moore's Law Change Drives OS Change

![Change](images/01-020.png "Change")

		CPU: MHz (megahertz 兆赫)
		Caoacity: KB (kilobytes 千字节), MB (megabytes 兆字节)
		Net bandwidth(净带宽、实际带宽): bit per second
		Number(#) of addr bits: bits
		#users/machine: how many user per machine
		
			
* Moore's Law Effects
	+ Nothing like this in any othre area of business
	
	+ Transportation in over 200 years:
		- 2 orders of magnitude from horseback @10 mphto concorde(协和式飞机) @1000 mph
		- Computers do this every decade (at least until 2002)
	
	+ Techniques have to vary over time to adapt to changing tradeoffs(折衷、权衡)



	
-----------

#### History

![Dawn](images/01-021.png "Dawn黎明")

##### Phase 1 (1948-1970): Hardware Expensive, Humans Cheap 

* When computers cost millions of $'s, optimize for more efficient use of the hardware!
	+ Lack of interaction between user and computer 

* User at console(控制台用户): one user at a time
* Batch monitor(批处理监视器): load program, run, print

* Optimize to better use hardware
	- When user thinking at console, computer idle(空闲的) -> BAD (A big waste)!
	- Feed computer batches and make users wait
	- Autograder for this course is similar
	
* __No protection__: what if bathc program has bug?
	- It will bring the whole machine down.
	
----------

* Core Memories(1950s & 60s)


![Dawn](images/01-022.png "Dawn黎明")

	Magnetic core memory (磁芯存储器、磁芯记忆体)：利用磁性材料製成之記憶體，
	其原理為：將磁環（磁芯）帶磁性或不帶磁性之狀態， 用以代表1或0之位元，一長串1
	或0之組合就代表要儲存之資訊。 磁芯記憶體是一種随机存取记忆体（Random Access
	 Memory），在電腦中可擔任主記憶體的角色。
	
	magnetization (磁化）
	iron rings (铁环）
	mesh of wires (钢丝网)
	
----------
	
	
##### Phase 1.5 (late 60s/early 70s):

* __Data channels, Interrupts__(数据通道，中断): overlap I/O and compute
  
	+ DMA - Direct Memory Access for I/O devices
	
			DMA (Direct Memory Access 直接内存存取) 是所有现代电脑的重要特
			色，它允许不同速度的硬件装置来沟通，而不需要依赖于CPU的大量中断负载。
			否则，CPU需要从来源把每一片段的资料复制到暂存器，然后把它们再次写回到
			新的地方。在这个时间中，CPU 对于其他的工作来说就无法使用。	
			 
	+ I/O can be completed asynchronously(异步地)

* __Multiprogramming__(多道程序): several programs run simultaneously

	- Small jobs not delayed by large jobs
	- More overlap between I/O and CPU
	- Need memory protection between programs and/or OS
	
* __Complexity gets out of hand__(复杂性失控):

	- Multics: announced in 1963, ran in 1969
		> 1777people "contributed to Multics" (30-40 core dev)
		
		> Turing a ward lecture from Fernando Corbato(Key researcher): "On buliding systems that will fail"
		
	- Os 360: released with 1000 known bugs (APARs)
	
		- APAR (authorized program analysis report 授权程序分析报告) is a term used in IBM for a description of a problem with an IBM program that is formally tracked until a solution is provided. 
	
* __OS finally becomes an important science__:
	- How to deal with complexity?
	- UNIX based on Multics, but vastly simplified
	
	

-----------------

* A Multics System (多路信息计算系统) -- Circa (大约) 1976

![Multics](images/01-023.png "Multics")



-----------------

* Early Disk History 

![Disk](images/01-024.png "Disk")

+ Bit vs. Byte (字节与字长的区别)
	- Bit: “位”或“比特”，是计算机运算的基础，属于二进制的范筹
	- Byte: “字节”，是计算机文件大小的基本计算单位.在应用层通常是用byte来作单位，表示文件的大小，在用户看来就是可见的数据大小。 
	
			Seagate，希捷（Seagate Technology Cor）成立于1979年，目前是全球
			最大的硬盘、磁盘和读写磁头制造商，总部位于美国加州司各特谷市。希捷在设
			计、制造和销售硬盘领域居全球领先地位，提供用于企业、台式电脑、移动设备
			和消费电子的产品。
		


---------------


##### Phase 2 (1970 - 1985):	 Hardware Cheaper, Humans Expensive

* Computers available for tens of thousands of dolars instead of millions

* OS Technology maturing/stabilizing

* __Interactive timesharing__(交互式分时):
	+ Use cheap terminals (~$1000) to let *multiple users* interact with the system at the same time
	+ Sacrifice *CPU time* to get better *response time*
	+ Users do debugging, editing and email online
	
* __Problem: Thrashing__(颠簸、打乱)
	- Performance very non-linear response with load 
	- Thrashing caused by many factors including: Swapping, queueing

![Change](images/01-025.png "Change")		
	
----------------	

* The ARPANet (1968 -1970's)
	
![ARPANet](images/01-026.png "ARPANet")

* "阿帕"（ARPA），是美国高级研究计划署（Advanced Research Project Agency）的简称。他的核心机构之一是信息处理（IPTO Information Processing Techniques Office），一直在关注电脑图形、网络通讯、超级计算机等研究课题。
	- 1969年11月，美国国防部高级研究计划管理局（ ARPA ）开始建立一个命名为ARPAnet的网络，但是只有4个结点，分布在洛杉矶的加利福尼亚州大学洛杉矶分校、加州大学圣巴巴拉分校、斯坦福大学、犹他州大学四所大学的4台大型计算机。

* IMP (Interface message processor 接口信息处理机 )
	- 接口信息处理机按照ARPA网络的术语把*转发节点*通称为*接口信息处理机*。IMP是一种专用于通信的计算机，有些IMP之间直接相连，有些IMP之间必须经过其他的IMP间接相连。
	
* IBM (International Business Machines Corporation 国际商业机器公司)	
	- IBM生产并销售计算机硬件及软件，并且为系统架构和网络托管提供咨询服务。主要客户是政府和企业。IBM虽然是一家商業公司，但在材料、化学、物理等科学领域卻也有很高的成就，利用這些學術研究為基础，发明很多产品。比较有名的IBM发明的产品包括硬盘、自動櫃員機、通用产品代码、SQL、关系数据库管理系统、DRAM及沃森。
	
	
![ARPANet](images/01-027.png "ARPANet")



![ARPANet](images/01-028.png "ARPANet")

* No network to Europe
	
	
	
---------------


##### Phase 3 (1981 - ):	 Hardware Very Cheaper, Humans Very Expensive

* Computer costs $1K, Programmer costs $100K/year
	- If you can make someone 1% mor efficient by giving them a computer, it's worth it!
	- Use computers to make people more efficient
	
* __Personal computing__:
	- Computers cheap, so give everyone a PC 
	
* __Limited Hardware Resources Initially__(最初硬件资源有限):
	- Os becomes a subroutine(子程序) library (子程序库)
	- One application at a time (MSDOS, CP/M, ...)

* __Eventually PCs become powerful__:
	- OS regains all the complexity of a "big" OS
	- multiprogramming, memory protection, etc (NT, OS/2)
	
	
		    Question: As hardware gets cheaper does need for OS go
		     away?(是否随着硬件的发展、可以不再需要OS？)---- Whether they
		     ever go away hard to say.
	
	
* Graphical User Interfaces

![GUIs](images/01-029.png "GUIs")	

* Xerox Star: 第一款配有滑鼠的商业电脑,也是商业电脑特色与元素的先驱，可以说说现代电脑的元祖雏形。

`Suit(诉讼)， 1988年苹果告微软侵权、败诉。`

* Win NT: is pretty much a **real operating system(OS)** wiht actual protection just like multics

* Win 2000 / Win XP/ Win Vista/ Win 7: these are pretty much **full operating systems(OS)**

	> HAL(Hardware Abstraction Layer 硬件抽象层),位于操作系统 内核与硬件电路之间的接口层，其目的在于将硬件抽象化。 
	
	>> 将硬件差别与操作系统其他层相隔离的一薄层软件，它是通过采用使多种不同硬件在操作系统的其他部分看来是同一种虚拟机的做法来实现的。
	
	>> 最初是由Microsoft公司为确保WindowsNT的稳定性和兼容性而提出的。后来，这种HAL设计思路被一些嵌入式操作系统参考，其系统内核被分成两层，上层称为“内核(Kernel)”，底层则称为“硬件抽象层”。在EOS中，HAL独立于EOS内核；对于操作系统和应用软件而言，HAL是对底层架构的抽象。
	
	>> 它隐藏了特定平台的硬件接口细节,为操作系统提供虚拟硬件平台,使其具有硬件无关性,可在多种平台上进行移植。 从软硬件测试的角度来看，软硬件的测试工作都可分别基于硬件抽象层来完成，使得软硬件测试工作的并行进行成为可能。
	
	
	
	
---------------
	
##### Phase 3 (1981 - ):	 Distributed Systems

* Networking (Local Area Networking)
	+ Different machines share resources
	+ Printers, FIle Servers, Web Servers
	+ Client - Server Model
* Services
	- Computing 
	- File Storage(文件存储器)

![GUIs](images/01-030.png "GUIs")	



---------------
	
##### Phase 4 (1988 - ):	Internet

* Developed by the research community
	+ Based on open stndard: Internet Protocol
	+ Internet Engineering Task Force(IETF)
* Technical basis for many other types of networks
	+ Intranet: enterprise IP network
* Services Provided by the Internet
		
    - Shared access to computing resources:telnet(1970's)
    - Shared access to data/files: FTP,NFS,AFS(1980's)
    - Communication medium over which people interact
    	- email(1980's), on-line chat rooms, instant messaging(1990's)
    	- audio,video(1990's, early 00's)
    - Medium for information dissemination
    	- USENET (1980's)
    	- WWW(1990's)
    	- Audio,video(late 90's,early 00's)- replacing radio, TV?
    	- File sharing(late 90's, early 00's) 


	> Internet Protocol (互联网协议, IP): The Internet Protocol is the principal communications protocol in the Internet protocol suite(互联网协议族) for relaying packets across network boundaries. Its routing function enables internetworking, and essentially establishes the Internet.是用于封包交換数据网络的一种协议。IP是在TCP/IP协议族中网络层的主要协议，任务是根据源主机和目的主机的地址来传送数据。为此目的，IP定义了寻址方法和数据报的封装结构。它的寻径功能使网络互联，并且基本上建立了互联网。
	
	> Internet Protocol Suite (互联网协议族, IPS): 是一个网路通讯模型，以及一整个网络传输协议家族，为网络的基础通讯架构。常被称为TCP/IP协议族(TCP/IP)。该协定家族的两个核心协定: TCP(传输控制协议)和IP(互联网协议)是最早通过的标准。由于在网络通讯协议普遍采用分层的结构，当多个层次的协议共同工作时，类似计算机科学中的堆栈，因此又被称为TCP/IP协议栈。这些协议最早源发于美国国防部(DoD)的ARPA网项目，因此也被称作DoD模型。这个协定套组由互联网工程任务组负责维护。
	
	> Internet Engineering Task Force(互联网工程任务组, IETF):负责互联网标准的开发和推动。是全球互联网最具权威的技术标准化组织，当前绝大多数国际互联网技术标准出自IETF。IETF由互联网结构委员会（Internet Architecture Board, IAB）监督，IAB向互联网协会（Internet Society，ISOC）负责。


------------------
				
	
* ARPANet Evolves into Internet
	+ First E-mail SPAM message: 1 May 1978 12:33 EDT
	+ 80-83: TCP/IP, DNS; ARPANET and MILNET split
	+ 85-86: NSF builds NSFNET as backbone, links 6 Supercomputer centers, 1.5 Mbps, 10,000 computers
	
	

----------------

* Communication Network: refers to the method that employees pass on information to other employees in an organization. Four different types: the wheel network, chain network, circle network, and all-channel network.


![GUIs](images/01-031.png "GUIs")	

---------------




* What is a Communication Network? (End-system CEntric View)
	+ Network offers one basiv service: move information
		- Bird, fire, messenger, turck, telegraph, telephone, internet...
		- Another example, transportation sevice: move objects (Horse, train, truck, airplane...)
	+ What distinguish different types of networks?
		- The sevices they provide
	+ What distinguish the sevices?
		- Latency (潜在因素)
		- Bandwidth (Highest BW? "Sneakernet")
		- Loss rate (损失率)
		- Number of end systems (端系统)
		- Servive interface (how to invoke the service?)
		- Others (Reliability, unicast vs. multicast, real-time...)
		
	
	
* What is a Communication Network? (Infrastructure Centric View)
	+ Communication medium: electron, photon
	+ Network components:
		- Links - carry bits from one place to another (or maybe multiple places): fiber, copper, satellite, ...
		- Interface - attach devices to links
		- Switches/routers - interconnect links: electronic/optic, crossbar/Banyan
		- Hosts - communication endpoints: workstations, PDAs, cell phones, toasters
	+ Protocols - rules governing communication between nodes
		- TCP/IP, ATM, MPLS, SONET, Ethernet, X.25
		

	
* Types of Networks
	+ Geographical distance
		- Local Area Networkss(LAN): Ethernet, Token ring, EDDI
		- Metropolitan Area Networks (MAN): DQDB, SMDS
		- WIde Areaa Networks(WAN): DQDB, SMDS
		- Wide Area Networks(WAN): X.25, ATM, frame relay
		- Caveat(附加说明、警告): LAN, MAN, WAN may mean different things (Service, network technology, networks)
	
![LAN](images/01-040.png "LAN")


![ISP&NAP](images/01-041.png "ISP&NAP")	

>> ISP(互联网服务提供商 Internet Service Provider)，即向广大用户综合提供互联网接入业务、信息业务、和增值业务的电信运营商。
 
>> NAP是因特网的路由选择层次体系中的通信交换点。 每个网络接入点都由一个共享交换系统或者局域网组成，用来交换业务量。通达因特网主干线的点。ISP互相连接的点。NAP 可用作主要业务提供者的数据互换点。

>> ICP(互联网内容提供商 Internet Content Provider），向广大用户综合提供互联网信息业务和增值业务的电信运营商。
	
------------	
	
> LAN(局域网 Local Area Network),is a computer network that interconnects computers within a limited area such as a residence, school, laboratory, university campus or office building.
> 指在某一区域内由多台计算机互联成的计算机组。一般是方圆几千米以内。局域网可以实现文件管理、应用软件共享、打印机共享、工作组内的日程安排、电子邮件和传真通信服务等功能。局域网是封闭型的，可以由办公室内的两台计算机组成，也可以由一个公司内的上千台计算机组成
![LAN](images/01-032.png "LAN")
	
> 具体到路由器，我们一般组网，都是组建的LAN网络，用户在局域网里通信、传输文件。其获取到的是内部IP，LAN 内部是交换机。我们可以不连接 WAN 口，把路由器当做普通交换机来使用, 一般用到的LAN的场景：

>>1.接电脑的网线，需要插到路由器的LAN口

>>2.二级路由，一般都是从上级路由的LAN口接线
![LAN](images/01-033.png "LAN")
	
> 一般路由器的LAN口会图上颜色和WAN口区分，一般LAN口数目会多于LAN口。
![LAN](images/01-034.png "LAN")
	
-----------------
	
> WAN(广域网 Wide Area Network ), not only covers a larger geographic distance, but also generally involves leased telecommunication circuits.(租用电信电路)
> 是一种跨越大的、地域性的计算机网络的集合。通常跨越省、市，甚至一个国家。广域网包括大大小小不同的子网，子网可以是局域网，也可以是小型的广域网。
![WAN](images/01-035.png "WAN")
	
> 接外部 IP 地址用，通常指的是出口，转发来自内部 LAN 接口的 IP 数据包。基本每个路由器都有WAN口，当然也有路由猫这种特例。WAN的应用场景：

>> 1.从猫引出的来网线，要插到路由器的WAN口

>> 2.二级路由，上级网线插到二级路由的WAN口	
![WAN](images/01-036.png "WAN")

> 一般路由器都会有一个WAN口，也有多个WAN口的路由。
![WAN](images/01-037.png "WAN")
	
	
-----------

> WLAN(无线局域网 Wireless LAN), 和LAN不同，WLAN的数据通过电磁波传输，也就是常说的空气传输。WLAN 利用电磁波在空气中发送和接受数据，而无需线缆介质。
> 使用 ISM (Industrial、Scientific、Medical) 无线电广播频段通信。WLAN 的 802.11a 标准使用 5 GHz 频段，支持的最大速度为 54 Mbps，而 802.11b 和 802.11g 标准使用 2.4 GHz 频段，分别支持最大 11 Mbps 和 54 Mbps 的速度。最新的11AC已经达到竟然的1.3Gbps。
![WLAN](images/01-038.png "WLAN")

> 由于WLAN采用全新的802.11协议，其设置要比普通的有限路由器复杂，好多朋友在配置无线网络的时候遇到问题。同时，WLAN没有硬件接口，支持WLAN的路由器，可以通过ISM定义的电磁波来彼此通信。

-----------

>> Summary: 

>> WAN 口是对外的接口，和运营商、上级网络打交道。LAN 和W LAN 是对内的接口，内部的电脑、手机、PAD，都是接入到 LAN 或者 WLAN。一般的无线路由器，包含了完整的LAN、WAN、WLAN功能。
![Summary](images/01-039.png "Summary")	

----------

> WAN(城域网 Metropolitan Area Network),是在一个城市范围内所建立的计算机通信网。在80年代末，在LAN的发展基础上提出的，在技术上与LAN有很多相似，而与广域网(WAN)区别较大。
> WAN一个重要用途是用作骨干网，通过它将位于同一城市内不同地点的主机、数据库，以及LAN等互联网连接起来，这与WAN的作用有相似之处，但两者在实现方法与性能上有很大差别。WAN不仅用于计算机通信，同时可用于传输话音、图像等信息，成为一种综合利用的通信网，但属于计算机通信网的范畴，不同于综合业务通信网(ISDN)。	
	
	
------------
	
* The Morris Internet Worm (1988)
![The Morris Internet Worm](images/01-042.png "The Morris Internet Worm")	
	
	
* LoveLetter Virus
![LoveLetter Virus](images/01-043.png "LoveLetter Virus")	
	

---------------
	
##### Phase 5 (1995 - ):	Mobile Systems

![Mobile](images/01-044.png "Mobile")

![SSIS](images/01-045.png "Societal Scale Information System")

![DataCenter](images/01-046.png "DataCenter")

* OS Concept & Feature
![OS](images/01-047.png "OS")



---------------

### Summary: History of OS

* Change is continuous and OSs should adapt
	+ Not: look how stupid batch processing was
	+ But: made sense at the time
* Situation today is much like the late 60s
	+ Small OS: 100K lines
	+ Large OS： 10M lines(5M for the browser!) -- (100-100 people-years)
	
* Complexity still reigns
	+ NT developed (early to late 90's): Never worked well
	+ Windows 2000/XP: Very successful
	+ Windows Vista (aka "Longhorn") delayed many times
		- Finnaly released in Jan 2007
		- Promised by removing some of the intended technology
		- Slow adoption rate, even in 2008/2009

		
-----------


* OS __Components__ -- What are the pieces of the OS?
	+ Process Management
	+ Main-Memory Management
	+ I/O System Management
	


* OS __Services__ -- What things does OS do?
	+ Services that (more-or-less) map onto(映射到) components
		+ Program excution
			- How do you execute concurrent sequences of instructions?
		+ I/O operations
			- Standardized interfaces to extremely diverse devices
		+ File system manipulation
			- How do you read/write/preserve fiels?
			- Looming concern(隐忧): How do you even find files?
		+ Communications
			- Networking protocols/Interface with CyberSpace?
	+ Cross-cutting capabilities(横切能力)
		- Error detection & recovery
		- Resource allocation
		- Accounting
	
	
* OS __Structure__ -- What is the organizational Principle?

1. Simple - Only one/two levels of code

> Simple Structure
![Simple](images/01-048.png "Simple")

> UNIX: Also "Simple" Structure, UNIX -- limited by hardware functionality
>> Original UNIX operating system consists of two separable parts:
>>> 1. Systems programs
>>> 2. The kernel 

>>>> Consists of everything below the system-call interface and above the physical hardware

>>>>  Provides the file system, CPU scheduling, memory management, and other operating-system functoins;
 
>>>> Many interacting functions for one level

>![UNIX](images/01-049.png "UNIX")
	
	
	
2. Layered - Lower levels independent of upper levels
	+ OS id divided many layers(levels)
		- Each built on top of lower layers
		- Bottom layer (layer 0) is hardware
		- Highest layer (layer N) is the user interface
	+ Each layer uses functions(operations) and services of only lower-level layers
		- Advantage: modularity --> Easier debugging/Maintenance
		- Not always possible: Does process scheduler lie above or below virtual memory layer?
			--> Need to reschedule processor while waiting or paging
			--> May need to page in information about tasks


	
3. Microkernel - OS built from many user-level processes
	+ Move as much form the kernel into "user" space 
		- Small core OS running at kernel level
		- OS Services built from many independent user-level processes
	+ Communication between modules with message passing 
	+ Benefits:
		- Easier to extend a microkernel
		- Easier to port OS to new architectures
		- More reliable (less code is running in kernel mode)
		- Fault Isolation (parts of kernel protected from other parts)
		- More secure



4. Modular
		- Core kernel with Dynamically loadable modules
	
	
	
* Conclusion
![Conclusion](images/01-050.png)
	
	
	
	
	
	
	
	
	
	
	