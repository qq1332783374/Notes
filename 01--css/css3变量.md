# CSS3 变量

假设在没有 `less` 和 `scss` 这些预编译器的时候，有这么个场景：

> 你需要构建一个比较大型的前端项目，设计的样式可能会达到上千行。在这个过程涉及到大量的通用颜色、字体大小的值等等。假设没有这个变量之前，可能我们需要一个一个手打上去。假如某一天甲方爸爸，要求整体的字体颜色换一下之类的需求，而你有不得不满足。这时前端这时可能会想要是有个统一的管理的就好了。



# 什么是CSS变量

`css` 变量当前有两种形式：

* 变量，就是拥有合法标识符合合法的值。可以重复使用。`css` 中可以使用 `var()` 函数使用变量。例如：`var(--color)` 会返回 `--color` 所对应的值
* 自定义属性。这些属性使用`--name` 的特殊格式作为名字。例如`--example-variable: 20px;`即使一个css声明语句。意思是将`20px`赋值给`--example-varibale`变量。

注意：**自定义属性和常规属性一样，作用在当前的层级，若没有定义，则从其父元素继承其值。**



# 使用

假设场景，实际中应该也不少

```css
.one {
  color: white;
  background-color: brown;
  width: 50px;
  height: 50px;
  display: inline-block;
}

.two {
  color: white;
  background-color: black;
  width: 150px;
  height: 70px;
  display: inline-block;
}
.three {
  color: white;
  background-color: brown;
  width: 75px;
}
.four {
  color: white;
  background-color: brown;
  width: 100px;
}

.five {
  background-color: brown;
}
```

可以看到，在上面的样式中存在着很多的相同的颜色和值，例如: 背景颜色 `brown` 。在这里代码量比较少维护起来没什么压力，假如是刚开始说的那样上千行那种，改起来就可能不是很爽了。下面我们来使用css的变量来改写一下上面的代码:

```css
:root {
    --main-bg-color: brown;
}

.one {
  color: white;
  background-color: var(--main-bg-color);
  width: 50px;
  height: 50px;
  display: inline-block;
}

.two {
  color: white;
  background-color: black;
  width: 150px;
  height: 70px;
  display: inline-block;
}
.three {
  color: white;
  background-color: var(--main-bg-color);
  width: 75px;
}
.four {
  color: white;
  background-color: var(--main-bg-color);
  width: 100px;
}

.five {
  background-color: var(--main-bg-color);
}
```

在这里虽然代码量还是一样的，但是这里多了个统一的变量值。如果需要改背景颜色的话只需改`:root` 下面的 `--mian-bg-color` 的颜色就行了。这样子维护起来会更方便、更统一。



# 使用的注意事项

* 命名： 不能包含`$`，`[`，`^`，`(`，`%`等字符，普通字符局限在只要是“数字`[0-9]`”“字母`[a-zA-Z]`”“下划线`_`”和“短横线`-`”这些组合，但是可以是中文，日文或者韩文

  ```css
  :root {
      --红色: red;
  }
  body {
      background-color: var(--红色);
  }
  ```

  

* 声明：变量的定义和使用只能在声明块`{}`里面，例如：下面是无效的

  ```css
  --main-bg-color: #fff;
  body {
      background-color: var(--main-bg-color);
  }
  ```

  

*  权重：先来看个例子

  ```css
  :root { 
      --color: purple;
  }
  div {
      --color: green;
  }
  #alert { 
      --color: red; 
  }
  * {
      color: var(--color); 
  }
  
  <p>
  	我的紫色继承于根元素
  </p>
  <div>
  	我的绿色来自直接设置
  </div>
  <div id='alert'>
  	ID选择器权重更高，因此阿拉是红色！
      <p>
          我也是红色，继承了爸爸的基因
      </p>
  </div>
  ```

  

  * 变量也是跟着CSS选择器走的，如果变量所在的选择器和使用变量的元素没有交集，是没有效果的。例如`#alert`定义的变量，只有`id`为`alert`的元素才能享有。如果你想变量全局使用，则你可以设置在`:root`选择器上；

  * 当存在多个同样名称的变量时候，变量的覆盖规则由CSS选择器的权重决定的，但并无`!important`这种用法，因为没有必要，`!important`设计初衷是干掉JS的`style`设置，但对于变量的定义则没有这样的需求。

* 完整语法：`var(<自定义属性名>, [默认值])`

  ```css
  :root {
      --main-bg-color: red;
  }
  body {
      background-color: var(--main-bg-color, blue);
  }
  ```

  此时的被背景颜色为 `blue`

* 不合法的缺省值

  ```css
  body {
      --color: 20px;
      background-color: var(--color, #00ffff);
  }
  ```

  此时 `body` 的颜色会是 `transparent` ， 所以说使用变量的地方是什么就对应什么。

* 数值的计算： 可以使用css3中的 `calc()` 进行运算

  ```css
  body {
      --size: 20;
      font-size: calc(var(--size) * 2px);
  }
  ```

  此时的字体大小会变成 `40px`

* 属性可传递

  ```css
  body {
      --red: red;
      --main-bg-color: var(--red);
  }
  ```

  对于复杂布局，CSS变量的这种相互传递和直接引用特性可以简化我们的代码和实现成本，尤其和动态布局在一起的时候，无论是CSS的响应式后者是JS驱动的布局变化。来体验一下:

  ```css
  .box {
      --columns: 4;
      --margins: calc(24px / var(--columns));
      --space: calc(4px * var(--columns));
      --fontSize: calc(20px - 4 / var(--columns));
  }
  @media screen and (max-width: 1200px) {
      .box {
          --columns: 3;
      }
  }
  @media screen and (max-width: 900px) {
      .box {
          --columns: 2;
      }
  }
  @media screen and (max-width: 600px) {
      .box {
          --columns: 1;
      }
  }
  ```

  

# 总结

这里粗略介绍了一下 css 变量的用法，在没有`Less/Scss` 的时候原生变量还是比较好的，缺点就是不是全部的浏览器都支持原生的变量。