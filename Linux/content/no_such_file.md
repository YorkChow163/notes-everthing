#  windows下生成的sh文件不能执行

windows下生成的sh文件，到linux下面执行出现no such file 


* vim打开文件

```
vim file.sh
```

* 输入``：``，在命令模式输入``set ff``,输入回车,查看文件格式，如果结果是dos格式的，则可使用后续的处理方法
* 输入``：``，在命令模式输入``set ff=unix``，输入回车,设置文件格式
* 输入``：``，在命令模式输入``wq``,保存退出；

