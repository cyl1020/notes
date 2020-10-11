### 一.网络请求

#### 1.wx.request(Object object)

> 发起 HTTPS 网络请求

**参数**

| 属性         | 类型                      | 默认值 | 必填 | 说明                                                         |
| :----------- | :------------------------ | :----- | :--- | :----------------------------------------------------------- |
| url          | string                    |        | 是   | 开发者服务器接口地址                                         |
| data         | string/object/ArrayBuffer |        | 否   | 请求的参数                                                   |
| header       | Object                    |        | 否   | 设置请求的 header，header 中不能设置 Referer。 `content-type` 默认为 `application/json` |
| timeout      | number                    |        | 否   | 超时时间，单位为毫秒                                         |
| method       | string                    | GET    | 否   | HTTP 请求方法                                                |
| dataType     | string                    | json   | 否   | 返回的数据格式                                               |
| responseType | string                    | text   | 否   | 响应的数据类型                                               |
| enableHttp2  | boolean                   | false  | 否   | 开启 http2                                                   |
| enableQuic   | boolean                   | false  | 否   | 开启 quic                                                    |
| enableCache  | boolean                   | false  | 否   | 开启 cache                                                   |
| success      | function                  |        | 否   | 接口调用成功的回调函数                                       |
| fail         | function                  |        | 否   | 接口调用失败的回调函数                                       |
| complete     | function                  |        | 否   | 接口调用结束的回调函数（调用成功、失败都会执行）             |

#### 2.封装系统提供的API

**封装的原因：**

- 1.降低网络请求和wx.requset的耦合度

- 2.使用Promise的方法获取回调的结果

```javascript
// utils/network.js
export default function axios(options) {
  //1.创建一个promise对象
  return new Promise((reslove, reject) => {
    //2.使用系统api发送请求
    wx.request({
      //路径
      url: options.url,
      //请求的方式（get/post）
      method: options.method || 'get',
      //请求的接口需要的一些参数
      data: options.data || {},
      //3.成功的回调
      success: reslove,
      //4.失败的回调
      fail: reject
    })
  })
}
```

#### 3.使用封装好的函数

```javascript
// pages/wxs/wxs.js
import axios from '../../utils/network.js'
Page({
  async axios() {
    const reslut = await axios({
      url: 'http://152.136.185.210:8000/api/z8/recommend'
    })
    console.log(reslut);
  },

  /**
   * 生命周期函数--监听页面加载
   */
  onLoad: function (options) {
    this.axios()
  }
})
```

### 二.展示弹窗

#### 1.wx.showToast(Object object)

> 显示消息提示框

**参数**

| 属性     | 类型     | 默认值    | 必填 | 说明                                             |
| :------- | :------- | :-------- | :--- | :----------------------------------------------- |
| title    | string   |           | 是   | 提示的内容                                       |
| icon     | string   | 'success' | 否   | 图标                                             |
| image    | string   |           | 否   | 自定义图标的本地路径，image 的优先级高于 icon    |
| duration | number   | 1500      | 否   | 提示的延迟时间                                   |
| mask     | boolean  | false     | 否   | 是否显示透明蒙层，防止触摸穿透                   |
| success  | function |           | 否   | 接口调用成功的回调函数                           |
| fail     | function |           | 否   | 接口调用失败的回调函数                           |
| complete | function |           | 否   | 接口调用结束的回调函数（调用成功、失败都会执行） |

**示例代码：**

```js
wx.showToast({
  title: '成功',
  icon: 'success',
  duration: 2000
})
```

#### 2.wx.showModal(Object object)

> 显示模态对话框

**参数**

| 属性         | 类型     | 默认值  | 必填 | 说明                                               |
| :----------- | :------- | :------ | :--- | :------------------------------------------------- |
| title        | string   |         | 否   | 提示的标题                                         |
| content      | string   |         | 否   | 提示的内容                                         |
| showCancel   | boolean  | true    | 否   | 是否显示取消按钮                                   |
| cancelText   | string   | '取消'  | 否   | 取消按钮的文字，最多 4 个字符                      |
| cancelColor  | string   | #000000 | 否   | 取消按钮的文字颜色，必须是 16 进制格式的颜色字符串 |
| confirmText  | string   | '确定'  | 否   | 确认按钮的文字，最多 4 个字符                      |
| confirmColor | string   | #576B95 | 否   | 确认按钮的文字颜色，必须是 16 进制格式的颜色字符串 |
| success      | function |         | 否   | 接口调用成功的回调函数                             |
| fail         | function |         | 否   | 接口调用失败的回调函数                             |
| complete     | function |         | 否   | 接口调用结束的回调函数（调用成功、失败都会执行）   |

**object.success 回调函数**

**参数**

| 属性    | 类型    | 说明                                                         |
| :------ | :------ | :----------------------------------------------------------- |
| confirm | boolean | 为 true 时，表示用户点击了确定按钮                           |
| cancel  | boolean | 为 true 时，表示用户点击了取消（用于 Android 系统区分点击蒙层关闭还是点击取消按钮关闭） |

**示例代码：**

```js
wx.showModal({
  title: '提示',
  content: '这是一个模态弹窗',
  success (res) {
    if (res.confirm) {
      console.log('用户点击确定')
    } else if (res.cancel) {
      console.log('用户点击取消')
    }
  }
})
```

#### 3.wx.showLoading(Object object)

> 显示 loading 提示框。需主动调用 wx.hideLoading 才能关闭提示框

**参数**

| 属性     | 类型     | 默认值 | 必填 | 说明                                             |
| :------- | :------- | :----- | :--- | :----------------------------------------------- |
| title    | string   |        | 是   | 提示的内容                                       |
| mask     | boolean  | false  | 否   | 是否显示透明蒙层，防止触摸穿透                   |
| success  | function |        | 否   | 接口调用成功的回调函数                           |
| fail     | function |        | 否   | 接口调用失败的回调函数                           |
| complete | function |        | 否   | 接口调用结束的回调函数（调用成功、失败都会执行） |

**示例代码：**

```js
wx.showLoading({
  title: '加载中',
})

setTimeout(function () {
  wx.hideLoading()
}, 2000)
```

#### 4.wx.showActionSheet(Object object)

> 显示操作菜单

**参数**

| 属性      | 类型           | 默认值  | 必填 | 说明                                             |
| :-------- | :------------- | :------ | :--- | :----------------------------------------------- |
| itemList  | Array.<string> |         | 是   | 按钮的文字数组，数组长度最大为 6                 |
| itemColor | string         | #000000 | 否   | 按钮的文字颜色                                   |
| success   | function       |         | 否   | 接口调用成功的回调函数                           |
| fail      | function       |         | 否   | 接口调用失败的回调函数                           |
| complete  | function       |         | 否   | 接口调用结束的回调函数（调用成功、失败都会执行） |

**object.success 回调函数**

**参数**

| 属性     | 类型   | 说明                                        |
| :------- | :----- | :------------------------------------------ |
| tapIndex | number | 用户点击的按钮序号，从上到下的顺序，从0开始 |

**示例代码：**

```js
wx.showActionSheet({
  itemList: ['A', 'B', 'C'],
  success (res) {
    console.log(res.tapIndex)
  },
  fail (res) {
    console.log(res.errMsg)
  }
})
```

### 三.页面分享

#### onShareAppMessage(Object object)

> 监听用户点击页面内转发按钮（button 组件 `open-type="share"`）或右上角菜单“转发”按钮的行为，并自定义转发内容。

**注意：只有定义了此事件处理函数，右上角菜单才会显示“转发”按钮**

**参数 Object object**:

| 参数       | 类型   | 说明                                                         |
| :--------- | :----- | :----------------------------------------------------------- |
| from       | String | 转发事件来源。 `button`：页面内转发按钮； `menu`：右上角转发菜单 |
| target     | Object | 如果 `from` 值是 `button`，则 `target` 是触发这次转发事件的 `button`，否则为 `undefined` |
| webViewUrl | String | 页面中包含web-view组件时，返回当前web-view的url              |

此事件处理函数需要 return 一个 Object，用于自定义转发内容，返回内容如下：

**自定义转发内容** 基础库 2.8.1 起，分享图支持云图片。

| 字段     | 说明                                                         | 默认值                                    |
| :------- | :----------------------------------------------------------- | :---------------------------------------- |
| title    | 转发标题                                                     | 当前小程序名称                            |
| path     | 转发路径                                                     | 当前页面 path ，必须是以 / 开头的完整路径 |
| imageUrl | 自定义图片路径，可以是本地文件路径、代码包文件路径或者网络图片路径。支持PNG及JPG。显示图片长宽比是 5:4。 | 使用默认截图                              |

**示例代码：**

```javascript
Page({
  onShareAppMessage: function (res) {
    if (res.from === 'button') {
      // 来自页面内转发按钮
      console.log(res.target)
    }
    return {
      title: '自定义转发标题',
      path: '/page/user?id=123'
    }
  }
})
```

### 四.小程序登录

#### 1.实现步骤

- 1.调用wx.login获取code
- 2.调用wx.request发送code到自己的服务器（自己的服务器返回一个登录态的标识，如token）
- 3.将登录态的标识token进行储存，以便下次使用
- 4.请求需要登录态标识的接口时，携带token

#### 2.图解

<img src="https://img-blog.csdnimg.cn/20181119160016335.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2R3YjEyMzQ1NjEyMzQ1Ng==,size_16,color_FFFFFF,t_70"  />

#### 3.实现代码

```js
App({
  globalData: {
    token: ''
  },
  onLaunch(options) {
    //1.先从缓存中取出token
    const token = wx.getStorageSync('token')

    //2.判断token是否有值
    if (token && token.length !== 0) { //已经有token，验证token是否过期
      //验证token是否过期
      wx.request({
        url: 'http://123.207.32.32:3000/auth',
        method: 'post',
        header: {
          token
        },
        success: res => {
          if (res.data.message === '已登录') {
            this.globalData.token = token
          } else {
            this.login()
          }
        }
      })
    } else { // 没有token，进行登录操作
      this.login()
    }
  },

  login() {
    //登录操作
    wx.login({
      //code只有5分钟的有效期
      success: function (res) {
        //1.获取code
        const code = res.code

        //2.将code发送给服务器
        wx.request({
          url: 'http://123.207.32.32:3000/login',
          method: 'post',
          data: {
            code
          },
          success: function (res) {
            //1.取出token
            const token = res.data.token

            //2.保存token
            this.globalData.token = token

            //3.进行本地存储
            wx.setStorageSync('token', token)
          }.bind(this)
        })
      }.bind(this)
    })
  }
})
```

### 五.页面跳转（navigator）

**open-type 的合法值**

| 值           | 说明                                                  |
| :----------- | :---------------------------------------------------- |
| navigate     | 对应 wx.navigateTo 或 wx.navigateToMiniProgram 的功能 |
| redirect     | 对应 wx.redirectTo 的功能                             |
| switchTab    | 对应 wx.switchTab 的功能                              |
| reLaunch     | 对应 wx.reLaunch 的功能                               |
| navigateBack | 对应 wx.navigateBack 的功能                           |
| exit         | 退出小程序，`target="miniProgram"`时生效              |

- redirect：关闭当前页面，跳转到应用内的某个页面，但是不允许跳到tabbar页面，并且不能返回（不是一个压栈）
- switchTab：跳转到tabbar页面，并关闭其他所有非tabbar页面（需要在tabbar中定义的）
- reLaunch：关闭所有的页面，打开应用中某个页面（直接展示某个页面，并且可以跳转到tabbar页面）