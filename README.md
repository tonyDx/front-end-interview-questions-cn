# 前端面试指南

<!--more-->

<p align="center">
<img alt="FrontEndInterviewQuestions" src="https://raw.githubusercontent.com/tonyDx/front-end-interview-questions-cn/master/assets/main.jpg">
</p>


#### jq
1. jq中有几种绑定事件的方式
    - bind delegate live on
2. jq初始化几种方式
    - 三种
3. winodw.onload  和 $(document).ready(function(){}) 的区别
    - $(docuemnt).ready()的本质是 document.addeventlisrener("DOMcontentLoaded",function(){},false)
    - onload - DOMcontentLoaded  这两个事件的区别
    - 先触发 DOMContentLoaded(html+js+css)  可以绑定多次
    - 后触发 load 事件只能绑定一次
4. jq的特点
    - 无new构造，链式操作，通过立即执行函数形成闭包封闭作作用域
    - jq轻量级的js框架，30kb，强大的选择器，出色的dom操作
    - 可靠的事件处理机制
    - 完善的ajax 不需要考虑复杂的浏览器的兼容性
    - 支持链式操作，隐式迭代，行为层和样式层想分离，还支持丰富的插件，文档也是非常的丰富
    - 图片工具 数据格式化  说的是那个很厉害的图表工具
####  网络部分
1. 从输入一个url发生了那些事情
    1. 浏览器通过DNS域名解析到服务IP（ping www.baidu.com）
    2. 客户端(浏览器)通过TCP协议建立到服务器的TCP连接  (三次握手)
    3. 客户端（浏览器）向web服务器端（HTTP服务器）发送HTTP协议包，请求服务器里的资源文档 （telnet 模拟）
    4. 服务器向客户端发送HTTP协议应答包
    5. 客户端和服务器断开（四次挥手），客户端开始解释处理HTML文档
    2. 有没有自己封装过ajax,ajax步骤（类似于一个定外卖的过程）
    1. 步骤
    1. 创建xmlhttprequest对象(要考虑兼容性)
    2. 创建http request请求，并且要指定，请求的方法，url,post请求需要设置httprequestheader,url xhr.open
    3. 发送http请求
    4. 使用xhr.onreadystatechange方法来监听readyState状态  304 200
    5. 获取数据，responseText 获取数据
    6. 使用js为dom提供的api 实现局部刷新
3. 同源策略
    1. 浏览器有一个重要的概念 同源策略
       1. 同源值得是，域名，协议，端口相同，不同源的客户端脚本在没有明确的授权下，不能读写对方的资源
       2. https://www.baidu.com:443 协议 域名 端口号  ajax受同源策略的限制
       3. https://zhidao.baidu.com:443  三级域名   他属于百度自己维护的网络地址
       4. 顶级域名.cn 中国在国际互联网中心，正式注册的顶级域名是.cn
       5. www是二级域名的前缀，历史问题，尊重用户的习惯
       6. www网页服务，ftp文件传输服务，telnet远程登录，之前用www来区分网页服务
       7. https:baidu.com 这个域名叫裸域名 裸域名只能绑定dns A记录 不能绑定cname记录
          A记录：域名对应的ip地址
          cname记录：边名记录  你输入baidu.com不加www也能访问到百度，这是因为起来别名
          裸域：cookie作用域大
       8. dns是倒着解析的，.com ->baidu.com->zhidao.baidu.com 最后的到一个绝对的ip地址
4. tcp协议
    1. TCP（Transmission Control Protocol，传输控制协议）是基于连接的协议，也就是说，在正式收发数据前，必须和对方建立可靠的连接。一个TCP连接必须要经过三次“对话”才能建立起来
    2. 三次握手
       1. client发送syn=client的数据包请求报文，这个客户端处于send状态
       2. Server端接受连接后，回复ACK = syn报文内容clent+1，并为这次连接分配资源。同时还发送了一个syn数据包
       3. Client端接收到ACK报文后,同时也回复Server端发送syn报文，在回复一个ack包，这样TCP连接就建立了
       4. 报文是客户端和服务器的规定的一种数据传输格式
    3. 四次挥手
       1. Client端发起中断连接请求，也就是发送FIN报文。Server端接到FIN报文后，意思是说"我Client端没有数据要发给你了"，但是如果你还有数据没有发送完成，则不必急着关闭（Socket），可以继续发送数据。
       2. server发送ACK，"告诉Client端，你的请求我收到了，但是我还没准备好，请继续等我的消息"。wait:这个时候Client端就进入FIN_WAIT状态，继续等待Server端的FIN报文。
       3. 当Server端确定数据已发送完成，则向Client端发送FIN报文，"告诉Client端，好了，我这边数据发完了，准备好关闭连接了"。
       4. Client端收到FIN报文后，"就知道可以关闭连接了，但是他还是不相信网络，怕Server端不知道要关闭，所以发送ACK后进入TIME_WAIT状态，如果Server端没有收到ACK则可以重传。“，Server端收到ACK后，"就知道可以断开连接了"。Client端等待了2MSL后依然没有收到回复，则证明Server端已正常关闭，那好，我Client端也可以关闭连接了。Ok，TCP连接就这样关闭了！
    4. udp协议 面向非链接协议 不需要建立连接，直接把数据包发送过去   微信聊天   tcp先的加好友  不加好友直接发，容易丢包，实事语言，视屏聊天
5. 报文（请求报文，和响应报文）http1.1 支持长连接，在一定的时间内，连接不会断开
   1. 请求报文
      请求行：请求方法，请求资源，协议版本
      请求头：accept-language,content-length,cach-control,cookie,请求的属性设置，referer
      请求主体：数据参数
   2. 响应报文
      响应头：协议版本号，状态码，消息短语,ser-cookie
      响应行：
      响应主体
   3. 2XX:成功
      3XX重定向 ：重新定向到浏览器的缓存中，或者重新定向到其他的服务器资源301 302 304
      4XX客户端错误 403静止访问 404
      5xx服务器错误 502 503  get是通过url传参或者cookie传参的
   4. 强缓存和协商缓存
      - 强缓存  cash-control
      - 协商缓存 请求头中字段：if-none-match 配合 etag
      - if-modify-since配合   last-modify使用 一秒修改了很多次，所以要配合etage
      - cache-controll  max-age  no-cache
      - expires  和cache-cotrol 同事从在   cache-control权重大
      - referer   从哪个网站点过来的   地址栏直接输入没有referer
6. 跨域(ajax受同源策略的限制)
   1. 服务器代理中转
   2. jsonp get请求
      - jsonp跨域原理：
        我定义一个回调函数的形式传给后台，后台以函数执行的形式的在传递回来
        1.  Web页面上用script引入 js文件时则不受是否跨域的影响（不仅如此，我们还发现凡是拥有"src"这个属性的标签都拥有跨域的能力，比如<script>、<img>、<iframe>）
        2. 利用这个特点呢，我们可以动态的创建script标签，跨域加载js文件
        3. 但是我们无法监控通过<script>的src属性是否把数据获取完成，所以我们需要做一个处理。
        4. 提前定义好处理跨域获取数据的函数，如 function doJSON（data）{}。
        5. 用src获取数据的时候添加一个参数cb=‘doJSON’ (服务端会根据参数cb的值返回 对应的内容)
        6. 加载完成之后，执行回调函数，并且把要获取的数据，作为参数传入回调函数当中
        7. 总之就是利用了函数不调用不执行的特点
   3. document.domain(针对基础域名相同的情况tj.58.com,bj.58.com)
   4. cros跨域  服务端进行设置
      - Access-control-Allow-Origin 跨域资源共享
   5. ifream 跨域  做广告 tab切换，在线编辑器（w3c亲知试一试）异步的获取的
      - 阻塞页面，动态的添加ifream标签
      - 解决跨域问题  借助一些手段来实现跨域
        - 借助hash loaction.hash  来实现跨域   取值的时候要借助定时器  只能是父向子页面传递
        - window.name  直接把name刻在了窗口上，只要窗口不关闭 就能实现 子向父巧妙跨域
        - 不同源，不能获取到当前window,想办法变同源
        - 具体过程就是 子现在window.name上 写上要传的值
        - 父判断iframe加载完成之后 改变iframe的src属性 派个人过去看看  完后再告诉自己 两口子吵架的一个过程
        - 通过 iframe.contentWinndow获取到这个值
7. cookie
    1. http协议是一种无状态协议，所以需要一种跟踪用户的会话技术，cooike横空出世
       - HTTP协议是无状态的协议。一旦数据交换完毕，客户端与服务器端的连接就会关闭，再次交换数据需要建立新的连接。这就意味着服务器无法从连接上跟踪会话。
    2. cookie服务器端生成，客户端设置，下次请求的时候带上，cookie的大小是有限制的4k，cookie受同源策略的限制
       - form email  承载用户信息http请求头首部
       - user-agent
       - referer
       - 客户端的ip地址 tcp连接可以获取到ip地址，tcp连接的另一头，ip地址标记的是机器，不是用户，ip地址不稳定，
         - 还有处于安全的考虑，把真是的ip地址伪装。
       - 用户登录  每个网站都需要注册登录的流程
       - 胖url 在url上 网站给我随机分配唯一标识  对服务器产生额外的负载，不能共享，声明周期页面关闭也不行了
       - cookie 放在本地的一个txt文件 临时cookie 页面关了就销毁了
       - domain key value path expires size
         - 只要在这个域名下子域的都能拿到这个cookie
         - expires 为session 代表这条cookie是临时的
8. json和jsonp分别代表什么？有什么区别
   - json就是一种数据格式，jsonp是一种实现跨域的方式
9. 浏览器缓存手段
10. xss和csrf有啥区别
11. https  = http+加密+认证+完整性保护
   - sever端生成一个公钥和私钥，把公钥给了第三方认证机构ca
   - ca对公钥进行md5加密，生成数字签名，在把数字签名用ca的私钥加密，生成数字证书，把这个证书返回给sever
   - sever拿到证书之后，发送给浏览器
   - 浏览器对数字证书进行验证，首先，浏览器本身会内置ca的公钥，用公钥对数字证书健米，验证这个ca生成的数字证书是否受信任
   - 成功之后，浏览器随机生成对称秘钥，用sever的公钥加密这个对称秘钥，再把加密的对称秘钥传送给sever
   - sever 收到对称秘钥，用自己的私钥进行解密，之后，他们直接的通信就用这个对称秘钥进行加密，来维持通信；
12. 七层网络模型：应用层  ->  计算机设备  tcp http
                 表示层
                 会话层
                 传输层  ->防火墙
                 网络层  ->路由器
                 数据链路层 交换机
                 物理层  -> 网卡
#### css3部分
1. css元素的选择器有哪些

2. 为元素和伪类选择器的区别
   - 元素他是一个真实元素
3. 浏览器渲染过程

4. 浏览器渲染模式 标准模式和怪异模式

5. flex布局，双飞翼布局，三栏布局，圣杯布局

6. css新增了那些api，以及作用
    - 选择器部分

    - 边框部分
       -  边框可以添加背景图片   border-image
       -  border-radius
       -  box-shadow

    - css背景部分
       -  background-origin  背景图片的其实位置 默认从border-box开始
       -  background-clip    padding-box content-box
       -  background-size    cover 覆盖整个区域  contain 按照比例扩大或者缩小图片，放下整个图片 背景图片的大小
       -  background-position (css2)

    - 文本和颜色
       -  text-shadow
       -  文本换行 word-break
       -          white-space  pre保留所有空格
       -  超出打点
        overflow hidden
        white-space:nowrap
        text-overdlow:ellipsis
       -  自定义字体 @font-face font-family
       -  颜色 rgba()
       -  hsl
       -  渐变色 gradient  线性渐变，和径向渐变
       -  transparent  做三角形

    - 盒模型
        - w3c标准盒模型   把所有的html元素都当做一个盒子来处理
          - element空间高度（盒子） = width + padding + border;
          - width 为内容高度。即width不包括padding 和 border
        - ie混杂盒模型   他们都是对元素计算尺寸的模型。但他们的不同是计算的方式不同。
          - 内容高度 （盒子）= width - padding - border
          - 即 设置width的数值就是element 的空间高度，width包含padding 和border
          - box-sizing : border-box/content-box

    - 布局
         - 多列布局
         - flex 布局
         - 响应式布局 媒体查询
           - rem 是css3新增的一个相对单位，相对的是html根元素
           - px 是相对于屏幕分辨率而言的
           - em 相对长度单位，相对于父级文本的字体而言
           - device-width/height 是设备的宽度（如电脑手机的宽度 不是浏览器的宽度）
           - width/height使用documentElement.clientWidth/Height即viewport的值。

    - 像素
        - 物理像素 最大分辨率---设备生产厂商定义的 1920*1080
        - 实际分辨率：自己设置电脑的分辨率
        - css像素 视觉上看到的像素，感官上的像素，浏览器厂商定义的（变化的像素，使用参考像素做一个换算）
        - 设备独立像素：操作系统定义的，和实际分辨率是一比一的关系（上边说的参考像素）
        - 总结：参考像素，根据设备独立像素做了一层调节,css像素根据参考像素进行了调节
        - 设备像素：设备像素比 = 实际分别率/css像素 = 物理像素/实际分辨率 = 物理像素
        - 并不是所有的设备像素比都是一比一的关系 iphone就是2比一的关系
        - 位图像素 图片  300*300为例 每一个像素，就叫一个位图像素
        - window.devicePixelRatio 可以查看设备像素比
        - 普通的分辨率屏幕：一个css像素对应一个位图像素
        - 视网膜屏幕分辨率：一个css像素对应4个位图像素 由于一个位图像素已经是最小比例，不能再分割，就近取色
         多以导致模糊，解决办法，分辨率增加一倍

    - transform 元素过渡的效果 只有开始位置和结束位置的时候用transition ，属性发生变化的时候才触发
    - animation 动画的效果  关注中间的状态 倾向的是一个过程 用animation  设置animation的时候刘触发了
7. 垂直居中有多少种方式
   - 定宽高
   1. position + margin
   - 不定宽高
   2. position + transform:translate(-50%)
   - 子元素居中
   3. flex
   - justify-content:center + align-items:center
8. bfc
####   框架
1. mvc,mvp,mvvm
   - 他们都是常见的软件设计模式，通过分离 关注点  来改改进代码的组织方式
   - 相同点就
    - 是mv(model-view)，mv,不同点Ccontroller,vm(view-model)，p(presenter)
    - 只是为了解决一类问题而总结出的抽象方法，一种架构模式往往使用了多种设计模式。
    - model：于封装  和应用程序的业务逻辑相关的数据  以及 对数据的处理方法
    - view: View作为视图层，主要负责数据的展示
    - 有了 model,和 view 就完成了 --> 数据从模型层到视图层的逻辑
    - 但是对于一个应用程序，他还需要--> 响应用户的操作、同步更新View和Model
      - mvc 这种架构模式引入controller
        - 让它来定义界面和用户输入的响应方式，它连接模型和视图，用于控制应用程序的流程，处理用户的行为和数据上的改变。
        - controller响应view的事件，调用model对外接口，修改数据
        - 一旦Model数据发生变化便通知相关视图进行更新。

        - Model和View之间使用了观察者模式，View事先在此Model上注册，进而观察Model，以便更新在Model上发生改变的数据。

        - controll和view使用了策略模式，---->  实现不同的响应的策略只要用不同的Controller实例替换即可
        -
     -
       1. 可以明显感觉到，MVC模式的业务逻辑主要集中在Controller,
       2. 而前端的View其实已经具备了独立处理用户事件的能力，
       3. 当每个事件都流经Controller时，这层会变得十分臃肿。
       4. 而且MVC中View和Controller一般是一一对应的，
       5. 捆绑起来表示一个组件，视图与控制器间的过于紧密的连接让Controller的复用性成了问题.

    - mvp 中的p横空出世
      - 是MVC模式的改良，由IBM的一个子公司提出的
      - MVP中的View并不能直接使用Model，而是通过为Presenter提供接口，让Presenter去更新Model，再通过观察者模式更新View。
      - MVP模式通过解耦View和Model，完全分离视图和模型使职责划分更加清晰；由于View不依赖Model，可以将View抽离出来做成组件
      - Presenter作为View和Model之间的 "中间人" ，除了基本的业务逻辑外，还有大量代码需要对从
      - View到Model和 从Model到View的数据进行  "手动同步" ，
      - 这样Presenter显得很重，维护起来会比较困难。
    - mvvm
     -  最早微软提出，抄的非常的火，vue,和ag
     -  MVVM把View和Model的同步逻辑自动化了,以前是手动更新
     -  MVVM中的View通过使用模板语法来声明式的将数据渲染进DOM，当ViewModel对Model进行更新的时候，会通过数据绑定更新到View
     - 在MVVM中，View不知道Model的存在，ViewModel和Model也察觉不到View，这种低耦合模式可以使开发过程更加容易，提高应用的可重用性。
     - 在Vue中，使用了双向绑定技术（Two-Way-Data-Binding），就是View的变化能实时让Model发生变化，而Model的变化也能实时更新到View。
     - 数据劫持 (Vue)
     - 发布-订阅模式 (Knockout、Backbone)
     - 脏值检查 (Angular)
     - 通过ES5提供的 Object.defineProperty() 方法来劫持（监控）各属性的 getter 、setter ，并在数据（对象）发生变动时通知订阅者，触发相应的监听回调。并且，由于是在不同的数据上触发同步，可以精确的将变更发送给绑定的视图，而不是对所有的数据都执行一次检测。
     - 要实现Vue中的双向数据绑定，大致可以划分三个模块：Observer、Compile、Watcher
     - Observer 数据监听器 负责对数据对象的所有属性进行监听（数据劫持），监听到数据发生变化后通知订阅者。
     - Compiler 指令解析器 扫描模板，并对指令进行解析，然后绑定指定事件。
     - Watcher 订阅者
     - MV*的目的是把应用程序的数据、业务逻辑和界面这三块解耦，分离关注点，不仅利于团队协作和测试，更有利于维护和管理。业务逻辑不再关心底层数据的读写，而这些数据又以对象的形式呈现给业务逻辑层。从 MVC --> MVP --> MVVM，就像一个打怪升级的过程，它们都是在MVC的基础上随着时代和应用环境的发展衍变而来的。
2. vue 和 react 的区别
   1. 关注点：专注于创造前端的富应用。React与Vue只有骨架，值关注框架本身，其他的功能如路由、状态管理等是 ，框架分离的组件。
   2. 数据绑定：vue双向数据绑定，react没有双向数据绑定。
      - 插值表达式
      - vuejs 给我们提供了一系列的指令，v-on,v-model，v-if(12个指令)
      - 事件处理这一块：通过v-on给元素注册事件，当一个组件被销毁时候所有的事件处理都会自动被删除。你无须担心如何自己清理它们。
      - 计算属性的概念
   3. 组件化和数据流
      - React是单向数据流，数据主要从父节点传递到子节点（通过props）。如果顶层（父级）的某个props改变了，React会重渲染所有的子节点。
      - react中实现组件有两种实现方式，一种是function compomtent方法，另一种是通过ES2015的思想类继承class compontent来实现
      - react组件可以将UI切分成一些的独立的、可复用的部件，这样你就只需专注于构建每一个单独的部件。它可以接收任意的输入值（称之为“props”），并返回一个需要在页面上展示的React元素。
      -  组件之间的通信
         - 父与子之间通props属性进行传递
         - 子与父之间，父组件定义事件，子组件触发父组件中的事件时，通过实参的形式来改变父组件中的数据来通信
         - 组件的生命周期
         contructor->compontenwillmount->render->cpmpontentdidmount
         父组件的props改变->compontentwillreceiveprops 这里不能this.setState({}) ->shouldcompontentupdate->compontentwillupdate->render->compomrentdidmount->compontentunmount
      - 组件系统是 Vue 的另一个重要概念，因为它是一种抽象，允许我们使用小型、独立和通常可复用的组件构建大型应用。
      - 当一个 Vue 实例被创建时，它向 Vue 的响应式系统中加入了其 data 对象中能找到的所有的属性。当这些属性的值发生改变时，视图将会产生“响应”，即匹配更新为新的值。当这些数据改变时，视图会进行重渲染。值得注意的是只有当实例被创建时 data 中存在的属性是响应式的。
      - newVue()->init阶段 beforecreated->这个时候千万不要操作data数据injected和reactivity->created->完成之后->判断实例当中有没有el,option这个选项，有的话->vm.$mount(el)
        ->template,这个选项 有的话解析成一个 template-render  ->beforemount ->render->mounted
      - 后续的钩子函数执行的过程都是需要外部的触发才会执行。比如说有数据的变化，会调用beforeUpdate，然后经过Virtual DOM，最后updated更新完毕。当组件被销毁的时候，它会调用beforeDestory，以及destoryed。
      - 组件之间的通信
        - props 向下，events 向上
    4. 组件之间层级太多的时候，需要状态管理，
       - react flux 和redux
       - 创建action 创建store,创建dispatcher view调用action
       - vue  vuex
       - Mutation 变化
       - Action
       - Module
    5. 渲染性能上
       - react试图渲染   借用一种在内存中描述 DOM 树状态的数据结构   比较计算之后给真实 DOM 打补丁
       - Vue 通过建立一个虚拟 DOM 对真实 DOM 发生的变化保持追踪
    6. 思想
      - Vue 的整体思想是拥抱经典的 Web 技术，并在其上进行扩展。
      - JSX
    7. 服务器端渲染（SSR）
       - 客户端渲染路线：1. 请求一个html -> 2. 服务端返回一个html -> 3. 浏览器下载html里面的js/css文件 -> 4. 等待js文件下载完成 -> 5. 等待js加载并初始化完成 -> 6. js代码终于可以运行，由js代码向后端请求数据( ajax/fetch ) -> 7. 等待后端数据返回 -> 8. react-dom( 客户端 )从无到完整地，把数据渲染为响应页面
      - 服务端渲染路线：1. 请求一个html -> 2. 服务端请求数据( 内网请求快 ) -> 3. 服务器初始渲染（服务端性能好，较快） -> 4. 服务端返回已经有正确内容的页面 -> 5. 客户端请求js/css文件 -> 6. 等待js文件下载完成 -> 7. 等待js加载并初始化完成 -> 8. react-dom( 客户端 )把剩下一部分渲染完成( 内容小，渲染快 )



3. this.setstate() 发生了什么
  - 会触发一次组件的更新过程，从而引发页面的从新绘制 设计react 四个生命周期函数
4. react router原理
   - 控制不同的url 渲染不同的组件
   - 点击链接->改变hash->触发window.onhashchage事件->改变组建中state中的route属性，react state发生改变，自动渲染页面
5. react diff算法
  - diffchildren,diffprops,就是比较节点的增删改查，完后把改变的信息记录到diffinfo中，打补丁到真实的dom 上  深度优先遍历的原则
### 模块化
 1. 什么是模块化
   - 模块化是讲一个复杂的系统拆分成多个模块，方便团队开发
   - 为什么要使用模块化
     - 降低复杂性，降低代码的耦合度，部署方便，提高团队开发效率
   - 模块化的好处
     - 避免命名冲突，减少变量空间污染
     - 更好的分离代码，按需加载
     - 提高代码的可复用性，可维护性
   - AMD依赖前置，提前加载依赖。而CMD就近加载，按需加载。
 2. 什么是前端工程化
   - 工程化是更具业务特点，讲前端开发流程规范化，标准化，这就包括，开发流程，技术选型，代码规范，构建发布等，用于提升前端开发工程师的开发效率，和提升代码质量。也就是上边的这些事情交给工具去处理 如代码规范 可以用eslint，构建发布，用webpack,glup
 3. webpack自动刷新实时预览的原理
   - 当文件内容发生变化的时候，首先重新构建，完后通知webpack-dev-server,sever会在webpack构建出的代码中注入代理客户端，客户端来控制网页的刷新  websocket通信
 3. webpack 工作原理
  - webpack的运行流程是一个串行的过程
    1. 初始化参数，从配置文件中或者shell语句中读取合并参数，得到最终参数
    2. 开始编译，用上一步得到的参数 初始化 compiler对象，加载所有配置的插件，执行对象的run方法开始执行编译
    3. 根据配置文件中的entry 找到所有的入口文件
    4. 从入口文件出发，调用所有配置的loader对模块进行翻译，在找出该模块所依赖的模块，在递归本步骤  一直到所有的入口依赖的文件都经过本步骤的处理
    5. 翻译完所有模块，得到每个模块 被翻译后的最终内容 和 他们之间的关系
    6. 根据关系，组成一个个包含多个模块的chunk, 在将一个个chunk转换成一个个单独文件，加入输出列表中，这是可以修改内容的最后机会
    7. 在确定好输出内容偶，根据配置确定输出路径和文件名，将文件的内容写入系统中
####  js部分
1. js执行机制
   - js一大特点：js是一门单线程的语言，eventloop是js的执行机制
   - 当我们执行一个js脚本的时候
       - macro-task(宏任务)：整体的代码script,settimeout，setinterval,
       - micro-task(微任务)：promise,promise.nextTick
     - 事件循环的顺序是：
       1. 整体的一个script作为宏任务进入主线程，遇到settimeout，将settimeout放到event table中 并且将其回调函数注册后  放到宏任务 event queue
       2. 接着往下执行，遇到promise.then   先放到event table 中，并且将then函数注册后分发到微任务 event queue 中
       3. 当整体的代码作为一个宏任务执行完毕之后，先执行微任务队列当中的任务(一个个拿出来)  eventloop的第一轮循环，一直到 微任务当中的任务执行完毕
       4. 紧接着开始第二轮循环，先从宏任务event queue开始
2. 浏览器的内核有哪些
   - ie       trident
   - 火狐     gecko   干沟
   - opera    以前用的  presto  新版本的用的  blink
   - 谷歌和 Safari   用的webkit   blink

3. 深拷贝怎么实现
   - JSON.parse(JSON.stringify(arr))
   - jq 的 extend
   - 手撕一个
4. 前端代码被压缩，如何找到对应的元素
  - source下找到对应的js脚本，下边会有个{}的按钮，点击可以将js脚本格式化
5.
