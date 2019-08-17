# 设置jupyter notebook远程访问

## 环境准备

- 云主机(阿里云，腾讯云，或者别的什么免费的)
- 系统: Ubuntu 16.06 (这是我的)
- 已经安装python 
- 已经安装了jupyter

## 配置过程

 **1. 创建jupyter文件夹和配置文件[jupyter_notebook_config.py]** 

```
jupyter notebook --generate-config
```

![1.png](https://i.loli.net/2019/08/16/sB9doSOTRvAiWkZ.png)

 **2. 设置jupyter远程访问的密码** 

```
jupyter notebook password
```

![3.png](https://i.loli.net/2019/08/16/aCqmvFDSjr7cfwY.png)

 **3. 编辑配置文件** 

```
vi /root/.jupyter/jupyter_notebook_config.py
```

![4.png](https://i.loli.net/2019/08/16/rgdpBfZaDyTqHiJ.png)

```
找到对应项并进行修改(前面有#号的记得去掉)
1, c.NotebookApp.open_browser = False
[↑禁止自动打开浏览器]
2, c.NotebookApp.allow_remote_access  = True
[↑允许外部访问]
3, c.NotebookApp.ip = '*'
[↑localhost表示仅仅运行本地的访问，' * ' 表示所有的ip都可以访问]
4, c.NotebookApp.port = 8800
[↑设置服务监听的端口(随便设，没被占用就行)]
5, 保存退出
```

 **4. 防火墙允许开放该端口** 

```
sudo firewall-cmd --zone=public --add-port=8800/tcp --permanent
接着输入密码
重启防火墙:
sudo systemctl restart firewalld
```

 **5. 启动 jupyter notebook** 

 *但是我们发现把 ssh关掉后，jupyter服务就关闭了，这并不符合我们的要求，我们需要的是让它一直保持开启状态。*

 **6. nohub方法让 jupyter 服务一直保持运行** 

- 安装 nohub(已经有的可以忽略这一步)

```
sudo apt-get install screen
```

- 创建一个screen窗口

```
screen -S name (name是你自己起的窗口名字，方便一个该窗口用途)
然后在该窗口启动 jupyter:
jupyter notebook
最后按CTRL + a ，再按一下 d 键，这时退出ssh登录也不会影响screen程序的执行
```

*最后就是登录看看效果如何(ip为本机ip)*

![5.png](https://i.loli.net/2019/08/16/Rm1kdqhEsJaHBZr.png)
