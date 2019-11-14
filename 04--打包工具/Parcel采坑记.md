# Parcel 采坑记

**parcel** 极速零配置Web应用打包工具

安装和使用方法请看官网

[官网]([https://parceljs.org](https://parceljs.org/))

------------

parcel 的确是个好东西，用起来也是相当的方便，启动项目就一句命令，配置相对 **webpack** 来说的确是少很多，但不代表是真的什么都不用配置。 



### 坑一：`es6/es7` 语法转换

起初我以为`parcel` 是不用配置 `babel` 的于是就很愉快的用起 `async/await` ，然而当我一顿操作猛如虎，一跑项目就翻车

![01](E:\myself\02--笔记\04--打包工具\img\01.png)

下面是我的测试代码

```javascript
{
    function fn1 () {
        return new Promise((resolve) => {
            setTimeout(() => {
                resolve(console.log(2000))
            }, 2000)
        })
    }

    async function init () {
        await fn1()
    }
    init()
}
```

不过这里配置起来还是挺简单的，对`babel` 有所了解的应该很快就解决了。这里我们只需要安装相应的编译插件就可以。

* **babel-polyfill**

  * github: https://github.com/babel/babel/tree/master/packages/babel-polyfill

  * 通过以下两个插件实现语法的转换：
    * `core-js/shim` 提供 `ES6/7/8` 标准方法实现
    * `regenerate-runtime` 提供 `async` 语法编译后的运行环境

* **babel-plugin-transfrom-runtime**

  * github: https://github.com/babel/babel/blob/master/packages/babel-plugin-transform-runtime
  * 使用新语法的时候，比较推荐这个插件，需要注意一点的是，安装时，需要和`babel-runtime` 作为依赖一起安装

* **babel-plugin-external-helpers**

  * github：https://github.com/babel/babel/blob/master/packages/babel-plugin-external-helpers/
  * 具体不知道怎么介绍

* 打包编译，这里还是使用上面的测试代码

  * 安装插件

    ```javascript
    npm install --save babel-runtime
    npm install --save-dev babel-plugin-transform-runtime babel-plugin-external-helpers
    ```

  * 测试代码

    ```javascript
    {
        function fn1 () {
            return new Promise((resolve) => {
                setTimeout(() => {
                    resolve(console.log('Parcel 打包成功'))
                }, 2000)
            })
        }
    
        async function init () {
            await fn1()
        }
        init()
    }
    ```

    

  * 配置 `.babelrc`

    ```javascript
    {
      "plugins": [
          "transform-runtime", 
          "babel-plugin-transform-regenerator", 
          "babel-plugin-transform-es2015-modules-commonjs"
      ]
    }
    ```

    

再次打包编译，编译`async/await` 应该没什么问题了

![02](E:\myself\02--笔记\04--打包工具\img\02.png)

