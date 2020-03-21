# **让虚拟DOM和DOM-diff不再成为你的绊脚石**

>[作者：chenhongdong](https://juejin.im/post/5c8e5e4951882545c109ae9c)

# Keep Moving
时至今日，前端对于知识的考量是越来越有水平了，逼格高大上了

各类框架大家已经可以说无论是工作还是日常中都已经或多或少的使用过了

曾经听说很多人被问到过虚拟DOM和DOM-diff算法是如何实现的，有没有研究过？

想必问出此问题的也是高手高手之高高手了，很多人都半开玩笑的说：“面试造航母，工作拧螺丝”

那么，话不多说了，今天就让我们也来一起研究研究这个东东

好饭不怕晚，沉淀下来收收心！我们虽然走的慢，但是却从未停下脚步

# 神奇的虚拟DOM
首先神奇不神奇的我们先不去关注，先来简单说说何为虚拟DOM

虚拟DOM简而言之就是，用JS去按照DOM结构来实现的树形结构对象，你也可以叫做DOM对象

好了，一句话就把这么伟大的东西给解释了，那么不再耽误时间了，赶紧进入主环节吧

当然，这里还有整个项目的地址方便查看

## 实现一下虚拟DOM
在亲自上阵之前，我们让粮草先行，先发个图，来看一下整个目录结构是什么样子的

![目录结构](https://user-gold-cdn.xitu.io/2019/3/20/1699a54e3737e10c?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

这个目录结构是用create-react-app脚手架直接生成的，也是为了方便编译调试
```
// 全局安装
    npm i create-react-app -g
    // 生成项目
    create-react-app dom-diff
    // 进入项目目录
    cd dom-diff
    // 编译
    npm run start
```
现在我们开始正式写吧，从创建虚拟DOM及渲染DOM起步吧

## 创建虚拟DOM
在element.js文件中要实现如何创建虚拟DOM以及将创建出来的虚拟DOM渲染成真实的DOM

首先实现一下如何创建虚拟DOM，看代码：

// element.js
    
    // 虚拟DOM元素的类，构建实例对象，用来描述DOM
    class Element {
        constructor(type, props, children) {
            this.type = type;
            this.props = props;
            this.children = children;
        }
    }
    // 创建虚拟DOM，返回虚拟节点(object)
    function createElement(type, props, children) {
        returnnew Element(type, props, children);
    }
    
    export {
        Element,
        createElement
    }
写好了方法，我们就从index.js文件入手来看看是否成功吧

## 调用createElement方法
在主入口文件里，我们主要做的操作就是来创建一个DOM对象，渲染DOM以及通过diff后去打补丁更新DOM，不啰嗦了，直接看代码：

// index.js
    
    // 首先引入对应的方法来创建虚拟DOM
    import { createElement } from'./element';
    
    let virtualDom = createElement('ul', {class: 'list'}, [
        createElement('li', {class: 'item'}, ['周杰伦']),
        createElement('li', {class: 'item'}, ['林俊杰']),
        createElement('li', {class: 'item'}, ['王力宏'])
    ]);
    
    console.log(virtualDom);
createElement方法也是vue和react用来创建虚拟DOM的方法，我们也叫这个名字，方便记忆。接收三个参数，分别是type，props和children

参数分析

type: 指定元素的标签类型，如'li', 'div', 'a'等

props: 表示指定元素身上的属性，如class, style, 自定义属性等

children: 表示指定元素是否有子节点，参数以数组的形式传入

下面来看一下打印出来的虚拟DOM，如下图

![虚拟DOM](https://user-gold-cdn.xitu.io/2019/3/18/1698eae05b555be0?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

到目前为止，已经轻而易举的实现了创建虚拟DOM。那么，接下来进行下一步，将其渲染为真实的DOM，别犹豫，继续回到element.js文件中

## 渲染虚拟DOM
// element.js
    
    class Element {
        // 省略
    }
    
    functioncreateElement() {
        // 省略
    }
    
    // render方法可以将虚拟DOM转化成真实DOM
    function render(domObj) {
        // 根据type类型来创建对应的元素
        let el = document.createElement(domObj.type);
        
        // 再去遍历props属性对象，然后给创建的元素el设置属性
        for (let key in domObj.props) {
            // 设置属性的方法
            setAttr(el, key, domObj.props[key]);
        }
        
        // 遍历子节点
        // 如果是虚拟DOM，就继续递归渲染
        // 不是就代表是文本节点，直接创建
        domObj.children.forEach(child => {
            child = (child instanceof Element) ? render(child) : document.createTextNode(child);
            // 添加到对应元素内
            el.appendChild(child);
        });
    
        return el;
    }
    
    // 设置属性
    functionsetAttr(node, key, value) {
        switch(key) {
            case'value':
                // node是一个input或者textarea就直接设置其value即可
                if (node.tagName.toLowerCase() === 'input' ||
                    node.tagName.toLowerCase() === 'textarea') {
                    node.value = value;
                } else {
                    node.setAttribute(key, value);
                }
                break;
            case'style':
                // 直接赋值行内样式
                node.style.cssText = value;
                break;
            default:
                node.setAttribute(key, value);
                break;
        }
    }
    
    // 将元素插入到页面内
    function renderDom(el, target) {
        target.appendChild(el);
    }
    
    export {
        Element,
        createElement,
        render,
        setAttr,
        renderDom
    };
既然写完了，那就赶快来看看成果吧

## 调用render方法
再次回到index.js文件中，修改为如下代码

// index.js
    
    // 引入createElement、render和renderDom方法
    import { createElement, render, renderDom } from'./element';
    
    let virtualDom = createElement('ul', {class: 'list'}, [
        createElement('li', {class: 'item'}, ['周杰伦']),
        createElement('li', {class: 'item'}, ['林俊杰']),
        createElement('li', {class: 'item'}, ['王力宏'])
    ]);
    
    console.log(virtualDom);
    
    // +++
    let el = render(virtualDom); // 渲染虚拟DOM得到真实的DOM结构
    console.log(el);
    // 直接将DOM添加到页面内
    renderDom(el, document.getElementById('root'));
    // +++
通过调用render方法转为真实DOM，并调用renderDom方法直接将DOM添加到了页面内

下图为打印后的结果：

![打印结果](https://user-gold-cdn.xitu.io/2019/3/18/16990900e15871dd?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

截止目前，我们已经实现了虚拟DOM并进行了渲染真实DOM到页面中。那么接下来我们就有请DOM-diff隆重登场，来看一下这大有来头的diff算法是如何发光发热的吧！

# DOM-diff闪亮登场
说到DOM-diff那一定要清楚其存在的意义，给定任意两棵树，采用先序深度优先遍历的算法找到最少的转换步骤

DOM-diff比较两个虚拟DOM的区别，也就是在比较两个对象的区别。

作用： 根据两个虚拟对象创建出补丁，描述改变的内容，将这个补丁用来更新DOM

已经了解到DOM-diff是干嘛的了，那就没什么好说的了，继续往下写吧

// diff.js
    
    function diff(oldTree, newTree) {
        // 声明变量patches用来存放补丁的对象
        let patches = {};
        // 第一次比较应该是树的第0个索引
        let index = 0;
        // 递归树 比较后的结果放到补丁里
        walk(oldTree, newTree, index, patches);
    
        return patches;
    }
    
    function walk(oldNode, newNode, index, patches) {
        // 每个元素都有一个补丁
        let current = [];
    
        if (!newNode) { // rule1
            current.push({ type: 'REMOVE', index });
        } elseif (isString(oldNode) && isString(newNode)) {
            // 判断文本是否一致
            if (oldNode !== newNode) {
                current.push({ type: 'TEXT', text: newNode });
            }
    
        } elseif (oldNode.type === newNode.type) {
            // 比较属性是否有更改
            let attr = diffAttr(oldNode.props, newNode.props);
            if (Object.keys(attr).length > 0) {
                current.push({ type: 'ATTR', attr });
            }
            // 如果有子节点，遍历子节点
            diffChildren(oldNode.children, newNode.children, patches);
        } else {    // 说明节点被替换了
            current.push({ type: 'REPLACE', newNode});
        }
        
        // 当前元素确实有补丁存在
        if (current.length) {
            // 将元素和补丁对应起来，放到大补丁包中
            patches[index] = current;
        }
    }
    
    function isString(obj) {
        returntypeof obj === 'string';
    }
    
    function diffAttr(oldAttrs, newAttrs) {
        let patch = {};
        // 判断老的属性中和新的属性的关系
        for (let key in oldAttrs) {
            if (oldAttrs[key] !== newAttrs[key]) {
                patch[key] = newAttrs[key]; // 有可能还是undefined
            }
        }
    
        for (let key in newAttrs) {
            // 老节点没有新节点的属性
            if (!oldAttrs.hasOwnProperty(key)) {
                patch[key] = newAttrs[key];
            }
        }
        return patch;
    }
    
    // 所有都基于一个序号来实现
    let num = 0;
    
    function diffChildren(oldChildren, newChildren, patches) {
        // 比较老的第一个和新的第一个
        oldChildren.forEach((child, index) => {
            walk(child, newChildren[index], ++num, patches);
        });
    }
    
    // 默认导出
    exportdefault diff;
代码虽然又臭又长，但是这些代码就让我们实现了diff算法了，所以大家先不要盲动，不要盲动，且听风吟，让我一一道来

## 比较规则
新的DOM节点不存在{type: 'REMOVE', index}
文本的变化{type: 'TEXT', text: 1}
当节点类型相同时，去看一下属性是否相同，产生一个属性的补丁包{type: 'ATTR', attr: {class: 'list-group'}}
节点类型不相同，直接采用替换模式{type: 'REPLACE', newNode}
根据这些规则，我们再来看一下diff代码中的walk方法这位关键先生

walk方法都做了什么？

每个元素都有一个补丁，所以需要创建一个放当前补丁的数组
如果没有new节点的话，就直接将type为REMOVE的类型放到当前补丁里
if (!newNode) {
            current.push({ type: 'REMOVE', index });
        }
如果新老节点是文本的话，判断一下文本是否一致，再指定类型TEXT并把新节点放到当前补丁
    elseif (isString(oldNode) && isString(newNode)) {
            if (oldNode !== newNode) {
                current.push({ type: 'TEXT', text: newNode });
            }
        }
如果新老节点的类型相同，那么就来比较一下他们的属性props

属性比较

diffAttr

去比较新老Attr是否相同

把newAttr的键值对赋给patch对象上并返回此对象

然后如果有子节点的话就再比较一下子节点的不同，再调一次walk

diffChildren

遍历oldChildren，然后递归调用walk再通过child和newChildren[index]去diff

    elseif (oldNode.type === newNode.type) {
            // 比较属性是否有更改
            let attr = diffAttr(oldNode.props, newNode.props);
            if (Object.keys(attr).length > 0) {
                current.push({ type: 'ATTR', attr });
            }
            
            // 如果有子节点，遍历子节点
            diffChildren(oldNode.children, newNode.children, patches);
        }
上面三个如果都没有发生的话，那就表示节点单纯的被替换了，type为REPLACE，直接用newNode替换即可
else {
            current.push({ type: 'REPLACE', newNode});
        }
当前补丁里确实有值的情况，就将对应的补丁放进大补丁包里
if (current.length > 0) {
            // 将元素和补丁对应起来，放到大补丁包中
            patches[index] = current;
        }
以上就是关于diff算法的分析过程了，没太明白的话没关系，再反复看几遍试试，意外总是不期而遇的

diff已经完事了，那么最后一步就是大家所熟知的打补丁了

补丁要怎么打？那么让久违的patch出来吧

# patch补丁更新
打补丁需要传入两个参数，一个是要打补丁的元素，另一个就是所要打的补丁了，那么直接看代码

import { Element, render, setAttr } from'./element';
    
    let allPatches;
    let index = 0;  // 默认哪个需要打补丁
    
    function patch(node, patches) {
        allPatches = patches;
        
        // 给某个元素打补丁
        walk(node);
    }
    
    function walk(node) {
        let current = allPatches[index++];
        let childNodes = node.childNodes;
    
        // 先序深度，继续遍历递归子节点
        childNodes.forEach(child => walk(child));
    
        if (current) {
            doPatch(node, current); // 打上补丁
        }
    }
    
    functiondoPatch(node, patches) {
        // 遍历所有打过的补丁
        patches.forEach(patch => {
            switch (patch.type) {
                case'ATTR':
                    for (let key in patch.attr) {
                        let value = patch.attr[key];
                        if (value) {
                            setAttr(node, key, value);
                        } else {
                            node.removeAttribute(key);
                        }
                    }
                    break;
                case'TEXT':
                    node.textContent = patch.text;
                    break;
                case'REPLACE':
                    let newNode = patch.newNode;
                    newNode = (newNode instanceof Element) ? render(newNode) : document.createTextNode(newNode);
                    node.parentNode.replaceChild(newNode, node);
                    break;
                case'REMOVE':
                    node.parentNode.removeChild(node);
                    break;
                default:
                    break;
            }
        });
    }
    
    exportdefault patch;
看完代码还需要再来简单的分析一下

## patch做了什么？
用一个变量来得到传递过来的所有补丁allPatches

patch方法接收两个参数(node, patches)

在方法内部调用walk方法，给某个元素打上补丁

walk方法里获取所有的子节点

给子节点也进行先序深度优先遍历，递归walk

如果当前的补丁是存在的，那么就对其打补丁(doPatch)

doPatch打补丁方法会根据传递的patches进行遍历

判断补丁的类型来进行不同的操作

属性ATTR for in去遍历attrs对象，当前的key值如果存在，就直接设置属性setAttr；如果不存在对应的key值那就直接删除这个key键的属性

文字TEXT 直接将补丁的text赋值给node节点的textContent即可

替换REPLACE 新节点替换老节点，需要先判断新节点是不是Element的实例，是的话调用render方法渲染新节点；

不是的话就表明新节点是个文本节点，直接创建一个文本节点就OK了。

之后再通过调用父级parentNode的replaceChild方法替换为新的节点

删除REMOVE 直接调用父级的removeChild方法删除该节点
将patch方法默认导出方便调用
好了，一切都安静下来了。让我们回归index.js文件中，去调用一下diff和patch这两个重要方法，看看奇迹会不会发生吧

# 回归
```
// index.js
    
    import { createElement, render, renderDom } from'./element';
    // +++ 引入diff和patch方法
    import diff from'./diff';
    import patch from'./patch';
    // +++
    
    let virtualDom = createElement('ul', {class: 'list'}, [
        createElement('li', {class: 'item'}, ['周杰伦']),
        createElement('li', {class: 'item'}, ['林俊杰']),
        createElement('li', {class: 'item'}, ['王力宏'])
    ]);
    
    let el = render(virtualDom);
    renderDom(el, window.root);
    
    // +++
    // 创建另一个新的虚拟DOM
    let virtualDom2 = createElement('ul', {class: 'list-group'}, [
        createElement('li', {class: 'item active'}, ['七里香']),
        createElement('li', {class: 'item'}, ['一千年以后']),
        createElement('li', {class: 'item'}, ['需要人陪'])
    ]);
    // diff一下两个不同的虚拟DOM
    let patches = diff(virtualDom, virtualDom2);
    console.log(patches);
    // 将变化打补丁，更新到el
    patch(el, patches);
    // +++
```
将修改后的代码保存，会在浏览器里看到DOM被更新了，如下图

![DOM被更新](https://user-gold-cdn.xitu.io/2019/3/18/16990900e15871dd?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

到这里就finish了，内容有些多，可能不是很好的消耗，不过没关系，就让我用最后几句话来总结一下实现的整个过程吧

# 四句话
我们来梳理一下整个DOM-diff的过程：

1. 用JS对象模拟DOM（虚拟DOM）
2. 把此虚拟DOM转成真实DOM并插入页面中（render）
3. 如果有事件发生修改了虚拟DOM，比较两棵虚拟DOM树的差异，得到差异对象（diff）
4. 把差异对象应用到真正的DOM树上（patch）

行了，就这四句话吧，说多了就有点画蛇添足了。好久没有写文章了，很感谢小伙伴们的观看，辛苦各位了，886

