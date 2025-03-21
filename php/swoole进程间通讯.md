1、父子进程通讯
```php
<?php
 
$ppid = getmypid(); // 父进程id
$work_arr = []; // 子进程数组
 
for ($i = 0; $i < 2; $i++) {
    $process = new \Swoole\Process(function (\Swoole\Process $process) use ($ppid) {
        // 子进程读取父进程发送的数据
        echo $process->read() . PHP_EOL;
 
        // 子进程向父进程发送数据
        $process->write("ppid[{$process->pid}] send to pid[{$ppid}]: hello parent process");
    });
    $process->start();
 
    $work_arr[$process->pid] = $process;
}
 
 
foreach ($work_arr as $pid => $work) {
    // 父进程向子进程发送数据
    $work->write("ppid[{$ppid}] send to pid[{$pid}]: hello child process");
    
    // 父进程读取子进程发送的数据
    echo $work->read(), PHP_EOL;
}
 
// 回收结束运行的子进程
\Swoole\Process::wait(true);
 
 
// 执行结果
/*
ppid[26207] send to pid[26208]: hello child process
ppid[26208] send to pid[26207]: hello parent process
ppid[26207] send to pid[26209]: hello child process
ppid[26209] send to pid[26207]: hello parent process
*/
```

2、子进程间通讯
```php
<?php
 
$process1 = new Swoole\Process(function (\Swoole\Process $process) {
    echo "process1 pid {$process->pid} read process2 data: {$process->read()}\n";
});
$process1->start();
 
$process2 = new \Swoole\Process(function (\Swoole\Process $process) use ($process1) {
    echo "process2 pid {$process->pid} send process2 data: hello world\n";
    $process1->write("hello world");
});
$process2->start();
 
\Swoole\Process::wait(true);
 
// 执行结果
/*
process2 pid 29961 send process2 data: hello world
process1 pid 29960 read process2 data: hello world
*/
```

3、使用队列进行父子进程通讯
```php
<?php
 
for ($i = 0; $i < 4; $i++) {
    $process = new \Swoole\Process(function (\Swoole\Process $process) {
        while ($msg = $process->pop()) {
            echo $process->pid . ": " . $msg . PHP_EOL;
            sleep(1);
        }
    });
    $process->useQueue(1, 1);
    //$process->useQueue(1, 2);
    $process->start();
}
 
$message = ["hello", "world", "swoole", "php"];
foreach ($message as $msg) {
    $process->push($msg);
}
 
\Swoole\Process::wait(true);
 
// $process->useQueue(1, 1);执行结果
/*
1252: hello
1252: world
1252: swoole
1252: php
*/
 
// $process->useQueue(1, 2);执行结果
/*
1631: hello
1632: world
1633: swoole
1634: php
*/
```

4、使用信号进行父子进程通讯
```php
<?php
 
$ppid = getmypid();
 
$work_arr = [];
 
for ($i = 1; $i < 4; $i++) {
    $process = new \Swoole\Process(function (\Swoole\Process $process) use ($ppid, $i) {
        sleep($i * 3);
        echo "PID:{$process->pid} kill -USR2 to master {$ppid}\n";
        \Swoole\Process::kill($ppid, SIGUSR2);
    });
    $process->start();
    $work_arr[$process->pid] = $process;
}
 
\Swoole\Process::signal(SIGUSR2, function () use ($work_arr) {
    foreach ($work_arr as $pid => $work) {
        echo "kill PID:{$pid}\n";
        \Swoole\Process::kill($pid, SIGKILL);
    }
});
 
\Swoole\Process::signal(SIGCHLD, function () {
    while ($ret = \Swoole\Process::wait(false)) {
        echo "PID={$ret['pid']}\n";
    }
});
 
swoole_timer_tick(2000,function (){});
 
 
// 执行结果
/*
PHP Deprecated:  Swoole\Event::rshutdown(): Event::wait() in shutdown function is deprecated in Unknown on line 0
Deprecated: Swoole\Event::rshutdown(): Event::wait() in shutdown function is deprecated in Unknown on line 0
PID:4928 kill -USR2 to master 4927
kill PID:4928
kill PID:4929
kill PID:4930
PID=4928
PID=4929
PID=4930
*/
```

参考：  
[Swoole4 文档](https://wiki.swoole.com/#/process/process)  
https://github.com/zhangyue0503/swoole 
