基类：
```php
<?php
 
 
namespace Sw\Io;
 
 
class Mutil
{
    // 闭包函数
    public $onReceive = null;
    public $onConnect = null;
    public $onClose = null;
 
    // 所有连接
    protected $sockets = [];
 
    // 启动服务时，stream_socket_server的连接
    public $socket = null;
 
    public function __construct($socket_address)
    {
        $this->socket = stream_socket_server($socket_address);
        stream_set_blocking($this->socket, 0);
        $linkId = (int)$this->socket;
        $this->sockets[$linkId] = $this->socket;
        echo "mutil server is starting ... \n";
        echo "this->socket: \n";
        var_dump($this->socket);
        echo "this->sockets[]: \n";
        var_dump($this->sockets);
    }
 
    public function accept()
    {
        echo "coming accept ...\n";
        while (true) {
            echo "while ... \n";
            $read = $this->sockets;
            $w = $r = null;
            if (empty($read)) {
                die("read: $read \n");
            }
 
            stream_select($read, $w, $e, 1);
            echo "read: ".json_encode($read)."\n";
            foreach ($read as $socket) {
                if ($socket === $this->socket) {
                    echo "create socket: $socket \n";
                    $this->createSocket();
                } else {
                    echo "sendMsg socket: $socket \n";
                    $this->sendMsg($socket);
                }
            }
        }
    }
 
    public function createSocket()
    {
        if (!is_resource($this->socket)) {
            die("this->socket: {$this->socket} \n");
        }
        $client = stream_socket_accept($this->socket);
        if (is_callable($this->onConnect)) {
            ($this->onConnect)($this, $client);
        }
 
        $clientId = (int)$client;
        $this->sockets[$clientId] = $client;
        echo "sockets[]: \n";
        var_dump($this->sockets);
    }
 
    public function sendMsg($client)
    {
        $data = fread($client, 65535);
 
        if ($data === false) {
            $this->free($client);
        } else if (strlen($data) === 0) {
            echo "client connect is erroor \n";
        } else {
            var_dump($data);
            if (is_callable($this->onReceive)) {
                ($this->onReceive)($this, $client, $data);
            }
            $this->free($client);
        }
    }
 
    public function send($conn, $data)
    {
        fwrite($conn, $data);
    }
 
    public function sendHttp($conn, $data)
    {
        $len = strlen($data);
        $response = "HTTP/1.1 200 ok\r\nContent-type:text/html;charset=utf-8\r\nConnection:keep-alive\r\nContent-length:{$len}\r\n\r\n{$data}";
        fwrite($conn, $data);
    }
 
    public function debug($data, $flag = false)
    {
        if ($flag) {
            var_dump($data);
        } else {
            echo ">>> $data \n";
        }
    }
 
    public function start()
    {
        $this->accept();
    }
 
    public function free($conn)
    {
        $id = (int)$conn;
        unset($this->sockets[$id]);
        fclose($conn);
    }
}
```

服务端代码：
```php
<?php
// 服务端代码
include 'Mutil.php';
$server = new \Sw\Io\Mutil('tcp://0.0.0.0:9502');
$server->onConnect = function ($socket,$client){
    echo "client connect ... \n";
};
 
$server->onReceive = function ($socket,$client,$data){
    $socket->send($client,"hello world ... \n");
};
 
echo "server is start ... \n";
$server->start();
```

客户端代码：
```php
<?php
// 客户端代码
$client = stream_socket_client('tcp://0.0.0.0:9502');
if (!$client) {
    die("connect is failed");
}
 
fwrite($client, "client recv data ... \n");
$data = fread($client, 65535);
 
if (!$data) {
    die("client recv is failed");
}
 
var_dump($data);
 
fclose($client);
```

参考：https://segmentfault.com/q/1010000005038403
