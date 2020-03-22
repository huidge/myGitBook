# 上手 React 框架开发

React（也被称为 React.js 或者 ReactJS）是一个用于构建用户界面的 JavaScript 库。起源于 Facebook 内部项目，最初用来架设 Instagram 的网站，并于 2013 年 5 月开源。React 性能较高，并且它的声明式、组件化特性让编写代码变得简单，随着 React 社区的发展，越来越多的人投入 React 的学习和开发，使得 React 不仅可以用来开发 Web 应用，还能开发桌面端应用，TV应用，VR应用，IoT应用等，因此 React 还具有一次学习，随处编写的特性。本教程将带你快速入门 React 开发，通过 20-30 分钟的学习，你不仅可以了解 React 的基础概念，而且能开发出一个待办事项小应用，还在想什么了？马上学起来吧！本文所有代码已放在 [GitHub 仓库[1]](https://github.com/pftom/react-quickstart-tutorial)中。

>此教程属于 [React 前端工程师学习路线[2]](https://github.com/tuture-dev/react-roadmap)的一部分，点击可查看全部内容。

# Hello, World
## 我们将构建什么？

在这篇教程中，我们将展示给你如何使用 React 构建一个待办事项应用，下面最终项目的展示成果：

![成果展示](https://mmbiz.qpic.cn/mmbiz_gif/gEz4yUa4ueSgJn0KJrxDH6ibQzoicuTCTtICSFSJlS7K2vKj0XPeJHsshDpydqeuGtphLcia0GDFkmWVhG7GH54gw/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)

你也可以在这里看到我们最后构建的结果：[最终结果[3]](https://codesandbox.io/s/currying-grass-rglst)。如果你现在对代码还不是很理解，或者你还不熟悉代码语法，别担心！这篇教程的目标就是帮助你理解 React 和它的语法。

我们推荐你在继续阅读这篇教程之前先熟悉一下这个待办事项，你甚至可以尝试添加几个待办事项！你可能注意到当你添加了2个待办事项之后，会出现不同的颜色；这就是 React 中条件渲染的魅力。

当你熟悉了这个待办事项之后你就可以关闭它了。在这篇教程的学习中，我们将从一个 Hello World 代码模板开始，然后带领你初始化开发环境，这样你就可以开始构建这个待办事项了。

## 你将学到什么？
你将学习所有 React 的基础概念，其中又分为三个部分：

* 编写组件相关：包括 JSX 语法、Component、Props
* 组件的交互：包括 State 和生命周期
* 组件的渲染：包括列表和 Key、条件渲染
* 和 DOM & HTML 相关：包括事件处理、表单。

## 前提条件

我们假设你熟系 HTML 和 JavaScript，但即使你是从其他编程语言转过来的，你也能看懂这篇教程。我们还假设你对一些编程语言的概念比较熟悉，比如函数、对象、数组，如果对类了解就更好了。

如果你需要复习 JavaScript，我们推荐你阅读[这篇指南[4]](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/A_re-introduction_to_JavaScript)。你可能注意到了我们使用了一些 ES6 的特性 -- 一个最近的 JavaScript 版本。在这篇教程，我们会使用 [arrow functions[5]](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)，[classes[6]](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes)，和 [const[7]](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/const)。你可以使用 [Babel REPL[8]](https://www.babeljs.cn/repl#?babili=false&browsers=&build=&builtIns=false&spec=false&loose=false&code_lz=MYewdgzgLgBApgGzgWzmWBeGAeAFgRgD4AJRBEAGhgHcQAnBAEwEJsB6AwgbgChRJY_KAEMAlmDh0YWRiGABXVOgB0AczhQAokiVQAQgE8AkowAUAcjogQUcwEpeAJTjDgUACIB5ALLK6aRklTRBQ0KCohMQk6Bx4gA&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=react&prettier=false&targets=&version=7.7.3) 来检查 ES6 代码编译之后的结果。

## 环境准备

首先准备 Node 开发环境，访问 [Node 官方网站[9]](https://nodejs.org/zh-cn/)下载并安装。打开终端输入如下命令检测 Node 是否安装成功：

```
node -v # v10.16.0
npm -v # 6.9.0
```

>注意 Windows 用户需要打开 cmd 工具，Mac 和 Linux 是终端。

如果上面的命令有输出且无报错，那么代表 Node 环境安装成功。接下来我们将使用 React 脚手架 -- [Create React App[10]](https://create-react-app.dev/)（简称 CRA）来初始化项目，同时这也是官方推荐初始化 React 项目的最佳方式。

在终端中输入如下命令：
```
npx create-react-app my-todolist 
```

等待命令运行完成，接着输入如下命令开启项目：

```
cd my-todolist && npm start 
```

CRA 会自动开启项目并打开浏览器，你应该可以看到下面的结果：
![https://imgkr.cn-bj.ufileos.com/603bba43-2ffe-4c66-87ad-a33a56c3f38e.png](https://imgkr.cn-bj.ufileos.com/603bba43-2ffe-4c66-87ad-a33a56c3f38e.png)

🎉🎉🎉 恭喜你！成功创建了第一个 React 应用！

现在 CRA 初始化的项目里有很多无关的内容，为了开始接下来的学习，我们还需要做一点清理工作。首先在终端中按 ` ctrl + c `关闭刚刚运行的开发环境，然后在终端中依次输入如下的命令：

```
# 进入 src 目录
cd src

# 如果你在使用 Mac 或者 Linux：
rm -f *

# 或者，你在使用 Windows：
del *

# 然后，创建我们将学习用的 JS 文件
# 如果你在使用 Mac 或者 Linux：
touch index.js

# 或者，你在使用 Windows
type nul > index.js

# 最后，切回到项目目录文件夹下
cd ..

```

此时如果在终端项目目录下运行 ` npm start ` 会报错，因为我们的 ` index.js `还没有内容，我们在终端中使用` ctrl +c `关闭开发服务器，然后使用编辑器打开项目，在刚刚创建的` index.js `文件中加入如下代码：


```
import React from "react";
import ReactDOM from "react-dom";

class App extends React.Component {
  render() {
    return <div>Hello, World</div>;
  }
}

ReactDOM.render(<App />, document.getElementById("root"));

```

我们看到` index.js `里面的代码分为三个部分。

首先是一系列导包，我们导入了` react `包，并命名为 React，导入了` react-dom `包并命名为 ReactDOM。对于包含 React 组件（我们将在之后讲解）的文件都必须在文件开头导入 React。

然后我们定义了一个 React 组件，命名为 App，继承自 React.Component，组件的内容我们将会在后面进行讲解。

接着我们使用 ReactDOM 的 render 方法来渲染刚刚定义的 App 组件，`render`方法接收两个参数，第一个参数为我们的 React 根级组件，第二个参数接收一个 DOM 节点，代表我们将把和 React 应用挂载到这个 DOM 节点下，进而渲染到浏览器中。

>注意

>上面代码的三个部分中，第一部分和第三部分在整篇教程中是不会修改的，同时在编写任意 React 应用，这两个部分都是必须的。后面所有涉及到的代码修改都是关于第二部分代码的修改，或者是在第一部分到第三部分之间插入或删除代码。

保存代码，在终端中使用 npm start 命令开启开发服务器，现在浏览器应该会显示如下内容：
![https://imgkr.cn-bj.ufileos.com/a765c989-b815-4e8a-b5b9-10f020315090.png](https://imgkr.cn-bj.ufileos.com/a765c989-b815-4e8a-b5b9-10f020315090.png)

准备工作已经就绪！

你可能对上面的代码细节还不是很清楚，别担心，我们将马上带你领略 React 的神奇世界！

# JSX 语法
首先我们来看一下 React 引以为傲的特性之一——JSX。它允许我们在 JS 代码中使用 XML 语法来编写用户界面，使得我们可以充分的利用 JS 的强大特性来操作用户界面。

一个 React 组件的 render 方法中 return 的内容就为这个组件所将渲染的内容。比如我们现在的代码：
```
render() {
    return <div>Hello, World</div>;
}
```
这里的` <div>Hello, World</div> `是一段 JSX 代码，它最终会被 [Babel[11]](https://www.babeljs.cn/) 转译成下面这段 JS 代码:
```
React.createElement(
  'div',
  null,
  'Hello, World'
)
```
`React.createElement()`接收三个参数：
- 第一个参数代表 JSX 元素标签。
- 第二个参数代表这个 JSX 元素接收的属性，它是一个对象，这里因为我们的 `div` 没有接收任何属性，所以它是 `null`。
- 第三个参数代表 JSX 元素包裹的内容。
`React.createElement()` 会对参数做一些检查确保你写的代码不会产生 BUG，它最终会创建一个类似下面的对象：
```
{
  type: 'div',
  props: {
    children: 'Hello, World'
  }
};
```
这些对象被称之为 “React Element”。你可以认为它们描述了你想要在屏幕上看到的内容。React 将会接收这些对象，使用它们来构建 DOM，并且对它们进行更新。

>注意

>我们推荐你使用 [“Babel”[12]](https://www.babeljs.cn/repl#?babili=false&browsers=&build=&builtIns=false&spec=false&loose=false&code_lz=MYewdgzgLgBApgGzgWzmWBeGAeAFgRgD4AJRBEAGhgHUQAnBAE2wHoDCBuAKFEll6gBDAJZg4dGFkYhgAV1ToAdAHM4UAKJIFUAEIBPAJKMAFAHI6IEFFMBKbgCU4g4FAAiAeQCyiumkbjjRBQ0KCoBETE6Oy4gA&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=react&prettier=false&targets=&version=7.7.3)  查看 JSX 的编译结果。

App 组件最终返回这段 JSX 代码，所以我们使用 ReactDOM 的 `render` 方法渲染 App 组件，最终显示在屏幕上的就是 `Hello, World` 内容。

## JSX 作为变量使用
因为 JSX 最终会被编译成一个 JS 对象，所以我们可以把它当做一个 JS 对象使用，它享有和一个 JS 对象同等的地位，比如可以将其赋值给一个变量，我们修改上面代码中的 render 方法如下：
```
render() {
  const element = <div>Hello, World</div>;
  return element;
}
```
保存代码，我们发现浏览器中渲染的内容和我们之前类似。

## 在 JSX 中使用变量
我们可以使用大括号 {} 在 JSX 中动态的插入变量值，比如我们修改 render 方法如下：
```
render() {
  const content = "World";
  const element = <div>Hello, {content}</div>;
  return element;
}
```
保存代码，发现浏览器中效果依然不变。

## JSX 中使用 JSX
我们可以在 JSX 中再包含 JSX，这样我们编写任意层次的 HTML 结构：
```
render() {
    const element = <li>Hello, World</li>
    return (
      <div>
        <ul>
          {element}
        </ul>
      </div>
    )
  }
```
## JSX 中添加节点属性
我们可以像在 HTML 中一样，给元素标签加上属性，只不过我们需要遵守[驼峰式命名法则[13]](https://baike.baidu.com/item/%E9%AA%86%E9%A9%BC%E5%91%BD%E5%90%8D%E6%B3%95)，比如在 HTML 上的属性 `data-index` 在 JSX 节点上要写成 `dataIndex`。

```
const element = <div dataIndex="0">Hello, World</div>;
```

>注意

>在 JSX 中所有的属性都要更换成驼峰式命名，比如 `onclick` 要改成 `onClick`，唯一比较特殊的就是 `class`，因为在 JS 中 `class`` 是保留字，我们要把 class` 改成 `className` 。

```
const element = <div className="app">Hello, World</div>;
```

## 实战
我们使用这一节讲解的 JSX 知识，来继续完成我们的待办事项应用。

在编辑器中打开 `src/index.js `，对我们的第二段代码做如下改变：
```
// ...

class App extends React.Component {
  render() {
    //return <div>Hello, World</div>;
    const todoList = ["图雀", "图雀写作工具", "图雀社区", "图雀文档"];
    return (
      <ul>
        <li>Hello, {todoList[0]}</li>
        <li>Hello, {todoList[1]}</li>
        <li>Hello, {todoList[2]}</li>
        <li>Hello, {todoList[3]}</li>
      </ul>
    );
  }
}

// ...
```
可以看到，我们使用 `const` 定义了一个 `todoList`数组常量，并且在 JSX 中使用 `{}` 进行动态插值，插入了数组的四个元素。

最后保存代码，浏览器中的效果应该是这样的：
![https://imgkr.cn-bj.ufileos.com/2b149455-ccf4-4efc-8d26-7c86d3d72d87.png](https://imgkr.cn-bj.ufileos.com/2b149455-ccf4-4efc-8d26-7c86d3d72d87.png)

>提示

>无需关闭刚才使用 `npm start` 开启的开发服务器，修改代码后，浏览器中的内容将会自动刷新！

你可能注意到了我们手动获取了数组的四个值，然后逐一的用 `{}`语法插入到 `JSX` 中并最终渲染，这样做还比较原始，我们将在后面列表和 Key小节中简化这种写法。

在这一小节中，我们了解了 JSX 的概念，并且实践了相关的知识。我们还提出了组件的概念，但是并没有深入讲解它，在下一小节中我们将详细地讲解组件的知识。

# Component
# Props
# State 和生命周期
# 列表和 Key
# 条件渲染
# 事件处理
# 表单