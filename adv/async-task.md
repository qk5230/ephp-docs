# 使用后台任务

服务启动时，监听task事件

```php
// Create production server
$http = \ePHP\Core\Server::init()->createServer($config);

// Register a task Listener for handle Async Task
// Backend Task
$http->server->on('task', function (\swoole_server $serv, $task_id, $from_id, $data)
{
    var_dump("receive task-data=", $data);
    sleep(5);

    // return to the worker
    return $data;
});

$http->start();
```

在控制器中，推到异步任务到任务队列

```php
$this->server->task(['jobName' => 'log', 'data' => '...'], -1, function (\swoole_server $serv, $task_id, $data)
{
    var_dump("异步执行完成，data=", $data);
});
```



