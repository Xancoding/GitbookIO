# SCP

* [SCP | AcWing Linux 基础课](https://www.acwing.com/file\_system/file/content/whole/index/content/2898266/)

***

## SCP是什么？

> **SCP是一个命令行程序，可让您在计算机之间安全地复制文件和目录**

## SCP 传文件

* 复制多个文件：`scp source1 source2 LOCAL_PATH SERVER:SERVER_PATH`
* 复制文件夹 ：
  * `scp -r ~/tmp SERVER:/home/acs/`：将本地家目录中的`tmp`文件夹复制到服务器中的`/home/acs/`目录下
  * `scp -r SERVER:homework .`：将服务器中的`~/homework/`文件夹复制到本地的当前路径下
