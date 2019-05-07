## 一、性能优化（前八后二） ##
* 1、css

    * 1.1 首屏优化。首页出现的时间很重要，内联样式渲染速度比link的速度快，建议仅仅首页内联。懒加载（需要再加载）或者预加载（先加载重要内容）。
    * 1.2异步加载。常识：css阻塞浏览器渲染，在css文件没有请求->下载->解析之前，浏览器不会渲染任何已处理的内容（用户也不需要将一个无样式的界面展示给用户）。首屏加载后，加载其它css会阻塞其它html和js的加载。使用一下四种css异步加载。
    
     * ① js动态创建link元素并插入到DOM中：
       
       ```  
       创建link标签
        const myCSS = document.createElement( "link" );
        myCSS.rel = "stylesheet";
        myCSS.href = "mystyles.css";
        // 插入到header的最后位置
        document.head.insertBefore( myCSS, document.head.childNodes[ document.head.childNodes.length - 1 ].nextSibling );

       ```    
       
     * ② 利用浏览器加载link标签优先级问题，如果link的media属性值设置为浏览器不识别的类型，就会最后加载它。记得onload后再修改为正常的媒体类型。
        
        ```
        <link rel="stylesheet" href="mystyles.css" media="顺便起的名字" onload="this.media='all'">
        ```   
     * ③ 修改link的rel属性为可选样式表‘alternate’，onload后再恢复正常。   
        ```
        <link rel="alternate stylesheet" href="mystyles.css" onload="this.rel='stylesheet'">
        ```
     * ④ 添加新属性rel='preload'含css，as="style"（类型），浏览器有兼容问题。  
      
        ```
        <link rel="preload" href="mystyles.css" as="style" onload="this.rel='stylesheet'">
 
        ```   
    * 1.3 webpack/gulp/grunt文件压缩（删除空格）
    * 1.4 注意css书写内功：
     * css规范，删除无用代码，提取公共代码（less/sass）
     * 少嵌套、避免使用通配符和属性选择器、
     * 尽量少用耗性能的属性比如、box-shadow/border-radius/filter/filter/:nth-child等
     *  优化重绘和重排：
     * 触发重排（布局）的属性比如：font-size/font-family/margin/伪类/滚动条/窗口大小,flex性能好些，参考[CSS Trigger](https://csstriggers.com/);
            常用技巧：少用table布局、动画元素absolute/fixed/、img设置宽高、
     * 重绘（颜色）是不可避免的，比如颜色、背景色等，比如scroll触发hover，写动画的时候避免重绘，常用工具[硬件加速](https://www.sitepoint.com/introduction-to-hardware-acceleration-css-animations/)和[will-change](https://drafts.csswg.org/css-will-change/),
            常用技巧：开启GPU硬件加速、
     * 建议使用link代替@import，import影响浏览器并行下载。
     * 优先使用class，少用id,
     * 图标处理：cssSprite(雪碧图)，字体图标，把图片转成base64。
     * body设置最新宽度、防止屏幕变小出现滚动条。
* 2、js 浏览器解析到script标签时候，会阻塞其它任务，知道本js文件加载完，这是阻塞的。
    * 2.1、script标签都放到body底部（首屏需要的js文件分离放到head上）
    * 2.2、标签加入 defer
    
       ```
        <script src="test.js" type="text/javascript" defer></script>
       ```
    * 2.3、动态加载js，比如，懒加载/按需加载/路由。
    
    ```
      function loadScript(url, callback) {
    const script = document.createElement('script')；
    script.type = 'text/javascript';
    处理IE
    if (script.readyState) {
      script.onreadystatechange = function () {
        if (script.readyState === 'loaded' || script.readyState === 'complete') {
          script.onreadystatechange = null;
          callback();
        }
      }
    } else {
      // 处理其他浏览器的情况
      script.onload = function () {
        callback();
      }
    }
    script.src = url;
    document.body.append(script);
  }

  动态加载js
  loadScript('file.js', function () {
    console.log('加载完成');
  })
    ```
    * 2.4 js文件合并压缩 ((1个100k)time<(4*25k)) 
    * 2.5 经常使用的全局变量引用放到一个局部变量里面，取的时候查找的速度快。作用域链（里到外）。
    * 2.6 减少对象成员的查找次数和嵌套深度，共用对象同一个属性时候，用变量引用一次。  
    * 2.7 dom操作优化，尽可能用局部变量存储DOM节点。   
    
    ```
      优化前，在每次循环的时候，都要获取id为t的节点，并且设置它的innerHTML
  function innerHTMLLoop () {
    for (let count = 0; count < 15000; count++) {
      document.getElementById('t').innerHTML += 'a';
    }
  }
  优化后，
  function innerHTMLLoop () {
    const tNode = document.getElemenById('t');
    const insertHtml = '';
    for (let count = 0; count < 15000; count++) {
      insertHtml += 'a';
    }
    tNode.innerHtml += insertHtml;
  }

    ```
    * 2.8 js操作dom结构时候减少重绘或者重排，最好一次渲染，减少渲染次数。  
    * 2.9 多利用事件委托，利用浏览器冒泡的原理，绑定父组件一次，就可以处理子节点的触发事件。  
    * 2.10 不断运行的代码，setInteval的性能比setTimeout要好。  
    * 2.11 数字转字符串，‘’+1效率>String()>.toString()>new String.  
    * 2.12 小数点转整形，Math.floor(2.33)/Math.round(2.33)>parseInt(2.333).  
    * 2.13 相同类型的声明，组成一个语句，减少脚本的执行时间。  
    * 2.14 字面量(速度)> new Array()/Object()/RegExp()  
    * 2.15 DocumentFragment文档碎片(虚拟节点)，可以先把目标dom结构appendChild到DocumentFragment中,然后再放入body中。大量的DOM更改，innerHTML要比标准的DOM方法快。  
    
    ```
       var fra = document.createDocumentFragment();
       for(var i = 0;i<100;i++){
           var p = document.createElement('p');
           p.innerHTML = i;
           fra.appendChild(p);
       }
       document.body.appendChild(fra);
       // 这样更好
       var HTML = [];
       for(var i = 0;i<100;i++){
           HTML.push('<p>'+i+'</p>')
       };
       document.body.innerHTML = HTML.join('');
    ```  
    * 2.16 删除dom节点时候，同时要删除监听事件，可以删除内存。removeChild和innerHTML=‘’，尽量选择innerHTML。  
    * 2.17 减值迭代。for循环从最大值中开始减值递减更加高效。  
    * 2.18 局部变量更加高效，比如：   
    ```
    for(var i = 0,len = list.length ; i < len; i++ ){
        // 保存len 可以减少计算的次数
    }
    ```  
    * 2.19 少用eval; 简单的判断用三目运算符；缩短否定检测(flag != fale)<(!flag);  
    * 2.20 避免和null对比检测;避免全局变量。  
    * 2.21 避免使用全局变量（格式 QUANJU_BIANLIAN），   
    ```  
    //全局变量外边不引用的话，
    (function(){
        var LO = null;
        function init(){
            // ....
        }
        function change(){
            // ....
        }
    }()
    // 外边引用全局遍量
    myNameSpace = function() {
        var LO = null;
        function init(){
            // ...
        }
        function change(){
            // ...
        }
        return {
            init : init,
            change:change
        }
    }
    ```  
    2.22 js创建的dom对象必须append中页面中，否则刷新页面 IE中不会回收这部分内存。  
    2.23 避免隐式装箱，性能‘stringSome’.length < new String('stringSome').length;  
    2.24 尽量使用原生方式、switch比if语句快（优雅）。  
    3.25单引号和双引号。建议html和JSON中使用双引号，js中使用单引号。  
    3.26 尽可能的多的检查输入的 数据类型（安全性和可用性）。  
    3.27 尽可能的多的使用设计模式，便于维护和扩展。  
    3.28 函数节流和去抖动。 节流是达到了5秒就执行一次，缩减执行频率。去抖是5秒内仅仅执行最后一次，如果5秒又触发了一次，从新计算5秒。  
    
* 3、html
    * 3.1 减少iframe的使用，因为iframe会增加一条http请求，阻止页面加载，即使内容为空，加载也需要时间
    * 3.2 id和class，在能看明白的基础上，简化命名，在含有关键字的连接词中连接符号用'-'，不要用'_'
    * 3.3 保持统一大小写，统一大小写有利于浏览器缓存，虽然浏览器不区分大小写，但是w3c标准为小写
    * 3.4 少量空格，清除空格。
    * 3.5 减少嵌套、语义化标签、结构化。
    * 3.6 减少注释（上线）、大量关键词影响搜索引擎。
    * 3.7 少用（没格式化控制标签）strong，b，i标签、向高版本浏览器兼容，用css控制。
* 4、react
    * 4.1  shouldComponentUpdate(nextProps,nextState),判断this.state和nextState,判断this.props和nextProps函数判断决定如何更新组件，一旦props或者state有任何变化，都会引起重新render。判断案例如下：
    react最常用的工具就是PureRenderMixin，使用 npm i react-addons-pure-render-mixin --save,安装并使用：  
    ```
   import React,{Component} from 'react'
    import PureRenderMixin from 'react-addons-pure-render-mixin'
    
    class Demo extends Component{
        constructor(props){
            super(props)
            this.shouldComponentUpdate=PureRenderMixin.shouldComponentUpdate.bind(this)
        }
        //省略其他代码....
    }
    ```  
    * 4.2 pureComponent和ImmutableJS想结合,缺点：获取组件属性必须使用get/getIn，比原生的.麻烦，immutableJs56k（体积大）；学习成本；难以调试；如果使用redux-logger,需要配置state.toJs()，pureComponent的原理就是shouldComponentUpdate。使用场景：  
    ```
    class CounterButton extends React.PureComponent {
      constructor(props) {
        super(props);
        this.state = {count: 1};
      }
    
      render() {
        return (
          <button
            color={this.props.color}
            onClick={() => this.setState(state => ({count: state.count + 1}))}>
            Count: {this.state.count}
          </button>
        );
      }
    }
    ```  
    * 4.3  组件拆分；交互少的用存函数（const），无状态操作。静态dom结构写成一个变量，引用一下。  
    * 4.4 少用{...this.props}仅仅传递需要的，太多的话加重负担；  
    * 4.5 建议bind方法放到constructor（性能最好const仅仅绑定一次）或者用箭头函数。  
    * 4.6 map里面key的不要用index（可变的），组件中的key是唯一的，最好都写上。  
    * 4.7 显示和隐藏用return null,而不是display：none。  
    * 4.8 慎用setState，和视图无关的state，不要放到state中：  
    * 4.9 redux性能优化，Selector中间件，缓存机制，可做到尽量少的存储state，  
    
    * 4.10 性能检测工具：[react-perf-tool](https://github.com/RamonGebben/react-perf-tool);官方：React.addons.Perf   
    
    
    ```
    class App extends Component {
      record = false
      onMouseDown = () => {
        this.record = true;
      }
      onMouseUp = () => {
        this.record = false;
      }
      render() {
        return (<div>
          <button onMouseDown={this.onMouseDown} onMouseUp={this.onMouseUp} />
        <div/>);
      }
    }
    ```  
    *  4.11 动态传参数可以使用闭包写法,常规写法为将父组件的函数传到子组件。  
    ```
    闭包
      class App extends Component {
        removeCharacter = index => () => {
          const { list } = this.state;
          list.splice(index, 1);
          this.setState({ list });
        }
        render() {
          return (<div>
            {this.state.list.map((value, index) =>
              <Child onClick={this.removeCharacter(index)} key={value.id} data={value} />
            )}
          </div>);
        }
      }
    ```
* 5、其它  
    * 5.1 后端部署：nginx反向代理、负载均衡、cdn
    * 5.2 缓存
     *  请求头 Expires(http1.0) 根据过期时间确定是否加载最新的版本。需要服务器和客户端时间严格同步。
     *  请求头 Cache-Control（http1.1）克服Expires头的限制。比如：Cache-Control: max-age=3600,以秒为单位，表示会被缓存60分钟
     *  响应体 Last-Modified,服务器发给浏览器时候带上了Last-Modified或Etag数据，当浏览器再次要这些东西时候，如果是新的则返回304代码，告诉浏览器直接使用本地代码否则下载最新的版本（多用于静态文件）。
     *  get请求加个时间撮或者hash值。     
     *  响应体 Last-Modified,服务器发给浏览器时候带上了Last-Modified或Etag数据，当浏览器再次要这些东西时候，如果是新的则返回304代码，告诉浏览器直接使用本地代码否则下载最新的版本（多用于静态文件）。
    * 5.3 算法；谷歌浏览器；和后端约定
    * 5.4 高性能的服务器。
## 二、webpack ##
* 1、webpack常用配置：
    
    * webpack2：
    * webpack3：
    * webpack4：
* 2、webpack 简单原理： 
    * 简单思路：wepack把项目当做一个整体，通过一个给定的主文件，它将从这个文件开始找本项目所有的依赖文件，通过loaders处理它们，最后打包成一个或多个浏览器可识别的js文件。
    * entry: 单个文件是字符串、多个文件是个对象。
    * output: 
    ```
    output: {
        filename: '[name].js',
        path: path.resolve('./build')
    }
    ```
    * loader: 实现不同格式文件的处理，需要写到module中
    ```
    var baseConfig = {
                entry: {
                    main: './src/index.js'
                },
                output: {
                    filename: '[name].js',
                    path: path.resolve('./build')
                },
                devServer: {
                    contentBase: './src',
                    historyApiFallBack: true,
                    inline: true
                },
                module: {
                    rules: [
                        {
                            test: /\.less$/,
                            use: [
                                {loader: 'style-loader'},
                                {loader: 'css-loader'},
                                {loader: 'less-loader'}
                            ],
                            exclude: /node_modules/
                        }
                    ]
                }
            }
    ```
    * Plugins : 他直接对整个构建过程起作用，比如：分离css ,html       
    
    ```
    var ExtractTextPlugin = require('extract-text-webpack-plugin');
    var HTMLWebpackPlugin = require('html-webpack-plugin')
            var lessRules = {
            use: [
                {loader: 'css-loader'},
                {loader: 'less-loader'}
            ]
        }

        var baseConfig = {
            // ... 
            module: {
                rules: [
                    // ...
                    {test: /\.less$/, use: ExtractTextPlugin.extract(lessRules)}
                ]
            },
            plugins: [
                new ExtractTextPlugin('main.css')，
                new HTMLWebpackPlugin()
            ]
        }
    
    ```
    * webpack-dev-server：使用内存来存储webpack开发环境下的打包文件，比其它服务器快，还可以热更新（自动刷新）
