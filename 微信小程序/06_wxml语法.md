### 一.wxml基本格式

- 1.<span style="color:red">类似于HTML代码</span>：比如可以写成单标签，也可以写成双标签

- 2.<span style="color:red">必须有严格的闭合</span>：没有闭合会导致编译错误

- 3.<span style="color:red">大小写敏感</span>：class和Class是不同的属性

  **示例代码：**

  ```html
  <!-- 1.wxml的格式 -->
  <view></view>
  <image />
  <view class="" Class=""></view>
  ```

### 二.Mustache语法

示例代码：

```html
<!--pages/wxml/wxml.wxml-->
<!-- 2.Mustache语法 -->
<view>{{message}}</view>
<view>{{firstname}} {{lastname}}</view>
<view>{{firstname + ' ' + lastname}}</view>
<view>{{age >= 18 ? '成年人' : '未成年人'}}</view>
<view>{{nowtime}}</view>

<button bindtap="handleSwitchColor">切换颜色</button>
<view class="box {{isActive ? 'active' : ''}}">哈哈哈</view>
```

```css
/* pages/wxml/wxml.wxss */
.active {
  color: red;
}
```

```javascript
// pages/wxml/wxml.js
Page({

  /**
   * 页面的初始数据
   */
  data: {
    message: 'cyl',
    firstname: 'stephen',
    lastname: 'curry',
    age: 18,
    nowtime: new Date().toLocaleString(),
    isActive: false
  },
  onLoad() {
    setInterval(() => {
      this.setData({
        nowtime: new Date().toLocaleString()
      })
    }, 1000)
  },

  handleSwitchColor() {
    this.setData({
      isActive: !this.data.isActive
    })
  }
})
```

### 三.条件渲染

> wx:if - wx:elif - wx:else
>
> hidden

#### 1.wx:if - wx:elif - wx:else

- 当条件为true时，view组件会渲染出来

- 当条件为false时，view组件不会渲染出来

  **示例代码：**

  ```html
  <!--pages/wxml/wxml.wxml-->
  <!-- 3.条件渲染 -->
  <!-- wx:if 的使用 -->
  <button bindtap="handleSwitchShow">切换显示</button>
  <view wx:if="{{isShow}}">哈哈哈</view>
  
  <!-- wx:elif/wx:else -->
  <view wx:if="{{score >= 90}}">优秀</view>
  <view wx:elif="{{score >= 80}}">良好</view>
  <view wx:elif="{{score >= 60}}">及格</view>
  <view wx:else>不及格</view>
  ```

  ```javascript
  // pages/wxml/wxml.js
  Page({
  
    /**
     * 页面的初始数据
     */
    data: {
      isShow: false,
      score: 99,
    },
  
    handleSwitchShow() {
      this.setData({
        isShow: !this.data.isShow
      })
    }
  })
  ```

#### 2.block wx:if

- 因为 `wx:if` 是一个控制属性，需要将它添加到一个标签上。如果要一次性判断多个组件标签，可以使用一个 `<block/>` 标签将多个组件包装起来，并在上边使用 `wx:if` 控制属性。

  **示例代码：**

  ```html
  <block wx:if="{{true}}">
    <view> view1 </view>
    <view> view2 </view>
  </block>
  ```

- **注意：** `<block/>` 并不是一个组件，它仅仅是一个包装元素，不会在页面中做任何渲染，只接受控制属性。

#### 3.hidden属性

**示例代码：**

```html
<!-- hidden属性 -->
<view hidden="{{false}}">我是hidden属性控制的内容</view>
```

#### 4.wx:if vs hidden

- 因为 `wx:if` 之中的模板也可能包含数据绑定，所以当 `wx:if` 的条件值切换时，框架有一个局部渲染的过程，因为它会确保条件块在切换时销毁或重新渲染。

- 同时 `wx:if` 也是**惰性的**，如果在初始渲染条件为 `false`，框架什么也不做，在条件第一次变成真的时候才开始局部渲染。

- 相比之下，`hidden` 就简单的多，组件始终会被渲染，只是简单的控制显示与隐藏。

- 一般来说，`wx:if` 有更高的切换消耗而 `hidden` 有更高的初始渲染消耗。因此，如果需要频繁切换的情景下，用 `hidden` 更好，如果在运行时条件不大可能改变则 `wx:if` 较好。

### 四.列表渲染

#### 1.wx:for

- 在组件上使用 `wx:for` 控制属性绑定一个数组，即可使用数组中各项的数据重复渲染该组件。

- 默认数组的当前项的下标变量名默认为 `index`，数组当前项的变量名默认为 `item`

  **示例代码：**

  ```html
  <!-- 遍历数组 -->
  <view wx:for="{{['abc', 'cba',' nba']}}">{{item}} {{index}}</view>
  ```

- 也可以遍历字符串、数字

  **示例代码：**

  ```html
  <!-- 遍历字符串/数字 -->
  <view wx:for="cyl">{{item}} {{index}}</view>
  <view wx:for="{{10}}">{{item}}</view>
  ```

- `wx:for` 也可以嵌套

  **示例代码：**

  ```html
  <block wx:for="{{[1, 2, 3, 4, 5, 6, 7, 8, 9]}}" wx:for-item="i">
    <block wx:for="{{[1, 2, 3, 4, 5, 6, 7, 8, 9]}}" wx:for-item="j">
      <view wx:if="{{i <= j}}">
        {{i}} * {{j}} = {{i * j}}
      </view>
    </block>
  </block>
  ```

#### 2.wx:for-item && wx:for-index

- 使用 `wx:for-item` 可以指定数组当前元素的变量名

- 使用 `wx:for-index` 可以指定数组当前下标的变量名

  **示例代码：**

  ```html
  <view wx:for="{{['abc', 'cba',' nba']}}" wx:for-index="idx" wx:for-item="itemName">
    {{idx}}: {{itemName}}
  </view>
  ```

- **注意：起别名的时候不能用  - （例如inner-item就不行）**

#### 3.block wx:for

- 类似 `block wx:if`，也可以将 `wx:for` 用在`<block/>`标签上，以渲染一个包含多节点的结构块

  **示例代码：**

  ```html
  <block wx:for="{{[1, 2, 3]}}">
    <view> {{index}}: </view>
    <view> {{item}} </view>
  </block>
  ```

#### 4.wx:key

- 如果列表中项目的位置会动态改变或者有新的项目添加到列表中，并且希望列表中的项目保持自己的特征和状态（如 input 中的输入内容，switch 的选中状态），需要使用 `wx:key` 来指定列表中项目的<span style="color:red">唯一的标识符</span>。

- `wx:key` 的值以两种形式提供
  - 1.字符串，代表在 for 循环的 array 中 item 的某个 property，该 property 的值需要是列表中唯一的字符串或数字，且不能动态改变。
  - 2.保留关键字 `*this` 代表在 for 循环中的 item 本身，这种表示需要 item 本身是一个唯一的字符串或者数字。

- 当数据改变触发渲染层重新渲染的时候，会校正带有 key 的组件，框架会确保他们被重新排序，而不是重新创建，以确保使组件保持自身的状态，并且提高列表渲染时的效率。

### 五.模板

> WXML提供模板（template），可以在模板中定义代码片段，然后在不同的地方调用

#### 1.定义模板

使用 name 属性，作为模板的名字。然后在`<template/>`内定义代码片段

<span style="color:red">注：模板中包裹的内容，在没有被使用前，是不会进行任何的渲染</span>

**示例代码：**

```html
<!--
  index: int
  msg: string
  time: string
-->
<template name="msgItem">
  <view>
    <text> {{index}}: {{msg}} </text>
    <text> Time: {{time}} </text>
  </view>
</template>
```

#### 2.使用模板

使用 is 属性，声明需要的使用的模板，然后将模板所需要的 data 传入

**示例代码：**

```html
<template is="msgItem" data="{{...item}}"/>
Page({
  data: {
    item: {
      index: 0,
      msg: 'this is a template',
      time: '2016-09-15'
    }
  }
})
```

is 属性可以使用 Mustache 语法，来动态决定具体需要渲染哪个模板

**示例代码：**

```html
<template name="odd">
  <view> odd </view>
</template>
<template name="even">
  <view> even </view>
</template>

<block wx:for="{{[1, 2, 3, 4, 5]}}">
  <template is="{{item % 2 == 0 ? 'even' : 'odd'}}"/>
</block>
```

#### 3.模板的作用域

模板拥有自己的作用域，只能使用 data 传入的数据以及模板定义文件中定义的 `<wxs />` 模块。

### 六.引用

> WXML 提供两种文件引用方式`import`和`include`。

#### 1.import

`import`可以在该文件中使用目标文件定义的`template`，如：

在 item.wxml 中定义了一个叫`item`的`template`：

```html
<!-- item.wxml -->
<template name="item">
  <text>{{text}}</text>
</template>
```

在 index.wxml 中引用了 item.wxml，就可以使用`item`模板：

```html
<import src="item.wxml"/>
<template is="item" data="{{text: 'forbar'}}"/>
```

#### 2.import 的作用域

> import 有作用域的概念，即只会 import 目标文件中定义的 template，而不会 import 目标文件 import 的 template。

**如：C import B，B import A，在C中可以使用B定义的`template`，在B中可以使用A定义的`template`，但是C不能使用A定义的`template`**。

**示例代码：**

```html
<!-- A.wxml -->
<template name="A">
  <text> A template </text>
</template>
<!-- B.wxml -->
<import src="A.wxml"/>
<template name="B">
  <text> B template </text>
</template>
<!-- C.wxml -->
<import src="B.wxml"/>
<template is="A"/>  <!-- Error! Can not use tempalte when not import A. -->
<template is="B"/>
```

#### 3.include

`include` 可以将目标文件**除了** `<template/>` `<wxs/>` 外的整个代码引入，相当于是拷贝到 `include` 位置

**示例代码：**

```html
<!-- index.wxml -->
<include src="header.wxml"/>
<view> body </view>
<include src="footer.wxml"/>
<!-- header.wxml -->
<view> header </view>
<!-- footer.wxml -->
<view> footer </view>
```

#### 4.两种导入方式总结

- import导入：
  - 主要是导入template
  - 特点：不能进行递归导入
- include引入：
  - 将公共的wxml中的组件抽取到一个文件中
  - 特点：不能导入template/wxs，可以进行嵌套引入

### 补充：

#### 1.block的好处

- 将需要进行遍历或者判断的内容进行包裹
- 将遍历和判断的属性放在block便签中，不影响普通属性的阅读，提高代码的可读性

#### 2.两个注意点

**注意1：**<span style="color:red">当 `wx:for` 的值为字符串时，会将字符串解析成字符串数组</span>

```html
<view wx:for="array">
  {{item}}
</view>
```

等同于

```html
<view wx:for="{{['a','r','r','a','y']}}">
  {{item}}
</view>
```

**注意2：** <span style="color:red">花括号和引号之间如果有空格，将最终被解析成为字符串</span>

```html
<view wx:for="{{[1,2,3]}} ">
  {{item}}
</view>
```

等同于

```html
<view wx:for="{{[1,2,3] + ' '}}" >
  {{item}}
</view>
```