### 一.页面样式写法

#### 1.页面样式的三种写法

> 行内样式、页面样式、全局样式
>
> 三种样式都可以作用于页面的组件

**示例代码 :**

```html
<!--pages/wxss/wxss.wxml-->
<!-- 1.设置样式的三种方式 -->
<!-- 1.1. 行内（内联）样式 -->
<view style="color: red; font-size: 32px">哈哈哈</view>


<!-- 1.2. 页面样式 -->
<view class="box">呵呵呵</view>

<!-- 1.3. 全局样式 -->
<view class="container">嘿嘿嘿</view>

<!-- 2.三种样式作用于同一个组件 -->
<view style="background: red;" class="content">嘻嘻嘻</view>
```

```css
/* pages/wxss/wxss.wxss */
.box {
  color: blue;
  font-size: 32px;
}

.content {
  background-color: purple;
}
```

```css
/* app.wxss */
.container {
  color: pink;
  font-size: 32px;
}

.content {
  background-color: orange;
}
```

#### 2.如果有相同的样式

优先级依次是：<span style="color:red">**行内样式 > 页面样式 > 全局样式**</span>

### 二.支持的选择器

| 选择器           | 样例             | 样例描述                                       |
| :--------------- | :--------------- | :--------------------------------------------- |
| .class           | `.intro`         | 选择所有拥有 class="intro" 的组件              |
| #id              | `#firstname`     | 选择拥有 id="firstname" 的组件                 |
| element          | `view`           | 选择所有 view 组件                             |
| element, element | `view, checkbox` | 选择所有文档的 view 组件和所有的 checkbox 组件 |
| ::after          | `view::after`    | 在 view 组件后边插入内容                       |
| ::before         | `view::before`   | 在 view 组件前边插入内容                       |

### 三.wxss的扩展 -- 尺寸单位

> <span style="color:red">rpx（responsive pixel）</span> : 可以根据屏幕宽度进行自适应。规定屏幕宽为750rpx。
>
> 如在 iPhone6 上，屏幕宽度为375px，共有750个物理像素，则750rpx = 375px = 750物理像素，1rpx = 0.5px = 1物理像素。

**建议：** 开发微信小程序时设计师可以用 iPhone6 作为视觉稿的标准。

**注意：** 在较小的屏幕上不可避免的会有一些毛刺，请在开发时尽量避免这种情况。

### 四.wxss的扩展 -- 样式导入

> 使用`@import`语句可以导入外联样式表，`@import`后跟需要导入的外联样式表的相对路径，用`;`表示语句结束。

**示例代码：**

```css
/** common.wxss **/
.small-p {
  padding:5px;
}

/** app.wxss **/
@import "common.wxss";
.middle-p {
  padding:15px;
}
```