# PHP源码的整体框架

SAPI（server API）	php应用接口层	
sapi 目录是对输入和输出层的抽象,是 PHP 供对外服务的规范。 典型SAPI：Cli、Fpm、Embed

Zend	实现PHP解析器（编译+执行）	核心模块，重点看内存管理、垃圾回收、数组实现等模块
ext(extension)	扩展相关	 
main	承上（sapi）启下(zend)	解析SAPI 的请求，并做调用zend前相应模块的初始化操作。 
TSRM	线程安全资源管理器(Thread Safe Resource Manager)	http://www.laruence.com/2008/08/03/201.html
pear	php官司方开源类库	
纯粹的PHP代码写出的函数和类：PHP扩展模块Pecl、Pear以及Perl的区别

win32	这个目录主要包括Windows平台相关的一些实现，同时也包括了Windows下编译PHP相关的脚本。	eg:sokcet的实现在Windows和nginx平台就不太一样
tests	PHP的测试脚本集合，包含PHP各项功能的测试文件	 
build	一些和源码编译相关的文件，比如开始构建之前的buildconf脚本等文件，还有一些检查环境的脚本等。	
