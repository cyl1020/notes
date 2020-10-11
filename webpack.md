# webpack

## 一.webpack的安装

- **安装webpack首先需要安装Node.js，Node.js自带了软件包管理工具npm**

- **查看自己的node版本：**

  ```javascript
  node --version
  ```

- **全局安装webpack(这里我先指定版本号3.6.0，因为vue cli2依赖该版本)**

  ```javascript
  npm install webpack@3.6.0 -g
  ```

- **局部安装webpack（后续才需要）**

  - `--save-dev`是开发时依赖，项目打包后不需要继续使用的。

    ```javascript
    cd 对应的目录
    npm install webpack@3.6.0 --save-dev
    ```

- **为什么全局安装后，还需要局部安装呢？**
  
  - `在终端直接执行webpack命令，使用的全局安装的webpack`
  - `当在package.json中定义了scripts时，其中包含了webpack命令，那么使用的是局部webpack`

## 二.webpack起步

### 1.准备工作

- **我们创建如下文件和文件夹：**

  ![image-20200425180644403](../AppData/Roaming/Typora/typora-user-images/image-20200425180644403.png)


- **文件和文件夹解析：**
  - `dist文件夹`：用于存放之后打包的文件
  - `src文件夹`：用于存放我们写的源文件
    - *main.js*：项目的入口文件。具体内容查看下面详情。
    - *mathUtils.js*：定义了一些数学工具函数，可以在其他地方引用，并且使用。具体内容查看下面的详情。
  - `index.html`：浏览器打开展示的首页html
  - `package.json`：通过`npm init`生成的，npm包管理的文件（暂时没有用上，后面才会用上）

- **mathUtils.js文件中的代码：**

  ![image-20200425181421188](../AppData/Roaming/Typora/typora-user-images/image-20200425181421188.png)

- **main.js文件中的代码：**

  ![image-20200425181451825](../AppData/Roaming/Typora/typora-user-images/image-20200425181451825.png)

### 2.js文件的打包

- **现在的js文件中使用了模块化的方式进行开发，他们可以直接使用吗？不可以。**

  - 因为如果直接在index.html引入这两个js文件，浏览器并不识别其中的模块化代码。
  - 另外，在真实项目中当有许多这样的js文件时，我们一个个引用非常麻烦，并且后期非常不方便对它们进行管理。

- **我们应该怎么做呢？使用`webpack`工具，对多个js文件进行打包。**

  - 我们知道，webpack就是一个模块化的打包工具，所以它支持我们代码中写模块化，可以对模块化的代码进行处理。
  - 另外，如果在处理完所有模块之间的关系后，将多个js打包到一个js文件中，引入时就变得非常方便了。

- **OK，如何打包呢？使用webpack的指令即可**

  ```javascript
  webpack src/main.js dist/bundle.js 
  //看上面的文件和文件夹
  //src/main.js 入口文件
  //dist/bundle.js 打包后的出口文件
  ```

- **打包完成后在控制台的输出****

![image-20200425182040334](../AppData/Roaming/Typora/typora-user-images/image-20200425182040334.png)

### 3.使用打包后的文件

- **打包后会在dist文件下，生成一个`bundle.js`文件**

  - bundle.js文件，是webpack处理了项目直接文件依赖后生成的一个js文件，我们只需要将这个js文件在index.html中引入即可

  - ![image-20200425182414648](../AppData/Roaming/Typora/typora-user-images/image-20200425182414648.png)

  - ![image-20200425182549246](../AppData/Roaming/Typora/typora-user-images/image-20200425182549246.png)

    

## 三.webpack配置

### 1.入口和出口

- **我们考虑一下，如果每次使用webpack的命令都需要写上入口和出口作为参数，就非常麻烦，有没有一种方法可以将这两个参数写到配置中，在运行时，直接读取呢？**

- **当然可以，就是创建一个`webpack.config.js`文件**

  ![image-20200425182801533](../AppData/Roaming/Typora/typora-user-images/image-20200425182801533.png)

  ​	

### 2.局部安装webpack

- **目前，我们使用的webpack是全局的webpack，如果我们想使用局部来打包呢？**
  - `因为一个项目往往依赖特定的webpack版本，全局的版本可能很这个项目的webpack版本不一致，导出打包出现问题。`
  - `所以通常一个项目，都有自己局部的webpack。`

- **第一步，项目中需要安装自己局部的webpack**

  - 这里我们让局部安装webpack3.6.0

  - Vue CLI3中已经升级到webpack4，但是它将配置文件隐藏了起来，所以查看起来不是很方便。

    ```javascript
    npm install webpack@3.6.0 --save-dev
    ```

    

- **第二步，通过`node_modules/.bin/webpack`启动webpack打包**

  ![image-20200425183948415](../AppData/Roaming/Typora/typora-user-images/image-20200425183948415.png)

### 3.package.json中定义启动

- **但是，每次执行都敲这么一长串有没有觉得不方便呢？**

  - OK，我们可以在package.json的scripts中定义自己的执行脚本。`这里在scripts中自定义的是build`

- **package.json中的scripts的脚本在执行时，会按照一定的顺序寻找命令对应的位置。**

  - 首先，会寻找本地的`node_modules/.bin`路径中对应的命令。

  - 如果没有找到，会去全局的环境变量中寻找。

  - 如何执行我们的build指令呢？

    ```javascript
    npm run build
    ```

    

![image-20200425184149749](../AppData/Roaming/Typora/typora-user-images/image-20200425184149749.png)

## 四.css-loader的使用

### 1.什么是loader？

- **loader是webpack中一个非常核心的概念。**
- **webpack用来做什么呢？**
  - 在我们之前的实例中，我们主要是用webpack来处理我们写的js代码，并且webpack会自动处理js之间相关的依赖。
  - 但是，在开发中我们不仅仅有基本的js代码处理，我们也需要加载css、图片，也包括一些高级的将ES6转成ES5代码，将TypeScript转成ES5代码，将scss、less转成css，将.jsx、.vue文件转成js文件等等。
  - 对于webpack本身的能力来说，对于这些转化是不支持的。
  - 那怎么办呢？给webpack扩展对应的loader就可以啦。
- **loader使用过程：**
  - `步骤一：通过npm安装需要使用的loader`
  - `步骤二：在webpack.config.js中的modules关键字下进行配置`

### 2.css文件处理 - 准备工作

- **项目开发过程中，我们必然需要添加很多的样式，而样式我们往往写到一个单独的文件中。**

  - 在src目录中，创建一个css文件，其中创建一个normal.css文件。

  - 我们也可以重新组织文件的目录结构，将零散的js文件放在一个js文件夹中。

    <img src="../AppData/Roaming/Typora/typora-user-images/image-20200425184714317.png" alt="image-20200425184714317"  />

- **normal.css中的代码非常简单，就是将body设置为red**

  ![image-20200425184758439](../AppData/Roaming/Typora/typora-user-images/image-20200425184758439.png)

- **但是，这个时候normal.css中的样式会生效吗？**

  - 当然不会，因为我们压根就没有引用它。
  - webpack也不可能找到它，因为我们只有一个入口，webpack会从入口开始查找其他依赖的文件。

- **在入口文件中引用：**

  ![image-20200425184841709](../AppData/Roaming/Typora/typora-user-images/image-20200425184841709.png)

### 3.css文件处理 – 打包报错信息

- **重新打包，会出现如下错误：**

  ![image-20200425185127707](../AppData/Roaming/Typora/typora-user-images/image-20200425185127707.png)

- **这个错误告诉我们：加载normal.css文件必须有对应的loader。**

### 4.css文件处理 – css-loader

- **在webpack的官方中，我们可以找到如下关于样式的loader使用方法：**

  ![image-20200425185252384](../AppData/Roaming/Typora/typora-user-images/image-20200425185252384.png)

- **按照官方配置webpack.config.js文件**

  - 注意：配置中有一个style-loader，我们并不知道它是什么，所以可以暂时不进行配置。

- **重新打包项目：**

  ![image-20200425185435976](../AppData/Roaming/Typora/typora-user-images/image-20200425185435976.png)

- **但是，运行index.html，你会发现样式并没有生效。**

  - 原因是css-loader只负责加载css文件，但是并不负责将css具体样式嵌入到文档中。
  - 这个时候，我们还需要一个`style-loader`帮助我们处理。

### 5.css文件处理 – style-loader

- **我们来安装style-loader**

  ```javascript
  npm install --save-dev style-loader
  ```

- **注意：style-loader需要放在css-loader的前面。**

- **疑惑：不对吧？按照我们的逻辑，在处理css文件过程中，应该是css-loader先加载css文件，再由style-loader来进行进一步的处理，为什么会将style-loader放在前面呢？**

- **答案：这次因为webpack在读取使用的loader的过程中，是按照`从右向左`的顺序读取的。**

- **目前，webpack.config.js的配置如下：**

  ![image-20200425185707549](../AppData/Roaming/Typora/typora-user-images/image-20200425185707549.png)



## 五.less文件处理

### 1.less文件处理 – 准备工作

- **如果我们希望在项目中使用less、scss、stylus来写样式，webpack是否可以帮助我们处理呢？**

  - 我们这里以less为例，其他也是一样的。

- **我们还是先创建一个less文件，依然放在css文件夹中**

  ![image-20200425185845232](../AppData/Roaming/Typora/typora-user-images/image-20200425185845232.png)

- **在main.js中引入less文件**

  ![image-20200425190030063](../AppData/Roaming/Typora/typora-user-images/image-20200425190030063.png)

- **打包**

  ![image-20200425190055094](../AppData/Roaming/Typora/typora-user-images/image-20200425190055094.png)

### 2.less文件处理 – less-loader

- **继续在官方中查找，我们会找到less-loader相关的使用说明**

- **首先，还是需要安装对应的loader**

  - 注意：我们这里还安装了less，因为webpack会使用less对less文件进行编译】

    ```javascript
    npm install --save-dev less-loader less
    ```

- **其次，修改对应的配置文件**

  - 添加一个rules选项，用于处理.less文件

    ![image-20200425190308074](../AppData/Roaming/Typora/typora-user-images/image-20200425190308074.png)

## 六.图片文件处理

### 1.图片文件处理 – 资源准备阶段

- **首先，我们在项目中加入两张图片：**
  - 一张较小的图片test01.jpg(小于8kb)，一张较大的图片test02.jpeg(大于8kb)
  - 待会儿我们会针对这两张图片进行不同的处理

- **我们先考虑在css样式中引用图片的情况，所以我更改了normal.css中的样式：**

  ![image-20200425190430886](../AppData/Roaming/Typora/typora-user-images/image-20200425190430886.png)

- **如果我们现在直接打包，会出现如下问题**

  ![image-20200425190448307](../AppData/Roaming/Typora/typora-user-images/image-20200425190448307.png)

### 2.图片文件处理 – url-loader

- **图片处理，我们使用url-loader来处理，依然先安装url-loader**

  ```javascript
  npm install --save-dev url-loader
  ```

- **修改webpack.config.js配置文件：**

  ![image-20200425190616114](../AppData/Roaming/Typora/typora-user-images/image-20200425190616114.png)

- **再次打包，运行index.html，就会发现我们的背景图片选出了出来。**
  - 而仔细观察，你会发现背景图是通过base64显示出来的
  - OK，这也是limit属性的作用，当图片小于8kb时，对图片进行base64编码

### 3.图片文件处理 – file-loader

- **那么问题来了，如果大于8kb呢？我们将background的图片改成test02.jpg**
  - 这次因为大于8kb的图片，会通过file-loader进行处理，但是我们的项目中并没有file-loader

    ![image-20200425190748281](../AppData/Roaming/Typora/typora-user-images/image-20200425190748281.png)

- **所以，我们需要安装file-loader**

  ```javascript
  npm install --save-dev file-loader
  ```

- **再次打包，就会发现dist文件夹下多了一个图片文件**

  ![image-20200425190850747](../AppData/Roaming/Typora/typora-user-images/image-20200425190850747.png)

### 4.图片文件处理 – 修改文件名称

- **我们发现webpack自动帮助我们生成一个非常长的名字**

  - 这是一个32位hash值，目的是防止名字重复
  - 但是，真实开发中，我们可能对打包的图片名字有一定的要求
  - 比如，将所有的图片放在一个文件夹中，跟上图片原来的名称，同时也要防止重复 

- **所以，我们可以在options中添加上如下选项：**

  - `img`：文件要打包到的文件夹

  - `name`：获取图片原来的名字，放在该位置

  - `hash:8`：为了防止图片名称冲突，依然使用hash，但是我们只保留8位

  - `ext`：使用图片原来的扩展名

    ![image-20200425191044205](../AppData/Roaming/Typora/typora-user-images/image-20200425191044205.png)

- **但是，我们发现图片并没有显示出来，这是因为图片使用的路径不正确**

  - 默认情况下，webpack会将生成的路径直接返回给使用者

  - 但是，我们整个程序是打包在dist文件夹下的，所以这里我们需要在路径下再添加一个`dist/`

    ![image-20200425191121207](../AppData/Roaming/Typora/typora-user-images/image-20200425191121207.png)

## 七.babel的使用

### 1.ES6语法处理

- **如果你仔细阅读webpack打包的js文件，发现写的ES6语法并没有转成ES5，那么就意味着可能一些对ES6还不支持的浏览器没有办法很好的运行我们的代码。**

- **在前面我们说过，如果希望将ES6的语法转成ES5，那么就需要使用babel。**

  - 而在webpack中，我们直接使用babel对应的loader就可以了。

    ```javascript
    npm install --save-dev babel-loader@7 babel-core babel-preset-es2015
    ```

- **配置webpack.config.js文件**

  ![image-20200425191401036](../AppData/Roaming/Typora/typora-user-images/image-20200425191401036.png)

- **重新打包，查看bundle.js文件，发现其中的内容变成了ES5的语法**

## 八.webpack配置vue

### 1.引入vue.js

- **后续项目中，我们会使用Vuejs进行开发，而且会以特殊的文件来组织vue的组件。**

  - 所以，下面我们来学习一下如何在我们的webpack环境中集成Vuejs

- **现在，我们希望在项目中使用Vuejs，那么必然需要对其有依赖，所以需要先进行安装**

  - `注：因为我们后续是在实际项目中也会使用vue的，所以并不是开发时依赖`

    ```javascript
    npm install vue --save
    ```

- **那么，接下来就可以按照我们之前学习的方式来使用Vue了**

  ![image-20200425191646637](../AppData/Roaming/Typora/typora-user-images/image-20200425191646637.png)

  ![image-20200425191653576](../AppData/Roaming/Typora/typora-user-images/image-20200425191653576.png)

### 2.打包项目 – 错误信息

- **修改完成后，重新打包，运行程序：**
  - 打包过程没有任何错误(因为只是多打包了一个vue的js文件而已)
  - 但是运行程序，没有出现想要的效果，而且浏览器中有报错

![image-20200425191807939](../AppData/Roaming/Typora/typora-user-images/image-20200425191807939.png)

- **这个错误说的是我们使用的是runtime-only版本的Vue，什么意思呢？**

  - 这里我只说解决方案：`Vue不同版本构建`，后续我具体讲解runtime-only和runtime-compiler的区别。

- **所以我们修改webpack的配置，添加如下内容即可**

  ![image-20200425191859819](../AppData/Roaming/Typora/typora-user-images/image-20200425191859819.png)

### 3.el和template区别（一）

- **正常运行之后，我们来考虑另外一个问题：**

  - 如果我们希望将data中的数据显示在界面中，就必须是修改index.html
  - 如果我们后面自定义了组件，也必须修改index.html来使用组件
  - 但是html模板在之后的开发中，我并不希望手动的来频繁修改，是否可以做到呢？

- **定义template属性：**

  - 在前面的Vue实例中，我们定义了el属性，用于和index.html中的#app进行绑定，让Vue实例之后可以管理它其中的内容

  - 这里，我们可以将div元素中的{{message}}内容删掉，只保留一个基本的id为div的元素

  - 但是如果我依然希望在其中显示{{message}}的内容，应该怎么处理呢？

  - 我们可以再定义一个template属性，代码如下：

    ![image-20200425192023767](../AppData/Roaming/Typora/typora-user-images/image-20200425192023767.png)

### 4.el和template区别（二）

- **重新打包，运行程序，显示一样的结果和HTML代码结构**
- **那么，el和template模板的关系是什么呢？**
  - 在我们之前的学习中，我们知道el用于指定Vue要管理的DOM，可以帮助解析其中的指令、事件监听等等。
  - `而如果Vue实例中同时指定了template，那么template模板的内容会替换掉挂载的对应el的模板。`
- **这样做有什么好处呢？**
  - 这样做之后我们就不需要在以后的开发中再次操作index.html，只需要在template中写入对应的标签即可

### 5.Vue组件化开发引入

- **在学习组件化开发的时候，我说过以后的Vue开发过程中，我们都会采用组件化开发的思想。**

  - 那么，在当前项目中，如果我也想采用组件化的形式进行开发，应该怎么做呢？

- **查看下面的代码：**	

  - 当然，我们也可以将下面的代码抽取到一个js文件中，并且导出。

    ![image-20200425192316638](../AppData/Roaming/Typora/typora-user-images/image-20200425192316638.png)

### 6.  .vue文件封装处理

- **但是一个组件以一个js对象的形式进行组织和使用的时候是非常不方便的**

  - 一方面编写template模块非常的麻烦
  - 另外一方面如果有样式的话，我们写在哪里比较合适呢？

- **现在，我们以一种全新的方式来组织一个vue的组件**

  ![image-20200425192612279](../AppData/Roaming/Typora/typora-user-images/image-20200425192612279.png)

- **但是，这个时候这个文件可以被正确的加载吗？**
  - 必然不可以，这种特殊的文件以及特殊的格式，必须有人帮助我们处理。
  - 谁来处理呢？`vue-loader以及vue-template-compiler`。

- **安装vue-loader和vue-template-compiler**

  ```javascript
  npm install vue-loader vue-template-compiler --save-dev
  ```

- **修改webpack.config.js的配置文件：**

  ![image-20200425192733281](../AppData/Roaming/Typora/typora-user-images/image-20200425192733281.png)

## 九.plugin的使用

### 1.认识plugin

- **plugin是什么？**
  - plugin是插件的意思，通常是用于对某个现有的架构进行扩展。
  - webpack中的插件，就是对webpack现有功能的各种扩展，比如打包优化，文件压缩等等。

- **loader和plugin区别**
  - loader主要用于转换某些类型的模块，它是一个转换器。
  - plugin是插件，它是对webpack本身的扩展，是一个扩展器。
- **plugin的使用过程：**
  - `步骤一：通过npm安装需要使用的plugins(某些webpack已经内置的插件不需要安装)`
  - `步骤二：在webpack.config.js中的plugins中配置插件。`

### 2.添加版权的Plugin

- **我们先来使用一个最简单的插件，为打包的文件添加版权声明**

  - 该插件名字叫BannerPlugin，属于webpack自带的插件。

- **按照下面的方式来修改webpack.config.js的文件：**

  **![image-20200425193133702](../AppData/Roaming/Typora/typora-user-images/image-20200425193133702.png)**

- **重新打包程序：查看bundle.js文件的头部，看到如下信息**

![image-20200425193153544](../AppData/Roaming/Typora/typora-user-images/image-20200425193153544.png)

### 3.打包html的plugin

- **目前，我们的index.html文件是存放在项目的根目录下的。**
  - 我们知道，在真实发布项目时，发布的是dist文件夹中的内容，但是dist文件夹中如果没有index.html文件，那么打包的js等文件也就没有意义了。
  - 所以，我们需要将index.html文件打包到dist文件夹中，这个时候就可以使用HtmlWebpackPlugin插件
- **HtmlWebpackPlugin插件可以为我们做这些事情：**
  - 自动生成一个index.html文件(可以指定模板来生成)
  - 将打包的js文件，自动通过script标签插入到body中

- **安装HtmlWebpackPlugin插件**

  ```javascript
  npm install html-webpack-plugin --save-dev
  ```

- **使用插件，修改webpack.config.js文件中plugins部分的内容如下：**

  ​	![image-20200425193450354](../AppData/Roaming/Typora/typora-user-images/image-20200425193450354.png)

  - 这里的template表示根据什么模板来生成index.html
  - 另外，我们需要删除之前在output中添加的publicPath属性
  - `否则插入的script标签中的src可能会有问题`

### 4.js压缩的Plugin

- **在项目发布之前，我们必然需要对js等文件进行压缩处理**

  - 这里，我们就对打包的js文件进行压缩

  - 我们使用一个第三方的插件uglifyjs-webpack-plugin，并且版本号指定1.1.1，和CLI2保持一致

    ```javascript
    npm install uglifyjs-webpack-plugin@1.1.1 --save-dev
    ```

- **修改webpack.config.js文件，使用插件：**

  <img src="../AppData/Roaming/Typora/typora-user-images/image-20200425193650368.png" alt="image-20200425193650368"  />

- **查看打包后的bunlde.js文件，是已经被压缩过了。**

## 十.搭建本地服务器

- **webpack提供了一个可选的本地开发服务器，这个本地服务器基于node.js搭建，内部使用express框架，可以实现我们想要的让浏览器自动刷新显示我们修改后的结果。**

- **不过它是一个单独的模块，在webpack中使用之前需要先安装它**

  ```javascript
  npm install --save-dev webpack-dev-server@2.9.1
  ```

- **devserver也是作为webpack中的一个选项，选项本身可以设置如下属性：**

  - `contentBase`：为哪一个文件夹提供本地服务，默认是根文件夹，我们这里要填写./dist
  - `port`：端口号
  - `inline`：页面实时刷新
  - `historyApiFallback`：在SPA页面中，依赖HTML5的history模式

- **webpack.config.js文件配置修改如下：**

  ![image-20200425204806010](../AppData/Roaming/Typora/typora-user-images/image-20200425204806010.png)

- **我们可以再配置另外一个scripts：**

  - --open参数表示直接打开浏览器

    ![image-20200425204835616](../AppData/Roaming/Typora/typora-user-images/image-20200425204835616.png)



## 十一.webpack.config.js

### 1.经过上面步骤的处理后 现在的webpack.config.js如下：

```javascript
const path = require('path')
const webpack = require('webpack')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const UglifyjsWebpackPlugin = require('uglifyjs-webpack-plugin')

module.exports = {
  entry: './src/main.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js',
    // publicPath: 'dist/'
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        // css-loader只负责将css文件进行加载
        // style-loader负责将样式添加到DOM中
        // 使用多个loader时, 是从右向左
        use: [ 'style-loader', 'css-loader' ]
      },
      {
        test: /\.less$/,
        use: [{
          loader: "style-loader", // creates style nodes from JS strings
        }, {
          loader: "css-loader" // translates CSS into CommonJS
        }, {
          loader: "less-loader", // compiles Less to CSS
        }]
      },
      {
        test: /\.(png|jpg|gif|jpeg)$/,
        use: [
          {
            loader: 'url-loader',
            options: {
              // 当加载的图片, 小于limit时, 会将图片编译成base64字符串形式.
              // 当加载的图片, 大于limit时, 需要使用file-loader模块进行加载.
              limit: 13000,
              name: 'img/[name].[hash:8].[ext]'
            },
          }
        ]
      },
      {
        test: /\.js$/,
        // exclude: 排除
        // include: 包含
        exclude: /(node_modules|bower_components)/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['es2015']
          }
        }
      },
      {
        test: /\.vue$/,
        use: ['vue-loader']
      }
    ]
  },
  resolve: {
    // alias: 别名
    extensions: ['.js', '.css', '.vue'],
    alias: {
      'vue$': 'vue/dist/vue.esm.js'
    }
  },
  plugins: [
      new webpack.BannerPlugin('最终版权归aaa所有'),
      new HtmlWebpackPlugin({
        template: 'index.html'
      }),
      new UglifyjsWebpackPlugin() //开发的时候不需要
  ],
  //在最后打包的时候这里不需要
  devServer: {
    contentBase: './dist',
    inline: true
  }
}
```

### 2.package.json如下：

```javascript
{
  "name": "meetwebpack",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "dev": "webpack-dev-server --open"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "babel-core": "^6.26.3",
    "babel-loader": "^7.1.5",
    "babel-preset-es2015": "^6.24.1",
    "css-loader": "^2.0.2",
    "file-loader": "^3.0.1",
    "html-webpack-plugin": "^3.2.0",
    "less": "^3.9.0",
    "less-loader": "^4.1.0",
    "style-loader": "^0.23.1",
    "uglifyjs-webpack-plugin": "^1.1.1",
    "url-loader": "^1.1.2",
    "vue-loader": "^13.0.0",
    "vue-template-compiler": "^2.5.21",
    "webpack": "^3.6.0",
    "webpack-dev-server": "^2.9.3"
  },
  "dependencies": {
    "vue": "^2.5.21"
  }
}

```

