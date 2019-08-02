---
title: supervisor 进程管理
date: 2019-08-02 14:20:50
tags:
  - supervisor
---

#### 安装和示例
```
# 安装
sudo apt-get install supervisor
# or
pip install supervisor

# 测试是否安装成功
echo_supervisord_conf


# 创建配置文件
echo_supervisord_conf > /etc/supervisord.conf 

# 启动服务
supervisord -c /etc/supervisord.conf
# 停止服务
supervisorctl shutdown

# 进程管理(supervisorctl help)
supervisorctl start
supervisorctl stop
supervisorctl reload
supervisorctl restart
supervisorctl status

# 开启web页面
[inet_http_server]         ; inet (TCP) server disabled by default
port=*:9001                ; ip_address:port specifier, *:port for all iface
username=user              ; default is no username (open server)
password=123               ; default is no password (open server)



# 示例
[program:task1]                   ;":"后面是程序名称
command=php task1.php             ;程序的启动命令
;process_name=%(program_name)s_%(process_num)s   ;进程名称
;numprocs=1                       ; 启动时的进程数 (默认 1)
directory=/root                   ;执行时切换到的目录(def no cwd)
user=root                         ;指定用户运行
startsecs=3                       ;在设定时间内，程序必须保持运行(def. 1)
;startretries=3                   ;当启动失败时尝试的最大次数(def 3)
autostart=true                    ;是否跟随supervisord启动 (def: true)
autorestart=true                  ;false[挂掉不重启],unexpected[非指定退出码重启],true[无条件重启](def: unexpected)
;exitcodes=0,2                    ;'expected' 符合退出代码之后去重启 (def 0,2)
redirect_stderr=true              ;是否开启程序标准输入输出重定向(def false)
stdout_logfile_maxbytes=50MB      ;文件最大大小 # 日志文件进行切割 (def 50MB)
stdout_logfile_backups=10         ; # 日志文件备份数目 (def 10)
stdout_logfile=/root/task1.log    ;标准输出路径;(def AUTO)
```

####  配置说明

```
- command：启动程序使用的命令，可以是绝对路径或者相对路径
- process_name：一个python字符串表达式，用来表示supervisor进程启动的这个的名称，默认值是%(program_name)s
- numprocs：Supervisor启动这个程序的多个实例，如果numprocs>1，则process_name的表达式必须包含%(process_num)s，默认是1
- numprocs_start：一个int偏移值，当启动实例的时候用来计算numprocs的值
- priority：权重，可以控制程序启动和关闭时的顺序，权重越低：越早启动，越晚关闭。默认值是999
- autostart：如果设置为true，当supervisord启动的时候，进程会自动重启。
- autorestart：值可以是false、true、unexpected。false：进程不会自动重启，unexpected：当程序退出时的退出码不是exitcodes中定义的时，进程会重启，true：进程会无条件重启当退出的时候。
- startsecs：程序启动后等待多长时间后才认为程序启动成功
- startretries：supervisord尝试启动一个程序时尝试的次数。默认是3
- exitcodes：一个预期的退出返回码，默认是0,2。
- stopsignal：当收到stop请求的时候，发送信号给程序，默认是TERM信号，也可以是 HUP, INT, QUIT, KILL, USR1, or USR2。
- stopwaitsecs：在操作系统给supervisord发送SIGCHILD信号时等待的时间
- stopasgroup：如果设置为true，则会使supervisor发送停止信号到整个进程组
- killasgroup：如果设置为true，则在给程序发送SIGKILL信号的时候，会发送到整个进程组，它的子进程也会受到影响。
- user：如果supervisord以root运行，则会使用这个设置用户启动子程序
- redirect_stderr：如果设置为true，进程则会把标准错误输出到supervisord后台的标准输出文件描述符。
- stdout_logfile：把进程的标准输出写入文件中，如果stdout_logfile没有设置或者设置为AUTO，则supervisor会自动选择一个文件位置。
- stdout_logfile_maxbytes：标准输出log文件达到多少后自动进行轮转，单位是KB、MB、GB。如果设置为0则表示不限制日志文件大小
- stdout_logfile_backups：标准输出日志轮转备份的数量，默认是10，如果设置为0，则不备份
- stdout_capture_maxbytes：当进程处于stderr capture mode模式的时候，写入FIFO队列的最大bytes值，单位可以是KB、MB、GB
- stdout_events_enabled：如果设置为true，当进程在写它的stderr到文件描述符的时候，PROCESS_LOG_STDERR事件会被触发
- stderr_logfile：把进程的错误日志输出一个文件中，除非redirect_stderr参数被设置为true
- stderr_logfile_maxbytes：错误log文件达到多少后自动进行轮转，单位是KB、MB、GB。如果设置为0则表示不限制日志文件大小
- stderr_logfile_backups：错误日志轮转备份的数量，默认是10，如果设置为0，则不备份
- stderr_capture_maxbytes：当进程处于stderr capture mode模式的时候，写入FIFO队列的最大bytes值，单位可以是KB、MB、GB
- stderr_events_enabled：如果设置为true，当进程在写它的stderr到文件描述符的时候，PROCESS_LOG_STDERR事件会被触发
- environment：一个k/v对的list列表
- directory：supervisord在生成子进程的时候会切换到该目录
- umask：设置进程的umask
- serverurl：是否允许子进程和内部的HTTP服务通讯，如果设置为AUTO，supervisor会自动的构造一个url
```

####  linux 信号

```
kill -l 
0) EXIST  1) SIGHUP       2) SIGINT       3) SIGQUIT      4) SIGILL
 5) SIGTRAP      6) SIGABRT      7) SIGBUS       8) SIGFPE
 9) SIGKILL     10) SIGUSR1     11) SIGSEGV     12) SIGUSR2
13) SIGPIPE     14) SIGALRM     15) SIGTERM     16) SIGSTKFLT
17) SIGCHLD     18) SIGCONT     19) SIGSTOP     20) SIGTSTP
21) SIGTTIN     22) SIGTTOU     23) SIGURG      24) SIGXCPU
25) SIGXFSZ     26) SIGVTALRM   27) SIGPROF     28) SIGWINCH
29) SIGIO       30) SIGPWR      31) SIGSYS      34) SIGRTMIN
35) SIGRTMIN+1  36) SIGRTMIN+2  37) SIGRTMIN+3  38) SIGRTMIN+4
39) SIGRTMIN+5  40) SIGRTMIN+6  41) SIGRTMIN+7  42) SIGRTMIN+8
43) SIGRTMIN+9  44) SIGRTMIN+10 45) SIGRTMIN+11 46) SIGRTMIN+12
47) SIGRTMIN+13 48) SIGRTMIN+14 49) SIGRTMIN+15 50) SIGRTMAX-14
51) SIGRTMAX-13 52) SIGRTMAX-12 53) SIGRTMAX-11 54) SIGRTMAX-10
55) SIGRTMAX-9  56) SIGRTMAX-8  57) SIGRTMAX-7  58) SIGRTMAX-6
59) SIGRTMAX-5  60) SIGRTMAX-4  61) SIGRTMAX-3  62) SIGRTMAX-2
63) SIGRTMAX-1  64) SIGRTMAX 


说明：
只有第9种信号(SIGKILL)才可以无条件终止进程，SIGKILL信号进程无法捕获，进程也不得不无条件终止，其他信号进程都有权利忽略
HUP 1 终端断线
INT 2 中断（同 Ctrl + C）
QUIT 3 退出（同 Ctrl + \）
TERM 15 终止
KILL 9 强制终止
CONT 18 继续（与STOP相反， fg/bg命令）
STOP 19 暂停（同 Ctrl + Z）
CHLD 17 父进程或init进程进行收拾僵尸进程用到的信号

kill (9)六亲不认的杀掉

term(15)正常的退出进程

在系统中，SIGKILL和SIGSTOP两种信号，进程是无法捕获的，收到后会立即退出

SIGTERM表示终止信号，是kill命令传送的系统默认信号，它与SIGKIIL的区别是，SIGTERM更为友好，进程能捕捉SIGTERM信号，进而根据需要来做一些清理工作

```



```php
task1.php
<?php


pcntl_signal(SIGTERM, function ($signo) {
    echo "stop stop stop ...";
    exit();
}, false);

while (true) {
    pcntl_signal_dispatch();
    //print_r(getmypid() . '-----' . $argv[1]);
    echo PHP_EOL;
    echo date('Y-m-d H:i:s', time()) . PHP_EOL;
    sleep(2);
}

?>
```