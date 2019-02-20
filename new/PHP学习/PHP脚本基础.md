title: PHP脚本基础
date: 2018-07-11 13:04:03
tags:
---

## PHP脚本基础

PHP作为一种解释型脚本语言，在处理一些简单任务时非常便利。sublime下写好处理流程，终端里直接执行就可以看到结果。

比如以下一些简单的任务模块

### 读文件

最常用命令：

`fopen(filename, mode)`

mode 为r（读）、w（写）等

fopen 要和 fclose 一同使用

打开文件后进行读取，分三种：

* fread() 直接读
* fgets() 按行读
* fgetc() 按字符读

另外，feof() 函数检查是否到达文件尾，用于遍历。

例子：

```php
   $cities = fopen("result1.txt", "r") or die("Unable to open");

   $location = array();

   while (!feof($cities)) {

     $location[] = fgets($cities);

   }

   array_pop($location);

   fclose($cities);

```

这里有一个坑，按行读取的字符串存在数组中，每个字符串都包含一个换行符，需要去掉。用替换的方式将 '\n' 换成 ''：

  `$location=str_replace("\n","",$location);`

### 使用curl

curl（Client URL Library）简洁好用，在命令行里可以和服务器各种交互，怎样使用PHP调用curl呢？

```php
<?php 
    // create curl resource 
   $ch = curl_init(); 

   // set url 
   curl_setopt($ch, CURLOPT_URL, "baidu.com"); 

   //return the transfer as a string 
   curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1); 

   // $output contains the output string 
   $output = curl_exec($ch); 

    //echo output
    echo $output;

   // close curl resource to free up system resources 
   curl_close($ch);      
?>
```

### 引用其他文件

很多情况下，需要调用其他PHP文件里的类和函数，有两种方式引用：require 和 include。唯一的区别是，引用出错时require会报error，include只会报warning。

`require_once 'file';`

文件可以是当前路径下的文件名，可以是绝对路径，可以是相对路径。相对路径的写法：

`include_once __FILE__.'../../dir/file';`

