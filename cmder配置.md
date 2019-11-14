Cmder 安装和配置

## 介绍

Windows系统下的命令行神器，自带`git` ，日常`Unix` 命令无压力，现在起Windows也可以`rm -rf MySql` 。软件的界面支持自定义，这点就挺好的。话不多说，用Windows的小伙伴赶紧来体验一下`Cmder`的强大之处吧！



## 安装

直接官网点击Download下载就可以了，个人建议下载`Full`版，因为 `minn` 版没有集成 `git`

* ​	[官网传送门](https://cmder.net/)

下载完成后找到你喜欢的目录解压，我这边的目录是 `E:\software\cmder`

![cmder配置-3](E:\myself\02--笔记\image\cmder配置-3.png)

现在就可以双击运行 `Cmder.exe` 文件查看终端啦。不过这里有一点就是，下次需要运行的话还是要到解压好的目录运行终端，这样就显得不是很方便。



## 配置

#### 全局环境变量配置

全局环境变量配置主要为了以后配置方便

![cmder配置-4](E:\myself\02--笔记\image\cmder配置-4.png)

![cmder配置-5](E:\myself\02--笔记\image\cmder配置-5.png)



这里确定之后，然后就到`Path` 那里新建一个配置，值为 `%CMDER_ROOT%`

![cmder配置-6](E:\myself\02--笔记\image\cmder配置-6.png)



以上配置完成之后，`win + R` 键输入 `cmder` 就可以打开 `cmder终端`了。这样子好像方便了一点点，但还是不够方便。接下来我们来把它添加到右键菜单

#### 右键菜单配置

以管理员身份运行 `cmd`，然后切换到`cmder`解压目录下，运行以下命令

```javascript
Cmder.exe /REGISTER ALL
```

![cmder配置-7](E:\myself\02--笔记\image\cmder配置-7.png)



完成之后，在桌面或者任意文件下打开鼠标右键菜单，出现`Cmder Here`就表示已经安装成功。

![cmder配置-8](E:\myself\02--笔记\image\cmder配置-8.png)



#### 解决中文乱码问题

打开`cmder`按 `win + alt + p` 键或者在右下角打开点击打开`Settings` 界面

在 `Settings/Startup/Environment`，添加一下命令：

```javascript
set LANG=zh_CN.UTF-8
set LC_ALL=zh_CN.utf8
chcp utf-8
```



#### 提示符号更换

`Cmder ` 中的提示符符号默认为 `λ`，感觉不是很习惯，而且听说这个符号`λ` 还会出现 bug，所以还是换一下比较好。

这里一共要修改两个文件

* `Cmder(解压根目录)\vendor\clink.lua` ，没什么意外应该是 51行，将 `λ` 改为 `$`

  ![cmder配置-9](E:\myself\02--笔记\image\cmder配置-9.png)

* `Cmder(解压根目录)\vendor\git-for-windows\etc\profile.d\git-prompt.sh` ，36行

  ![cmder配置-10](E:\myself\02--笔记\image\cmder配置-10.png)

* 改完之后`Cmder`记得要重启一下才能看到。



#### 配置到 **VsCode**

* 快捷键 `ctrl + shift + p` 打开命令窗口，输入 `User Settings`  然后回车进入设置界面

  ![cmder配置-1](E:\myself\02--笔记\image\cmder配置-1.png)

* 打开设置文件，添加以下配置，这里需要转义一下

  ```json
  "terminal.integrated.shell.windows": "cmd.exe",
      "terminal.integrated.shellArgs.windows": ["/k", "%CMDER_ROOT%\\vendor\\init.bat"]
  ```

* 配置成功

  ![cmder配置-11](E:\myself\02--笔记\image\cmder配置-11.png)



#### 配置到 **Webstorm**

* 设置项 `File > Settings > Tools > Terminal > Shell Path`

* **Shell Path** 的值为 `"cmd" /k ""%CMDER_ROOT%\vendor\init.bat""`

  ![cmder配置-12](E:\myself\02--笔记\image\cmder配置-12.png)

  

  ![cmder配置-13](E:\myself\02--笔记\image\cmder配置-13.png)



## 常用快捷键

* **Tab**：用于补全
* **Ctrl+T**：打开新窗口
* **Ctrl+W**：关闭当前窗口
* **Alt+F4**：关闭所有窗口
* **Ctrl+1**：窗口切换，以此类推
* **Alt + enter**：切换到全屏状态
* **鼠标右键**：粘贴文本



## 总结

配置完成，记一下快捷键，然后就可以愉快的敲代码啦！

如有不足之处，还望大家及时指出！

谢谢大家!



更多配置请参考官方 [文档](https://github.com/cmderdev/cmder/wiki/Customization)

