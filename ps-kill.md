# ps-kill 本地查询和关闭进程套装

[TOC]

### 1.概念

##### 1-1.ps介绍

Linux中的ps指令是process status的缩写,用来列出系统中当前运行的进程

ps列出的进程信息是快照的形式,一次性查看,如果需要动态显示,则使用top指令查看



##### 1-2.kill介绍

用于杀死进程,linux中最常见的一种终止程序执行的命令



##### 1-3.linux上进程有五种状态

1. 运行(正在运行或者运行队列)
2. 中断(休眠,手足,等待某个条件形成或接收到信号)
3. 不可中断(收到信号不唤醒和不可运行,进程必须等待直到有中断发生)
4. 僵死(进程已停止,但进程描述符存在,直到父进程调用wait()系统调用后释放)
5. 停止(进程收到SIGSTOP,SIGSTP,SIGTIN,SIGTOU信号后停止运行)

> D 不可中断 uninterruptible sleep (usually IO) 
>
> R 运行 runnable (on run queue) 
>
> S 中断 sleeping 
>
> T 停止 traced or stopped 
>
> Z 僵死 a defunct (”zombie”) process 



### 2.ps指令

##### 语法

```shell
ps [参数]
```

##### 参数列表

```shell
#基础命令元素
-A #显示所有进程 pid tty(文本的输入输出环境) time(cpu用掉的时间) CMD概述四种信息
-a #显示同一终端下所有的进程(使用较少)
c  #显示进程真实名称(使用较少)
e  #显示环境变量
-e #效果同-A
f  #显示程序间的关系 根据tty(过滤无环境进程)显示相关进程
-au #当前中断比较详细的咨询
-aux #显示所有使用者的行程(正在内存中的进程)

#实用组合
-ef连用 #显示所有进程,并显示进程的关系 ppid(父进程) stime(日期) C(cpu资源消耗)
-ef | grep 进程关键字(模糊查询) #查询指定进程信息 

```

##### 显示信息说明

```shell
F 代表这个程序的旗标 (flag)， 4 代表使用者为 super user
S 代表这个程序的状态 (STAT)，关于各 STAT 的意义将在内文介绍
UID 程序被该 UID 所拥有
PID 就是这个程序的 ID ！
PPID 则是其上级父程序的ID
C CPU 使用的资源百分比
PRI 这个是 Priority (优先执行序) 的缩写
NI 这个是 Nice 值
ADDR 这个是 kernel function，指出该程序在内存的那个部分。如果是个 running的程序，一般就是 "-"
SZ 使用掉的内存大小
WCHAN 目前这个程序是否正在运作当中，若为 - 表示正在运作
TTY 登入者的终端机位置
TIME 使用掉的 CPU 时间。
CMD 所下达的指令

USER：该 process 属于那个使用者账号的
PID ：该 process 的号码
%CPU：该 process 使用掉的 CPU 资源百分比
%MEM：该 process 所占用的物理内存百分比
VSZ ：该 process 使用掉的虚拟内存量 (Kbytes)
RSS ：该 process 占用的固定的内存量 (Kbytes)
TTY ：该 process 是在那个终端机上面运作，若与终端机无关，则显示 ?，另外， tty1-tty6 是本机上面的登入者程序，若为 pts/0 等等的，则表示为由网络连接进主机的程序。
STAT：该程序目前的状态，主要的状态有
R ：该程序目前正在运作，或者是可被运作
S ：该程序目前正在睡眠当中 (可说是 idle 状态)，但可被某些讯号 (signal) 唤醒。
T ：该程序目前正在侦测或者是停止了
Z ：该程序应该已经终止，但是其父程序却无法正常的终止他，造成 zombie (疆尸) 程序的状态
START：该 process 被触发启动的时间
TIME ：该 process 实际使用 CPU 运作的时间
COMMAND：该程序的实际指令
```



##### 信息输出到指定位置

```
ps [参数] > file  #如下
ps -ef | grep "ssh" > demo.txt
```



### 3.kill指令

##### 语法

```
kill [num] PID
```

##### 参数列表

```shell
无参 一般是在程序能够正常运行的情况下,友好的使用方式
-2 与ctrl+c的动作相同,通知程序停止执行
-9 强制停止
-15 正常的程序通知停止执行,预设的讯号
-l 列出所有讯号(这是小写L,查看kill的64个预设讯号,不需要PID)
```

