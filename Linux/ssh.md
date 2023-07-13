# SSH

* [SSH | AcWing Linux 基础课](https://www.acwing.com/file\_system/file/content/whole/index/content/2898263/)
* [Adding a new SSH key to your GitHub account](https://docs.github.com/cn/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)

***

#### SSH是什么？

> **SSH是一种网络协议，用于计算机之间的加密登录**

#### SSH 免密登录

1. `ssh-keygen` ：在本地服务器生成密钥
2. `cd .ssh/`
3. `vim config` ：定义服务器别名

```config
Host server
	HostName 服务器IP地址  
	User     登录用户名
	port     远程主机端口号，默认为 22
```

4. `ssh-copy-id server`：在本地服务器配置免密登录至云服务器
5. 免密登录至`docker 容器`步骤同上
6. 配置完成后，就可以直接使用 `ssh server` 免密登录啦

