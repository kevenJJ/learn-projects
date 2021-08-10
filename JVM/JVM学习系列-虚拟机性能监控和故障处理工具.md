### 第4章 虚拟机性能监控和故障处理工具


#### 4.1 概述


#### 4.2 基础故障处理工具


##### 4.2.1 jps:虚拟机进程状况工具
jps （JVM Process Status Tool）
jps [options] [hostid]

| options | application |
| ----    | ----        |
| -q      | 只输出LVMID,省略主类的名称 |
| -m      | 输出虚拟机进行启动时传递给主类的 main() 函数的参数 |
| -l      | 输出主类的全名，如果进程执行的是 JAR 包，则输出 JAR 路径 |
| -v      | 输出虚拟机进程启动时的 JVM 参数 |

##### 4.2.2 jstat:虚拟机统计信息监视工具
jstat(JVM Statistics Monitoring Tool) 是用于监视虚拟机各种运行状态信息的命令行工具。
可以显示本地或者远程虚拟机进程中的类加载、内存、垃圾收集、即使编译等运行时数据
jstat [option vmid [interval[s|ms][count]]]
VMID与LVMID 说明：
如果是本地虚拟机进程，VMID与LVMID是一致的；如果是远程虚拟机进程，那VMID的格式应当是：[protocol:][//] lvmid [@hostname[:port]/servername]
参数 interval 和 count 代表查阅间隔和次数，如果省略这2个参数，说明只查询一次。
选项option代表用户希望查询的虚拟机信息，主要分三类：
* 类加载    -class
![](img/-class.png)

* 垃圾收集  -gc
  * -gccapacity 监视内容与-gc基本相同，但输出主要关注 Java 堆各个区域使用到最大、最小空间
  ![](img/-gccapacity.png)
  * -gcutil 监视内容与-gc基本相同，但输出主要关注已使用空间占总空间的百分比
![](img/-gcutil.png)

  * -gccause 与-gcutil 功能一样，但是会额外输出导致上一次垃圾收集产生的原因
  ![](img/-gccause.png)
  * -gcnew 监视新生代垃圾收集状况
  ![](img/-gcnew.png)
  * -gcnewcapacity 监视内容与-gcnew基本相同，输出主要关注使用到的最大、最小空间
  * -gcold  监视老年代垃圾收集状况
  ![](img/-gcold.png)
  * -gcoldcapacity 监视内容与-gcold内同相同，输出主要关注使用到的最大、最小空间。
  ![](img/-gcoldcapacity.png)
  * -gcpermcapacity 输出永久代使用到最大、最小空间。
  * -gcmetacapacity 输出元空间使用到最大、最小空间
  ![](img/-gcmetacapacity.png)
* 运行期编译状况    -compiler 输出即使编译器编译过的方法、耗时等信息
![](img/-compiler.png)
* -printcompilation 输出已经被即使编译的方法
![](img/-printcompilation.png)


##### 4.2.3 jinfo:Java配置信息工具
jinfo（configuration info for java）的作用是实时查看和调整虚拟机各项参数。


##### 4.2.4 jmap:Java内存影像工具
jmap（Memory Map For Java）命令用于生成堆转储快照（一般称为heapdump或dump）
jmap [option] vmid

|   选项   | 作用    |
|   ----   | ---- |
|   -dump       |   生成Java堆转储快照。格式为 -dump:[live,]format=b,file=<filename>,其中 live 子参数说明是否只 dump 出存活的对象。|
| -finalizerinfo | 显示在 F-Queue 中等待 Finalizer 线程执行 finalize 方法的对象。只在 linux/Solaris 平台上有效  |
| -heap | 显示 Java 堆详细信息，如使用哪种回收器、参数配置、分代状况等。只在 Linux/Solaris 平台下有小 |
| -histo | 显示堆中对象统计信息，包括类、实例数量、合计容量等 |
| -permstat | 以 ClassLoader 为统计口径显示永久代内存状态   |
| -F | 当虚拟机进程对 -dump 选项没有响应时，可使用这个选项强制生成dump 快照。



