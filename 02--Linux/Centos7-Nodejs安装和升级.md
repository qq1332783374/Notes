# Centos 7 安装和升级

**tips: 服务器一般默认是root，如果权限不足请加 `sudo`**



## 1. Node.js 安装

### 1.1 使用EPEL安装

```
yum info epel-release
```

如果有输出有关epel-release的已安装信息，则说明已经安装，如果提示没有安装或可安装，则安装

```
yum install epel-release
```

安装完后，就可以使用yum命令安装nodejs了，安装的一般会是6.x的版本，并且会将npm(3.x)作为依赖包一起安装

```
yum install nodejs
```

安装完成后，验证是否正确的安装，`node -v`，如果输出如下版本信息，说明成功安装

```
v6.13.3
```

这个版本比较低，可能很多的插件已经不支持了，那么就需要升级了。



## 2. 升级Nodejs

### 2.1 安装 `n`

`n` 是Nodejs管理工具，GitHub ：https://github.com/tj/n

```
npm i -g n
```



### 2.2 安装Nodejs

* 安装最新版本

```
n latest
```

* 安装指定版本

```
n 12.12.0
```

### 2.3 切换Nodejs版本

```
n
```

选择已安装版本

```
 ο  node/8.11.3
    node/10.4.1
```

查看当前`node -v`  ， 如果输出是刚选择的版本号，就代表成功。但问题来了，切换了之后还是原来的版本。



## 3. 解决 `n` 切换失效的办法



### 3.1 查看`node`安装路径

```
which node
# 输出的信息
/usr/local/bin/node  # 以实际地址为准
```



### 3.2 修改配置文件

```
vim ~/.bash_profile
```



### 3.3 将一下的代码加入到末尾

```
export N_PREFIX=/usr/local #node实际安装位置
export PATH=$N_PREFIX/bin:$PATH
```



### 3.4 保存

```
1. 先按 Esc
2. :wq
```



### 3.5 执行 `source` ，是配置生效

```
source ~/.bash_profile
```



### 3.6 再次查看 `node -v` 版本