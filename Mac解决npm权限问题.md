# Mac解决权限问题

1、创建一个目录用作全局安装：

```
mkdir ~/.npm-global
```

2、配置npm使用这个新目录：

```
npm config set prefix '~/.npm-global'
```

3、打开或者创建一个“~/.bash_profile”文件并添加下行代码：

```
export PATH=~/.npm-global/bin:$PATH
```

4、返回命令行，更新系统变量：

```
source ~/.bash_profile
```

测试：不用sudo，全局下载安装一个包：

```
npm install -g jshint
```

不使用第2-4步的方法的话，你也可以使用相应的环境变量（比如如果你不想编辑~/.profile）来实现：

```
NPM_CONFIG_PREFIX=~/.npm-global
```

