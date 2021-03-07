# 最强图解OSI模型

首先，如果可以科学上网的同学，我强烈建议大家去看原视频，真的是==墙裂推荐==！！

视频链接在这里:https://www.youtube.com/watch?v=vv4y_uOneC0&ab_channel=TechTerms

参考资料: [OSI七层模型详解](https://blog.csdn.net/yaopeng_2005/article/details/7064869?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-8.edu_weight&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-8.edu_weight)



如果不能或者看完的同学也可以看一下，下面的讲解(如有错误之处请在评论区批评指正:happy:)



### 为什么有OSI模型？

我们知道互联网其实就是很多计算机成为结点连成的计算机网络，现在我们考虑最简单的情况，两台计算的的通信，这两台计算机通过网线进行连接（插口是RJ45,使用网络接口控制器(NIC)进行解析）,在两台计算的系统是一样的情况下，由于同一系统所用的接口都是一样的，所以可以很容易的进行通信，传递数据。就像A、B两个人都是说的中文，那么两个人很好交流。

<table>
    <tr>
        <td>
        <img src="https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201020234243516.png" style="zoom:65%;">
        </td>
        <td>
            <img src=https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201020234335017.png border=2>
        </td>
    </tr>
</table>
![image-20201021151315060](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021151315060.png)

图源：百度百科-RJ45

那如果一台是WINDOWS,另外一台是MAC OS，该怎么传递数据呢？类比刚刚的例子，现在就像是A是说的中、B是说的英文，而且他们都自听得懂自己所说的语言，那么A和B就有沟通障碍了，这时候要是有个翻译官就好了。那么用谁来充当这两台电脑的翻译官呢，OSI模型就是，通过这个协议将两者的数据统一，那么就可以通信了。

<img src="https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201020234626922.png" alt="image-20201020234626922"  />

how there two computers are going to communicate successfully with each other?

### OSI模型

OSI（Open System Interconnect），即开放式系统互联。 一般都叫OSI参考模型，是ISO（国际标准化组织）组织在1984年研究的网络互连模型。ISO为了更好的使网络应用更为普及，推出了OSI参考模型。其含义就是推荐所有公司使用这个规范来控制网络。这样所有公司都有相同的规范，就能互联了。

![image-20201020234902281](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201020234902281.png)

![image-20201020235943518](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201020235943518.png)

![](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/0_1325744597WM32.gif)

图源: [OSI七层模型详解](https://blog.csdn.net/yaopeng_2005/article/details/7064869?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-8.edu_weight&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-8.edu_weight)

OSI Model is Protocols

![image-20201020235006496](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201020235006496.png)

### Application Layer



应用层（Application Layer）是OSI参考模型的最高层，它是计算机用户，以及各种应用程序和网络之间的接口，其功能是直接向用户提供服务，完成用户希望在网络上完成的各种工作。它在其他6层工作的基础上，负责完成网络中应用程序与网络操作系统之间的联系，建立与结束使用者之间的联系，并完成网络用户提出的各种网络服务及应用所需的监督、管理和服务等各种协议。此外，该层还负责协调各个应用程序间的工作。
应用层为用户提供的服务和协议有：

文件服务、目录服务、文件传输服务（FTP）、远程登录服务（Telnet）、电子邮件服务（E-mail）、打印服务、安全服务、网络管理服务、数据库服务等。上述的各种网络服务由该层的不同应用协议和程序完成，不同的网络操作系统之间在功能、界面、实现技术、对硬件的支持、安全可靠性以及具有的各种应用程序接口等各个方面的差异是很大的。



应用层的主要功能如下：

- **用户接口**：应用层是用户与网络，以及应用程序与网络间的直接接口，使得用户能够与网络进行交互式联系。
- **实现各种服务**：该层具有的各种应用程序可以完成和实现用户请求的各种服务。

![image-20201020235228190](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201020235228190.png)

### Presentation Layer

表示层（Presentation Layer）是OSI模型的第六层，它对来自应用层的命令和数据进行解释，对各种语法赋予相应的含义，并按照一定的格式传送给会话层。其主要功能是“处理用户信息的表示问题，如编码、数据格式转换和加密解密”等。

表示层的具体功能如下：

- **数据格式处理(Translation)**：协商和建立数据交换的格式，解决各应用程序之间在数据格式表示上的差异。
- **数据的编码**：处理字符集和数字的转换。例如由于用户程序中的数据类型（整型或实型、有符号或无符号等）、用户标识等都可以有不同的表示方式，因此，在设备之间需要具有在不同字符集或格式之间转换的功能。
- **压缩和解压缩(Data Compression)**：为了减少数据的传输量，这一层还负责数据的压缩与恢复。
- **数据的加密和解密(Encryption/Decryption)**：可以提高网络的安全性。

![image-20201021000024430](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021000024430.png)

![image-20201021000309332](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021000309332.png)

Data Compression can make data transmission done  very fast.  Thus data compression is very  helpful in real-time video and audio-streaming

![image-20201021000705419](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021000705419.png)

![image-20201021000205639](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021000205639.png)

数据的加密和解密Encryption/Decryption

![image-20201021000933388](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021000933388.png)

### Session Layer

想象一下，你像举办一次Party，你雇了帮手来帮你保证这次party能圆满的、不出意外的进行。你需要他们布置场地、打扫卫生、提供酒水、招待客人。

![image-20201021001026051](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021001026051.png)

Session Layer 也是和举办party一样，有两个帮手APIs、NETBIOS

![image-20201021001427748](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021001427748.png)

会话层（Session Layer）是OSI模型的第5层，是用户应用程序和网络之间的接口，主要任务是：向两个实体的表示层提供建立和使用连接的方法。将不同实体之间的表示层的连接称为会话。因此会话层的任务就是组织和协调两个会话进程之间的通信，并对数据交换进行管理。
用户可以按照半双工、单工和全双工的方式建立会话。当建立会话时，用户必须提供他们想要连接的远程地址。而这些地址与MAC（介质访问控制子层）地址或网络层的逻辑地址不同，它们是为用户专门设计的，更便于用户记忆。域名（DN）就是一种网络上使用的远程地址例如：www.baidu.com就是一个域名。

会话层的具体功能如下：

- **会话管理**：允许用户在两个实体设备之间建立、维持和终止会话，并支持它们之间的数据交换。例如提供单方向会话或双向同时会话，并管理会话中的发送顺序，以及会话所占用时间的长短。
- **会话流量控制**： 的错误，而是磁盘空间、打印机缺纸等类型的高级错误。

会话管理：这里是与服务器会话的建立

![image-20201021001648201](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021001648201.png)

当你与服务器建立连接后，你想要下载一个文件或者访问一些资源，那么服务器会判断你是否有这个权限，这也是会话层的任务。

![image-20201021001716048](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021001716048.png)



![image-20201021001736091](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021001736091.png)

![image-20201021001823568](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021001823568.png)





![image-20201021002006749](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021002006749.png)

![image-20201021002112301](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021002112301.png)

### Transport Layer

网络层（Network Layer）是OSI模型的第三层，它是OSI参考模型中最复杂的一层，也是通信子网的最高一层。它在下两层的基础上向资源子网提供服务。

其主要任务是：

通过路由选择算法，为报文或分组通过通信子网选择最适当的路径。该层控制数据链路层与传输层之间的信息转发，建立、维持和终止网络的连接。具体地说，数据链路层的数据在这一层被转换为数据包，然后通过路径选择、分段组合、顺序、进/出路由等控制，将信息从一个网络设备传送到另一个网络设备。

一般地，数据链路层是解决同一网络内节点之间的通信，而网络层主要解决不同子网间的通信。例如在广域网之间通信时，必然会遇到路由（即两节点间可能有多条路径）选择问题。 

在实现网络层功能时，需要解决的主要问题如下：

- 寻址：数据链路层中使用的物理地址（如MAC地址）仅解决网络内部的寻址问题。在不同子网之间通信时，为了识别和找到网络中的设备，每一子网中的设备都会被分配一个唯一的地址。由于各子网使用的物理技术可能不同，因此这个地址应当是逻辑地址（如IP地址）。
-  交换：规定不同的信息交换方式。常见的交换技术有：线路交换技术和存储转发技术，后者又包括报文交换技术和分组交换技术。
-  路由算法：当源节点和目的节点之间存在多条路径时，本层可以根据路由算法，通过网络为数据分组选择最佳路径，并将信息从最合适的路径由发送端传送到接收端。

- 连接服务：与数据链路层流量控制不同的是，前者控制的是网络相邻节点间的流量，后者控制的是从源节点到目的节点间的流量。其目的在于防止阻塞，并进行差错检测。



![image-20201021002210019](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021002210019.png)

Segmentation

![image-20201021002427364](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021002427364.png)



![image-20201021002533692](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021002533692.png)

![image-20201021002630854](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021002630854.png)

![image-20201021002749295](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021002749295.png)

![image-20201021002809799](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021002809799.png)





![image-20201021002850416](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021002850416.png)

![image-20201021002921524](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021002921524.png)



![image-20201021003138020](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021003138020.png)

![image-20201021003008337](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021003008337.png)

![image-20201021003050274](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021003050274.png)





![image-20201021003249923](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021003249923.png)

![image-20201021003343968](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021003343968.png)

![image-20201021003307055](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021003307055.png)

![image-20201021003515407](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021003515407.png)



![image-20201021003637108](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021003637108.png)

### Data Link Layer

数据链路层（Data Link Layer）是OSI模型的第二层，负责建立和管理节点间的链路。

该层的主要功能是：通过各种控制协议，将有差错的物理信道变为无差错的、能可靠传输数据帧的数据链路。
在计算机网络中由于各种干扰的存在，物理链路是不可靠的。因此，这一层的主要功能是在物理层提供的比特流的基础上，通过**差错控制、流量控制方法**，使有差错的物理线路变为无差错的数据链路，即提供可靠的通过物理介质传输数据的方法。
该层通常又被分为介质访问控制（MAC）和逻辑链路控制（LLC）两个子层。

MAC子层的主要任务是解决共享型网络中多用户对信道竞争的问题，完成网络介质的访问控制；

LLC子层的主要任务是建立和维护网络连接，执行差错校验、流量控制和链路控制。
数据链路层的具体工作是接收来自物理层的位流形式的数据，并封装成帧，传送到上一层；同样，也将来自上层的数据帧，拆装为位流形式的数据转发到物理层；并且，还负责处理接收端发回的确认帧的信息，以便提供可靠的数据传输。

![image-20201021003726095](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021003726095.png)



![image-20201021003803498](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021003803498.png)



![image-20201021003850372](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021003850372.png)



![image-20201021003914548](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021003914548.png)



![image-20201021004048066](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021004048066.png)

![image-20201021004318203](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021004318203.png)

![image-20201021004237629](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021004237629.png)

### Physical Layer

在OSI参考模型中，物理层（Physical Layer）是参考模型的最低层，也是OSI模型的第一层。
物理层的主要功能是：利用传输介质为数据链路层提供物理连接，实现比特流的透明传输。
物理层的作用是实现相邻计算机节点之间比特流的透明传送，尽可能屏蔽掉具体传输介质和物理设备的差异。使其上面的数据链路层不必考虑网络的具体传输介质是什么。“透明传送比特流”表示经实际电路传送后的比特流没有发生变化，对传送的比特流来说，这个电路好像是看不见的。



![image-20201021004541235](https://picgo-w.oss-cn-chengdu.aliyuncs.com/img/image-20201021004541235.png)