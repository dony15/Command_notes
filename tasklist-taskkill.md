# tasklist-taskkill

[TOC]



### 1.概念

windows 封装的cmd命令,具有比较友好的使用文档(包含中文参数介绍,组合演示等)

tasklist和taskkill组合使用能够完成开发中监控web项目的大部分操作

### 2.tasklist

 该工具显示在**本地**或**远程机器**上当前运行的进程列表。

```shell
#常用指令
tasklist #查看进程信息,如:进程名 PID 会话名 会话 内存使用
tasklist | findstr PID #查看该PID的查看进程信息 如:进程名 PID 会话名 会话 内存使用
tasklist /FI "PID eq 8924" #查看PID = 8924的进程信息
```



```shell
tasklist /?  #查看比较详细的文档说明
C:\Users\banma530\Desktop>tasklist /?

TASKLIST [/S system [/U username [/P [password]]]]
         [/M [module] | /SVC | /V] [/FI filter] [/FO format] [/NH]

描述:
    该工具显示在本地或远程机器上当前运行的进程列表。

参数列表:
   /S     system           指定连接到的远程系统。

   /U     [domain\]user    指定应该在哪个用户上下文执行这个命令。

   /P     [password]       为提供的用户上下文指定密码。如果省略，则
                           提示输入。

   /M     [module]         列出当前使用所给 exe/dll 名称的所有任务。
                           如果没有指定模块名称，显示所有加载的模块。

   /SVC                    显示每个进程中主持的服务。

   /V                      显示详述任务信息。

   /FI    filter           显示一系列符合筛选器指定的标准的任务。

   /FO    format           指定输出格式。
                           有效值: "TABLE"、"LIST"、"CSV"。

   /NH                     指定列标题不应该在输出中显示。
                           只对 "TABLE" 和 "CSV" 格式有效。

   /?                      显示帮助消息。


筛选器:
    筛选器名        有效操作符                有效值
    -----------     ---------------           --------------------------
    STATUS          eq, ne                    RUNNING |
                                              NOT RESPONDING | UNKNOWN
    IMAGENAME       eq, ne                    映像名称
    PID             eq, ne, gt, lt, ge, le    PID 值
    SESSION         eq, ne, gt, lt, ge, le    会话编号
    SESSIONNAME     eq, ne                    会话名
    CPUTIME         eq, ne, gt, lt, ge, le    CPU 时间，格式为
                                              hh:mm:ss。
                                              hh - 时，
                                              mm - 分，ss - 秒
    MEMUSAGE        eq, ne, gt, lt, ge, le    内存使用量，单位为 KB
    USERNAME        eq, ne                    用户名，格式为 [domain\]user
    SERVICES        eq, ne                    服务名称
    WINDOWTITLE     eq, ne                    窗口标题
    MODULES         eq, ne                    DLL 名称

说明: 当查询远程机器时，不支持 "WINDOWTITLE" 和 "STATUS"
      筛选器。

示例:
    TASKLIST
    TASKLIST /M
    TASKLIST /V /FO CSV
    TASKLIST /SVC /FO LIST
    TASKLIST /M wbem*
    TASKLIST /S system /FO LIST
    TASKLIST /S system /U domain\username /FO CSV /NH
    TASKLIST /S system /U username /P password /FO TABLE /NH
    TASKLIST /FI "USERNAME ne NT AUTHORITY\SYSTEM" /FI "STATUS eq running"

```



### 3.taskkill

使用该工具按照进程 ID (**PID**) 或**映像名称**终止任务。

```shell
taskkill /? #查看比较详细的文档说明

C:\Users\banma530\Desktop>taskkill /?

TASKKILL [/S system [/U username [/P [password]]]]
         { [/FI filter] [/PID processid | /IM imagename] } [/T] [/F]

描述:
    使用该工具按照进程 ID (PID) 或映像名称终止任务。

参数列表:
    /S    system           指定要连接的远程系统。

    /U    [domain\]user    指定应该在哪个用户上下文执行这个命令。

    /P    [password]       为提供的用户上下文指定密码。如果忽略，提示
                           输入。

    /FI   filter           应用筛选器以选择一组任务。
                           允许使用 "*"。例如，映像名称 eq acme*

    /PID  processid        指定要终止的进程的 PID。
                           使用 TaskList 取得 PID。

    /IM   imagename        指定要终止的进程的映像名称。通配符 '*'可用来
                           指定所有任务或映像名称。

    /T                     终止指定的进程和由它启用的子进程。

    /F                     指定强制终止进程。

    /?                     显示帮助消息。

筛选器:
    筛选器名      有效运算符                有效值
    -----------   ---------------           -------------------------
    STATUS        eq, ne                    RUNNING |
                                            NOT RESPONDING | UNKNOWN
    IMAGENAME     eq, ne                    映像名称
    PID           eq, ne, gt, lt, ge, le    PID 值
    SESSION       eq, ne, gt, lt, ge, le    会话编号。
    CPUTIME       eq, ne, gt, lt, ge, le    CPU 时间，格式为
                                            hh:mm:ss。
                                            hh - 时，
                                            mm - 分，ss - 秒
    MEMUSAGE      eq, ne, gt, lt, ge, le    内存使用量，单位为 KB
    USERNAME      eq, ne                    用户名，格式为 [domain\]user
    MODULES       eq, ne                    DLL 名称
    SERVICES      eq, ne                    服务名称
    WINDOWTITLE   eq, ne                    窗口标题

    说明
    ----
    1) 只有在应用筛选器的情况下，/IM 切换才能使用通配符 '*'。
    2) 远程进程总是要强行 (/F) 终止。
    3) 当指定远程机器时，不支持 "WINDOWTITLE" 和 "STATUS" 筛选器。

例如:
    TASKKILL /IM notepad.exe
    TASKKILL /PID 1230 /PID 1241 /PID 1253 /T
    TASKKILL /F /IM cmd.exe /T
    TASKKILL /F /FI "PID ge 1000" /FI "WINDOWTITLE ne untitle*"
    TASKKILL /F /FI "USERNAME eq NT AUTHORITY\SYSTEM" /IM notepad.exe
    TASKKILL /S system /U domain\username /FI "USERNAME ne NT*" /IM *
    TASKKILL /S system /U username /P password /FI "IMAGENAME eq note*"

```

