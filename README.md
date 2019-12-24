# 前端知识点
  本文偏向于react+redux+redux-saga。包含 js基础，react基础，redux数据流。

## 一、跨域。

1.**jsonp**。通过script 标签的 src 属性 结尾添加 `？callback= callback` ,服务器接收请求时 将 数据包含在callback函数中。然后通过函数执行获取跨域数据。 script 标签有一个 defer 和async 的属性，都是让script 异步加载，不阻塞页面的渲染。但是 多个 具有defer属性的script 的执行顺序是一定的，并且执行时会阻塞后续defer 的script执行。 但是async 不能保证执行的顺序。而是谁加载完谁先执行。使用时，如果涉及页面DOm操作，则使用defer，如果不涉及页面操作，则使用async。
	
2.**服务端配置。

3.**cors 跨域资源。

4.**proxy** ，服务器代理。

5.**window.domin + iframe** 。 iframe 的缺点是 会阻塞后续资源的load，一般通过 动态添加 iframe 的src属性来解决。

## 二、react diff算法。vue Object.defineProperty

react 的diff算法，通过深度优先 比较 virtual dom。root diff ,component diff , element diff.
	
vue Object.defineProperty(obj,obj.data,{set:function(){},get:function(){}}) 双向数据绑定
	
### 三、css3 flex 布局.
	
1.父元素设置 `display：flex`。 

行内元素也可以使用flex布局，`display：inline-flex； display：-webkit-flex;`使用flex布局之后，子元素的`float，clear，vertical-align` 属性将失效。
	
2.容器上有多个属性 

`flex-direction` 主轴方向  row/row-reverse/column/ column-reverse; 默认为row,一般使用 row 和column 两个选项。
	
`flex-wrap` 是否换行  `nowrap` 不换行 `wrap` 换行，第一行在上方 ，`wrap-reverse` 换行，第一行在下边。 默认为 不换行
	
`flex-flow`  direction 和 wrap 的组合写法  flex-flow ：flex-direction flex-wrap。
		
`justify-content` 定义主轴上的对齐方式。 flex-start 上侧或左侧 ，flex-end 下册或右侧 ，center 中间 ，space-between  相邻元素之间的距离相等，两边为0.space-around 所有环绕间隔都相等，包括元素与边界的距离。item之间的距离比item与边框的距离大一倍。

`align-items` 在与主轴交叉的轴上如何对齐。交叉轴上的 flex-start flex-end center baseline stretch. 默认为 stretch。stretch没设置高度或宽度的前提下，将占满整个交叉轴的物理长度。
		
`align-content` 在多条主轴上的对齐方式。如果只有一根轴线，则不起作用。flex-start flex-end center stretch space-between space-around。stretch没设置高度的前提下，占据整个交叉轴。
 
 3.子元素设置flex 值
 
`order` 项目的排列顺序，数值越小，排列越靠前，默认为0.
	
`flex-grow` 定义项目的放大比例，默认为0，即使存在剩余空间，也不放大。
	
`flex-shrink` 定义项目的缩小比例。默认为1，如果空间不足，该项目缩小。所有都为1时，所有的项目都将等比例缩小。
	
`flex-basis` 在分配多余剩余空间之前，项目占据的主轴空间。默认为auto。即项目的本来大小，一般不设定。
	
`flex` 为 flex-grow flex-shrink flex-basis 三个属性的缩写。默认值为 0 1 auto； 后面两个可选。	
	
`align-self`  允许单个项目有与其他项目不一样的对齐方式 可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。	

### 四、基础算法。

1.快速排序 O(nlogn)。从中间选择一个数作为基准，然后将小于这个数的放在前边，大于这个数的放后边。然后合并为新的数组。尾递归。
	
2.堆排序。

3.归并排序。

4.二分查找。从中间选择一个数作为基准，如果基准小于要查找的数，则从这个基准的位置开始到结尾再次查询。一直到找到这个数。

5.线性查找。

6.DFS 深度优先搜索。

7.BFS 广度优先搜索。

8.Dijkstra 算法。

9.动态规划。将复杂的问题简化成一个一个的小问题，然后对这些小问题求解，然后整合起来求解。最经典的问题是背包问题。

10.朴素贝叶斯分类算法。

### 五、回流和重绘。

浏览器渲染DOM。 **边渲染边下载。 css解析和html 解析是同步的。

1. 浏览器解析html，`DOM Tree` 生成Dom树，包括display为none或者head标签中 的节点。
2. `CSS Tree` 解析css，根据css从右向左依次构建css tree。所有的css都是阻塞渲染的。不包括display：none和head标签中的节点。
将 DOM Tree 和 CSS Tree 结合成 Render Tree。不包括display：none和head标签中的节点。 firefox 是gekco 引擎， chrome 和 safari 以webkit 引擎为主。blink是基于webkit引擎的一个分支。
3.在 rendertree 的基础上开始绘制 包括 color。outline 等样式。
**回流  reflow
	当 render 树种的一部分或者全部因为大小边距等问题发生改变而需要重建的过程叫做 回流。
**重绘 repaint
	当元素的一部分属性发生变化，如 外观背景色 不会引起布局变化而需要重新渲染的过程叫做重绘。

浏览器有优化机制，可以将多次回流重绘放在一个队列中，当队列达到一定的数量，浏览器就会flush队列，做一个批处理，这样就将多次的回流、重绘变成一次回流重绘。

引起回流重绘的操作

1.请求 offset属性 top，left，height，width

2.scrollTop/Left/Width/Height

3.clientTop/Left/Width/Height

4.width,height

5.getComputedStyle  和 currentStyle 属性 获取 css文件中的样式。
	
当获取这些操作时，浏览器为了给你一个精确地答案，会提前 flush 队列，以避免队列中有影响到答案的因素存在。

优化

1.将多次的style 操作 合并为一次 或者改为 toggleClass,减少回流重绘次数。
	
2.使用 DocumentFragment 进行缓存操作，只引发一次回流重绘。
	
`DocumentFragment` 是一个占位符，当把这个节点插入文档树时，插入的不是他本身，而是他所有的子孙节点。引发一次回流重绘。文档碎片。document.body.appendChild() 参数只能是一个Node。
所以 使用createElement 一次插入 一个Fragment 标签就行了。性能最大化。

3.尽量不使用表格布局，如果表格没有定宽，则表格的宽度由最宽的一列决定，那么很可能最后一行的width超出之前的列宽，引起整个table 的多次回流重绘才能计算好其在渲染树中节点的属性，通常要花三倍于同等元素的时间。


### 六、js中this 的指向。

原生函数 的this指向 window。

对象的方法 中的this 指向 对象。

构造函数的this指向对象实例。

箭头函数 作为对象方法的时候，this 指向 window。作为回调函数时，this指向你期望的那个对象。

1.object中的this 指向自身。

2.函数中的this 执行调用者，如果有多个调用者，则指向距离函数最近的调用者对象。

3.可以用 apply，call ，bind 改变函数中的this指向。apply，call 的参数不同，都是在运行时绑定this，bind 则是在定义时绑定this。之后可以在其他地方使用时不改变this指向。

4.如果在 window 环境中执行，则this 指向window，严格模式下指向 undefined。

5.call ，apply ，bind 可以改变 函数的this指向，可将this指向传入的对象。区别不同的是

**call 和apply 的异点		

1.call 和 apply 的参数不同，call 的参数用逗号隔开，apply 的参数用数组包括。
		
2.call 的执行速度永远比apply 的速度要快。在safari 的浏览器里差别不是很大，但在chrome浏览器中差别巨大，apply 的执行速度比call慢10倍。

提高效率就是 用call来模拟apply，使用 switch case 来判断apply参数的个数，然后使用 call（obj，args[0],args[1]）。

### slice、substr 和 sunstring

1.`substring`（index1，index2）index1是从第index1位开始截取，然后截取到第index2 位之前结束。sunstring 以参数中较小的一个作为起始位置，如果为负数，则直接转换为0。

2.`substr`（index1，index2） index1 是从第index1位开始截取，然后 截取index2的字符，如果为负数，则只将第一个参数与字符串的长度相加。

3.`slice` 和 `substring`功能一样，只是如果参数是负数，则将负数和字符串的长度相加，然后向右。
	
### 七、强缓存和协商缓存

1.**强缓存： 
		
http/1.0 使用 expires 设置过期时间
	
http/1.1 使用 cache-control  只有 no-cache，no-store，max-age ，public，private。
	
当expires 和 cache-control都存在的情况下，cache-control的优先级高于expires.
	
2.**协商缓存：
		
`last-modifield/if-modifield-since``Etag/if-None-Match `会在服务器第一次返回数据时通过header 返回缓存信息。
		
强缓存的状态码为 200， 协商缓存的状态码为 304.
	
强缓存直接从缓存中取，协商缓存需要和服务器通信协商，告知缓存是否可用。
	
### 八 、 redux-saga 引入  redux-saga/effects

1.put 派发 action
	
2.call 执行 函数，并解析结果，同步，返回纯文本对象的函数，适合返回promise结果的函数。

3.apply 参数为 数组，同步，和call功能相同，适合返回promise结果的函数。

4.cps 则 适合 回调函数类型的函数。

5.fork 执行函数解析结果，异步。返回一个task，可以用于 cancel取消

6.takeLatest 监听，多次触发则执行最新的。
	
7.take 监听多个，参数为数组，同步阻塞。

8.cancel 取消fork 异步操作。

9.all , race 多个任务并行执行。all 会在 所有任务都执行完后结束，如果有错误的，则取消所有操作，race 会在第一个任务执行成功之后，取消剩下的所有任务。

### 九、 react setState  

1.setState 是一个异步操作。

2.如果在一个函数中多次执行setState ，则最好是 将一个返回 object 的函数传入 setState，否则setState 会将所有的操作合并成一个 state，执行一次，这样就会忽略掉最后一个之前的操作，传入的函数的参数为两个 prevState，props。

3.setState 更新数据是一个 重新渲染组件的过程，触发重绘，即使state并没有什么变化，但只要调用setState就要重绘render，所以为了避免不必要的更新，要在shouldComponentUpdate 中判断是否需要重新渲染。。

4.setState 可以使用第二个参数，即传入一个函数，来作为setState 的一个state更新成功后的回调函数，在函数中可以使用更新后的参数。

### 十、Event Loop。 setTimeout 和 Promise 的执行顺序。

宏任务与 微任务。

1.SetTimeout 的执行顺序 是在本次循环结束，下一次事件循环的头部开始时执行。
	
2.Promise 是在 本次循环结束的末尾执行。
	
结论就是promise 会在 setTimeout 之前执行。
	
### 十一、cookie 和 localStorage 、sessionStorage 。

-cookie 4kb，有过期时间。有路径访问限制。在请求中会附带cookie一起发送。

-localStorage 无过期时间，5Mb。

-sessionStorage 在窗口关闭之后过期，5Mb。

### 十二、react 的生命周期 分三个阶段。

**组件挂载阶段**

1.getDefaultProps  获取组件默认的属性。

2.getIniticalState 获取组件默认的状态。

3.componentWillMount 组件即将挂载。

4.render 组件渲染。

5.componentDidMount 组件挂载完成。

**组件 更新阶段**

6.componentWillReviceProps 组件接受新的props，参数1个，nextprops。 props改变时触发。

7.shouldComponentUpdate  组件是否刷新，参数两个 nextprops，nextState。 props 和 state改变时触发，作对比，决定是否更新，返回false会阻止组件重新渲染，会阻止 will update，render，didupdate 执行。

8.componentWillUpdate 组件即将更新，参数两个 nextprops，nextState。

9.render  组件渲染。

10.componentDidUpdate 组件完成更新，参数两个  prevProps，prevState，此时已经更新。

**组件销毁阶段**
	
11.componentWillUnmount  组件即将销毁。

其中 shouldComponentUpdate 阶段是react 优化的重点，可以自定义diff算法避免不必要的dom操作。

使用 key值来帮助react识别列表中所有子组件的最小变化。

react 通过render 实现数据的绑定。

view 触发action ，vm 接受 action，派发dispatch，renducer 接受state 并返回新的state ，vm 接受新的state 并触发render 函数 重绘页面。页面数据更新。
	
### 十三、防抖和节流。
	
`防抖`:指的是，在某一时间段内频繁的触发函数。通过setTimeout 来实现，设定一个时间段，在该时间段内如果一直重复触发函数，则不执行函数，如果在时间段内没有触发函数，则会执行一次函数。
	
`节流`:是指当某函数频繁的触发一段事件之后，执行一次，然后再次进入防抖阶段。

### 十四、Vue
	指令
	{{}} 双括号  ，值绑定
	v-bind：id 属性绑定  缩写为 ：
	v-on: click  事件绑定	缩写为 @
	v-for in  数组循环
	v-html  html片段绑定
	v-if 判断语句

	v-on:click.prevent 修饰符，点击时调用e.preventDefault()；

### 十五、数组方法
**增删**

1.`push`  向数组尾部添加一个元素或者一组元素，返回的是数组的长度。

2.`pop` 从数组尾部删除一个元素，返回的是删除的元素。

3.`unshift` 向数组头部添加一个元素或者一组元素，返回的是数组的长度。
	
4.`shift` 从数组的头部删除一个元素，返回的是删除的元素。
	
**查**

5.`every` 判断数组是否每个元素都符合这个条件。如果全部返回true，则返回true，有一个返回false，则返回false。
	
6.`some` 判断数组是否有一个元素符合这个条件，只要有一个符合，就返回true，否则就返回false。
	
7.`indexOf` 和`lastIndexOf`  查找某个元素在数组中的位置。如果不存在则返回 -1，如果存在，则返回第一次出现时的下标。lastIndexOf 是从后向前开始查。在查找是否存在元素NaN时候，会出现错误。

8.`find` 传入一个回调函数，返回符合条件的第一个元素，然后终止搜索。
	
9.`findIndex` 传入一个回调函数，找到符合条件的第一个元素，返回该元素的下标，然后终止搜索。
	
**数组截取。
	
10.`slice（a,b）`  截取数组，返回截取的数组，不传参数时，可做数组拷贝，返回新数组。
	
11.`splice（index,num,...array）` 在具体位置替换删除新增 数组元素。index 为插入的位置，num为操作的数组元素的个数，array为新掺入替换的元素,可以为多个，用逗号隔开，如果删除或者替换元素，则返回一个被删除的数组。如果只传一个index，则默认删除index之后的所有元素。改变原数组
	
12.`sort` 排序  传入一个 function(a,b){return a -b} 如果返回false 则为正向排序，如果返回true，则为反向排序。
	
13.`concat`  数组合并。a.concat(b),返回一个数组，改变数组a。
	
14.`join`  将数组转换为 字符串。
	
15. `reverse`  数组翻转。
	
**数组迭代

1.`for` 迭代数组，可递增也可递减。
	
2.`for in`  是根据key 迭代的。如果数组中存在非数字键，也会被显示出来。适用于迭代对象。当迭代对象时，它还从构造函数的原型中查找继承的非枚举属性，需要使用 hasOwnproperty 来判断属性是否属于对象本身还是属于被继承的。
	
3.`forEach`  es6新的方法，不可被 continue,break,return 跳出循环。返回值为 undefined。

4.`for of`  es6新方法，可用来遍历 数组，字符串，set，map 集合。
	
5、map  es6 新的方法，传入一个函数，这个函数的参数有三个，分别为 item，index，array。 返回值为一个新的数组。

6、filter es6 新的方法，传入一个函数，如果这个元素满足条件，则返回该元素。返回值为一个新的数组。返回符合条件的元素组成的数组。

7、reduce es6 新的方法 ，传入一个函数，其中第一个数为上一个循环的结果，第二个参数为 当前循环的数组元素。最终返回一个值。
	
转换
	
1、Array.from() 可以将 类数组元素转换为数组元素。 Array.prototype.slice。call(args) 该方法也可以将类数组转换为数组。
	
2、a.includes() 判断数组a中是否存在该元素。 两个参数，查找的值，起始位置。用来替换 indexOf 方法。indexOf在查找元素 NaN时，会出现错误。
	
3、Array.isArray()  可以判断是否是数组类型。

**数组去重 

Array.from(new Set(Array))

**查询类型 

Object.prototype.toString.call(arr) 返回 [object Array] [object String] [object Function];

Array.isArray()

instanceof Array
	
### 十六、axios。
	
`get` ，`post` ，`put` ，`delete`，`head` ，`patch` `request`，在使用别名的时候。url ，method ，data 这些属性都不必在配置中指定
	
例如 axios.post(url,data,config);

`axios(config)` 此时 config中需要 `url`，`method`，`data`，`params` 等参数。

axios.all([request1,request2]).then

配置的默认值 `axios.defaults`， 默认值的优先级为  defaults  ， 通过 axios.defaults 修改的 ，在请求时 config中设定的参数 。后者优先级高于前者。

可以通过 axios.defaults.参数  来修改

**拦截器

var requestinterceptors = axios.interceptors.request.use(function1，function2) 

在请求之前拦截，function1参数为config，function2参数为error。

var responseinterceptors = axios.interceptors.response.use(function1，function2) 

在响应被then或者catch之前处理数据，function1参数为 response ，function2 参数为error。
	
请求拦截器移除  axios.interceptors.request.eject(requestinterceptors )；

响应拦截器移除 axios.interceptors.response.eject(responseinterceptors);

错误处理  catch ，还可以使用config 中的 validateState function来自定义HTTP状态码的错误范围。
	
请求取消	 var CancelToken = axios.CancelToken;var source = CancelToken.source(); 

使用 axios.isCancel 来判断是否为取消。

`axios.create()` 创建一个axios 的新实例，可用于不同的http请求。

### 十七、Promise  

一句话概括Promise  ，Promise对象用于异步操作，代表一个尚未完成并且预计将在未来完成的异步操作。
	
new Promise((resolve,reject)=>{resolve(data)}).then()；
	
Promise.resolve（123） 相当于 new Promise （（resolve，reject）=>{resolve(123)}）;

Promise.all([promise1,promise2]) 等待所有异步调用全部完成，如果其中一个失败，则全部失败。

Promise.race([promise1,promise2]) 如果有一个成功完成，则 成功，如果全部失败，则失败。
	 
### 十八 、 antd 各个组件的使用。
	
1.Form 表单组件 Form.Item
		
	Input 输入框

	Select 下拉框

	checkbox 复选框

	Cascader 级联选择。

	DatePicker 日期组件。

	Upload 	上传组件。

	search  搜索组件 ，onsearch ，enterbutton

	page

2.Table 表格组件

3.Button 按钮组件

4.24栅格  ROW 和 COL组件
	
5.Icon 图标组件  icon 引用阿里巴巴Icon图标库的js

6.message 全局提示。
	
7.Model 对话框。

### 十九、 对象深拷贝和浅拷贝。

**深拷贝

1.将对象A拷贝到对象B，利用递归原理，将所有属于对象的属性类型都遍历赋值给另一个对象即可。如果属性是另一个对象的引用，则对该属性递归。
	
2.利用 JSON.parse(JSON.stringify())  ，通过JSON解析的方法。当值有`null`,`undefined` 和 `function` 时，会转换为空。
	
**浅拷贝

1.对象引用的拷贝。 Object.assign  ，通过 Object.assign({},a) ,可以实现一层的深拷贝。如果 值中有 其他对象的引用，则依旧是浅拷贝。
	
2. ...展开运算符。

### 二十、AMD  RequireJs 。

AMD 优先加载，先加载后调用。
	
CMD 哪里需要，哪里加载。
	
在 require.config 中配置路径。

define定义模块，将依赖模块先加载，然后在callback中使用，return 一个对象或者方法。

循环依赖问题，需要导入 require模块，然后再callback中 调用require 来加载模块。

### 二十一、 TypeScript
	
基础类型  string ， number ，boolean ， undefined ， null ， never ，void（undefined和null）

接口 interface ， 接口继承 extends 。
	
类继承 implements  ,定义接口 interface ，然后 类继承并实现这个接口定义的属性和方法。

别名 type
	
泛型 Array  T  可以用 Array<T> 或者 T[] 表示。

非必须 ?:  

联合类型 | ：

交叉类型 ：&

继承 extends  多个继承用 逗号 隔开 。

类型推断 
	
类型断言  as  或者用！开头
	
元组  [string ,number]  ,元组超出 按照 联合类型检测。
	
枚举 enum

### 二十二 、 字符串方法 

1.`charAt`   获取某个位置上的字符。传入的参数范围为 0-str.length-1，可以是数字或者字符串数字。如果传入其他的，则默认返回第一个字符。如果不在参数范围内，则返回空字符串。
	
2.`indexOf`  和 数组的indexof 作用相同，返回第一个出现的字符的位置。

3.`lastindexOf`  和 数组的lastIndexOf 作用相同，从右至左开始查询第一个出现字符的位置。

4.`substr` 和 c语言的 substr 作用相同，传入两个参数 a，b ，其中a代表起始位置，b代表截取的长度。如果a为负数，则表示从右开始 a绝对值的位置开始截取。b只能为大于等于0的整数。表示长度。返回截取的字符串。

5.`substring`(a,b)  从a位置开始截取，截止到b位置之前，不包含b位置。返回被截取的字符串。当a为负时，将a转换为0。a,b都为正数。按两个数最小的那个开始计算位置。

6.`slice`  截取，和 substring 相同，都是start 和end-1，主要是 关于负数的转换。substring转换第一个参数负值为0，而slice则将负数都从右开始计算，然后从左至右截取。

7.`split`（a,b） a为字符串或者正则表达式。b为数组的最大长度。 将字符串转换为数组。

8.`replace`（正则，newstr） 字符串内部替换。

9.`match`（rgExp） 正则匹配。

10.`startsWith` 以什么为开头

11.`endsWith`  以 什么结尾 

12.`toLowerCase`  将字符转换为小写

13.`toUpperCase`  将字符串还为大写

14.`toString` 返回字符串

15.`valueOf` 返回字符串对象。

### 二十三 、 scss css预处理器
1.嵌套。

2.变量 `$str`。

3.计算 + - * /。

4.`@mixin` 和 `@includes`。 写一组css ，传入变量。返回一组css。

5.`extends` 继承。

6.`@function @return`  写一个 function ，传入变量 ，@renturn 一个包含变量的计算表达式。

### 二十四、 做 移动端 一个高度是宽度一半的 div，宽度随设备宽度自适应。

1.js 获取 div 的实际宽度，动态设置 div 高度。
	
2.css 设置 height 为 50vw。

### 二十五、移动端 rem 适配  。 淘宝移动端的 flexible。屏幕像素密度 = 分辨率 / 屏幕尺寸。 屏幕尺寸为 英寸 ，1英寸为 2.54cm。 

主要基于 设备像素比 ，dpr。设备像素比 = 设备像素 / css像素(设备独立像素)。 devicePixelRatio . 垂直方向或者水平方向

<meta name="viewport" content="width=device-width, initial-scale=1.0">

在移动端，布局视口的宽度要远大于浏览器的宽度。在PC端，布局视口的宽度等于浏览器的宽度。
	
布局视口是为了保证在移动端 也可以正常浏览 PC端的网页。

ppi 屏幕像素密度，即每英寸聚集的像素点个数。 dpi 每英寸像素点， 印刷行业术语。
	
虚拟像素 ，指的是 css 像素。
	
即设备的css 像素相同，但是 设备物理像素不同，retina 屏就是 二倍 dpr 屏幕，一个css像素对应四个 物理像素。 1倍的dpr 则为一个css 像素对应1个设备物理像素。

### 二十六、清除浮动。
	
1. ``.clearfix:after{
		content:' '；
		display:block;
		clear:both;
		height:0;
		visiblity:hidden;
	}``

2.父元素 float 浮动。
	
3.父元素 overflow 设置为 hidden 或者 auto；
	
4.添加 div style cleat：both 


### 二十七、 Object.is(a,b) 判断a,b是否相等。
	
es6 用来判断 a 和 b 是否相等。类似于全等 === 。但是又不一样的地方

全等 比较 值 和 值得类型，但是对于 NaN这种不等于自身的值，却同样返回false。 但是 object.is(NaN，NaN） 则返回true。且 -0 和 +0 不相等， 任何返回NaN 的表达式 都和NaN 相等。

ie 不兼容。
	 
以下是利用原生js实现polyfill
 	
if（!Object.is）{
	Object.is = function(x,y){
		if(x === y){ 如果相等，则判断 x是否为0 或者 如果x等于0时，判断 1/x 等于 无穷大，1/y 等于 无穷大，是否相等， 1/-0 === - infinity. 1/+0 == infinity
			return x!==0 || 1/x === 1/y;  //符号会随着无穷显示。
			
		}else{
			return x!==x &&y!==y  // 如果不相等，则判断x 和y 是否都不等于自身，如果都等与自身，则说明x，y 都为NaN，返回true。只有NaN 不等于自身。
		}
	}
}

### 二十八、 什么是闭包 

利用定义时的词法作用域。 一个parent函数，返回一个child函数，child函数拥有parent内部变量和方法的所有访问权。

1.当一个函数执行完毕，内存回收之后依然驻留在内存中。 闭包可以拥有函数所在的词法作用域的访问权。

2.闭包的使用场景。面向对象，封装。

### 二十九、面向对象，抽象，封装，继承，多态，聚合。 原型链。

1.实例抽象化，提取共同点，属性。

2.封装，在抽象类上添加行为，提供对外接口。

3.多态，多个实例的相同行为可根据实例属性的不同呈现出不同的行为标准。

4.原型链 js面向对象实为原型继承。

5.聚合 多个对象聚合可以形成新的对象。

### 三十、面向对象和面向过程。

1.面向过程，是将问题分解，并通过函数一步一步实现，然后逐步调用。


2.面向对象，是将问题分解并抽象为各个对象，通过对象添加属性方法行为。

面向对象的四大要素是   抽象，封装 ，继承 ，多态。

### 三十一、eventloop 事件流  宏任务 和 微任务

1.宏任务  setTimeout setInterval  


2.微任务  Promise 

宏任务是在本轮事件队列结束之后，下次事件队列开始处理之前 执行。

微任务 是在 本轮事件队列的末尾执行。

微任务的执行顺序 先于 宏任务的执行。

### 三十二、XML ，HTML ， XHTML ，JSON

1.XML 是一组标签，包括数据，标签不是预先定义的，用来描述数据结构的。

2.XHTML 是HTML 的严格语法，约定属性名必须小写，空元素必须关闭，属性名必须加引号等。

3.HTML 超文本标记语言 标签都是预定义的。

4.JSON 是 一种轻量级的数据交换格式，是javascript 的一个子集，由键值对组成，结构简单，易于读写。

### 三十三 、输入网址 到网页呈现的过程。

1.DNS域名解析。

	在本地域名服务器中查询IP地址，未找到域名
	
2.TCP链接，三次握手。
		
	①第一次握手，建立连接，客户端向服务端发送请求报文。
		
	②第二次握手，服务器接受到请求报文后，如同意链接，则向客户端发送确认报文。
		
	③第三次握手，客户端接收到服务器的确认后，再次向服务器发送确认报文，完成连接。
	
3.浏览器发送HTTP请求。
	
4.服务器处理HTTP请求。
	
5.浏览器页面渲染。 
		
	①构建DOM树。
	
	②构建css数。
	
	③定位页面元素，render tree
	
	④绘制页面元素
	
6.断开TCP连接，四次挥手。
		
	① 客户端想分手，发送消息给服务器。
	
	②服务端通知客户端接收到分手请求，但是还没做好分手准备。
	
	③服务端已做好分手准备，通知客户端。
	
	④客户端发送消息给服务器，确认分手，服务器关闭连接。

### 三十四、前后端通信  1、发送者和请求者，2、传输媒介 3、传输的数据 4、传输协议

	目的是为了 1、传输数据 2、发布指令。

### 三十五、webpack 的loader 和plugin 的区别。

1.loader 是对模块的源代码进行转换，可以再 import 模块的时候进行预处理文件，loader 可以将文件从不同的语言转换为javascript。甚至可以解析 css模块，和图片。loader是一个函数，接收原文件为参数，返回转换的结果。
	
2.plugin 是为了拓展 webpack 的功能。在构建流程里边添加钩子，可以完成loader 不能完成的复杂功能，可以控制webpack打包的各个流程。实现对webpack 的自定义功能扩展。


webpack 打包流程  1、读取文件，分析模块依赖 2、解析文件，深度遍历 3、针对不同模块使用不同的loader 4、编译模块，生成抽象语法树 5、遍历抽象语法树，生成JS。

### 三十六、模块化解决了前端的哪些痛点。
	
1.命名冲突。

2.文件依赖。

3.代码复用。

### 三十七、 关于 三大框架的 数据绑定。

**框架主要解决了jQuery时期的频繁操作DOM 的痛点，封装了DOM操作，通过数据驱动，自动更新DOM。
	
1.angualrjs 双向数据绑定，依赖于脏检查，通过将{{}}或者ng-bind 的值转换为一个个 `watcher`，当用户通过view 触发 `click`或者`change`事件时，会触发$scope上响应model 的响应函数，通过去遍历每一个watcher值，触发脏检查，`$digest`函数，`$apply`函数实际也是调用的$digest 函数。知道所有的watcher的值不再发生变化。所以 遍历至少执行两边。执行完之后修改DOM。

2.vue 双向数据绑定，通过 `Object.defineProperty` 函数 ，通过设置 obj ，obj.data , config ，其中config 中定义了 `getter` 和 `setter` ，来截取数据的存取。 Object.defineProperty 的配置参数一共两种。一种是数据描述符，一种是存取描述符。这两种描述符不能同时出现。

3.react 单项数据流。 结合 `redux` 单项数据流思想。 由view 触发 操作，viewModel 解析操作，派发 action，reducer接受action ，返回新的state，viewModel 接受新的state，重新渲染`virtual Dom`，然后通过`Diff`算法，更新DOm。


### 三十八、 null 和 undefined 的差异。
	
**相同点
	
① 都属于 falsely 值。

② 在双等号 比较时返回true。

③ 在if条件语句中都返回false。
	
**不同点
	
① typeof null 返回 object，undefined 返回 undefined。

② 转换为 数字时，null 转换为0，undefined转换为 NaN。

③ undefiend 表示 调用一个变量，而该变量没有赋值，这时默认为 undefined。

④ null 是一个特殊的对象，将变量设置为null时，会被内存回收机制回收。

### 三十九、基本类型，引用类型。

基本类型  `null` ，`undefined` ，`string` ，`number`，`boolean` ，`symbol`（es6新增）。原始类型

引用类型 `Object`

类型比较使用 Object.prototype.toString.call(变量) === [object Array] 或其他 来比较。 返回的类型有 Null ，Undefiend ，Array ，String ，Number，Boolean，Function，Symbol，Object,RegExp,Date

基本类型在内存中是**栈存储**，引用类型在内存中是**堆存储**。

### 四十、new 操作符 实现了什么功能。

function Person（name）{
	this.name = name;
	this.getname = function(){return this.name}
}

var obj = {};

obj.__proto__ = Person.prototype;

var res = Person.call(obj,'name');


return typeof res=== 'object'?res:obj ;

### 四十一、如何判断一个变量为数组类型。
	
1.Object.prototype.toString.call(arr) === [object Array]

2.Array.isArray(arr);

3.arr instanceof Array 

### 四十二、 let const var 的区别。

1.let const 无变量提升，暂时性死区。

2.var 变量提升为undefined。

3.const 不变，let 为可变数据。 在块内不能二次声明。var 声明过的也不能。

4.块内通过const 和let 定义的变量会覆盖 父作用域的值。

### 四十三、为什么虚拟dom会提高性能。

1.virtual dom 会通过 在js 和dom之间加一层缓存。利用javascript 对象描述 dom树。
	
2.当virtual dom 的状态改变时，会重新构造一颗新的对象树。然后用新的树和旧的树进行比较，记录这两棵树的差异。

3.diff 算法。 深度优先比较。
	
	①、将树形结构按照层级分解，只比较同级元素。
	
	②、给列表添加key，方便比较。
	
	③、匹配相同class 的component
	
	④、合并操作，将同一时间setState 的数据合并，到每一个时间循环的结束，React 检查所有标记dirty的component重新绘制。
	
	⑤、选择性子树渲染，开发人员可以重写shouldComponentUpdate ，提高diff性能。

### 四十四、react 中的 refs。 使用 ref = {（input）=>{this.input = input}}

1.该声明的回调函数会接受input 对应的DOM元素，我们将其绑定到this指针以便在其他的类函数中使用。另外值得一提的是，refs不是类组件的专属，函数式组件同样能够利用闭包暂存其值。

### 四十五、react 中的 render callback pattern

**回调渲染模式

组件会接受某个回调函数作为其子组件，然后在渲染函数中以 props.children 进行调用。
	
这种模式的优势在于将父组件和子组件解耦，父组件可以直接访问子组件的内部状态而不再需要通过Props传递。这样我们更为方便的控制子组件展示的UI界面。

### 四十六、react 的 展示组件和 容器组件 之间的不同。通过react-redux connect创建。

1.展示组件 主要关心的是看什么，通过props 接收数据和回调，并且基本没有自身的状态。拥有的状态也只是和自身的UI状态有关系。

2.容器组件更关心怎么运作，容器组件会为展示组件或者其他容器组件提供数据和行为，调用actions，负责更改传递给展示组件的props。是其他组件的数据源。
	
### 四十七、react 中的 类组件和函数组件 之间有什么不同

1.类组件 不仅允许你使用更多的额外功能，例如 组件自身的状态，和生命周期钩子，也能使组件直接访问store 并维持状态。

2.函数组件是 当 一个组件只需要 接受props，并将组件自身渲染到页面时，不存在自身的状态，这时它是一个 无状态组件，可以使用一个纯函数创建这样的组件。这种组件也叫哑组件或者展示组件。

### 四十八、 react 中 state 和 props 之间有何不同

state 是一种数据结构，用于组件挂载时所需数据的默认值，state会随着时间的推移而发生突变，但多数时候是用户自己的行为导致的。

props 则 属于组件的配置，props 是有父组件传递给子组件的，对子组件来说，props是不可变的，组件不能改变自身的props ，但是可以通过props中传递的方法修改父组件的state，继而达到修改props的目的。

### 四十八、受控组件

如`input`组件的value 属性 根据`onchange` 事件来与`state`做交互

### 四十九、高阶组件
	
高阶组件是一个以组件为参数并返回一个组件的函数。

### 五十、为什么建议传递给setState 的参数为一个函数而不是 一个对象。

**因为setState 是异步的。

如果传对象，并且多次 setState 的时候，会将state 合并为一个state，这样就丢失了state状态操作，只留下了最后一个操作。所以使用 function返回一个新的state才正确。

### 五十一、react 组件方法bind this 的替代方法 

使用`箭头函数`绑定this。但是这样就导致了每次组件渲染就会创建一个新的回调。

### 五十二、构造函数中的super 

在super 调用之前，子类是不能使用this的。

### 五十三、在react 中，哪个生命周期中调用副作用函数。

应该在 `componentDidMount`  这个生命周期内 调用ajax，这个方法会在组件第一次挂载时调用，在整个生命周期中止执行一次。

因为你不能保证在 组件挂载完成之前调用ajax时，ajax返回数据 setState 的时候，组件是否已经挂载。

### 五十四、react createElement 和 cloneElement

1.createElement react就是使用这个函数渲染DOm的。接受三个参数  标签名，标签属性，children子组件。

2.cloneElement 第一个参数是组件，其他与createElement相同。主要是为了将第一个参数组件的属性替换。

### 五十五、flux 思想，Redux  单项数据流，一个store

1.用户访问view。

2.view 发出用户的action

3.Dispatcher 收到action，要求Store进行相应的更新

4.store更新后，发出一个change事件。

5.view 收到change事件后，更新页面。

redux 的缺点是  一个组件需要的数据都需要父组件传过来。

当一个组件相关数据更新时，即使父组件不需要用到这个组件，但是父组件还是会重新render，或许需要些复杂的shouldComponentUpdate进行判断。

### 五十六、正则表达式。

	^ 开头  $ 结尾
	
	\d 数字
	
	\s 非空
	`[1-9a-zA-Z]`数字字母范围
	`?` 0次或1次
	`+` 1次或多次
	`*` 0次或多次 

### 五十七、关于图片的下载问题。

如果 img 元素 出现，则浏览器会下载src 中的图片，即使 该 img 的display 为none。

如果 一个div 的background-image ，如果该元素的display为none ，则 图片同样会下载。**只有当div 的父元素也是display 的情况下，背景图片不会加载。

### 五十八、js 强制类型转换。
	
**显示类型转换

`String`  `Number`  `Boolean` 

**隐式类型转换

1.+ 号 ，X + Y  如果 X 和 Y 其中有一个是数字
		
	1.另一个是字符串，则将 数字转换为字符串。
		
	2.如果另一个是布尔值，则将布尔值转换为数字。
		
	3.如果为对象或者数组，则将对象和数组使用 Object.prototype.toString.call(obj)转换为字符串，然后执行字符串相加。

2. -号 ，进行 Number 转换。除了falsey，和true ，null，可转换为0和1，其他全部转换为 NaN。undefined也转换为 NaN，null转换为0；

3. \*,/ 进行Number 转换。除了`falsey`，和`true` ，`null`,可转换为0和1，全部转换为 NaN。。undefined也转换为 NaN，null转换为0；

4.== ，X==Y，如果X为数字，则将Y转换为数字，如果Y为数字，则将X转换为数字。都不是数字，基础类型比较unicode值，对象则比较 堆地址。

**if 则全部 做 括号内结果的 Boolean 转换。 falsely值有  “”（空字符串） ， false ，0 ，NaN ，undefined ，null 

### 五十九、link 和 script

`link` 加载css ，不阻塞 DOM tree 的构造。但是阻塞 render tree 的渲染。

`script` 不加`defer` ，`async` ，则会阻塞 DOM tree 的构造，但是 浏览器会 偷看 script标签之后的  `link` ，`img`，`script` 元素，然后加载资源。 浏览器也会立即加载script并执行。

浏览器每碰到一次 script 标签，就要渲染一次 render tree，以保证 script 执行时可以获取到最新的页面状态。
	
`script` 执行都会 阻塞 html 解析，阻塞 DOM tree 的构建。
	
`defer` 和 `async`  也不一定保证 script 会按照出现顺序执行。HTML5标准 规定 defer 会按循序执行，会在DOMConenteLoaded之前执行。但是 浏览器并未严格按照标准处理，所以会出现风险。当二者一起使用的话，与 async 的效果相同。

当`script` 加上 `defer` 时，异步加载，与DOM tree 加载 一起并行执行。但是 会在 `DOmContentLoader` 之前执行js，即等到DOMtree 完全解析完成之后。
	
`async` 则只是异步加载，与DOM tree 加载一起并行执行。并不一定会在 DOM tree 完全加载之后执行。 加载完成之后会立即执行。最好不要 有涉及DOM 的操作。

优化方法。
	
1.在head 中 将 script 写在link上边。
	
2.将所有 脚本都放在 </body>出现之前。

### 六十、 outhtml ，innerhtml ，innerText ， TextContent

`outHtml` 是 将元素本身及所有子节点全部 输出。包括空格。

`innerHtml` 则 输出元素的所有子节点，包括空格。

`innerText`  输出所有子节点中的文本节点。

`textContent` 输出所有子节点中的文本节点的值。p.textContent

`nodeValue` 输出具体文本节点的value。 如  p 标签为元素节点 ，p中的文字 123 为 text 文本节点。nodeValue 就是 text 文本节点的值。 p.childNodes[0].nodeValue

### 六十一、js 设计模式

1.工厂模式  在函数内部定义一个空对象，给对象添加属性和方法，返回这个对象。
 
2.构造函数模式  创建构造函数，通过new 操作符创建函数实例。 缺点为 每创建一个实例，方法就重新创建。各个实例之间的方法不是共享的。

3.原型模式   通过 函数的prototype属性定义属性和方法，通过new 操作符创建函数实例。缺点  方法共享，但是属性也共享，如果某个实例特殊，则需要自定义属性。

4.组合模式（构造函数+ 原型） 利用原型和构造函数的优点，构造函数定义属性，原型定义方法。

5.动态原型模式。  通过条件判断在prototype上动态添加方法。比组合模式更灵活。

### 六十二、js 继承方式

1.原型链继承。 将一个构造函数的prototype 执行 另一个被继承构造函数的实例。

2.经典继承 （伪构造函数模式）

3.组合继承  使用经典继承，继承属性。使用原型链继承，继承方法。

4.原型式继承  Object.create()，一个函数接受一个对象参数，然后创建一个新的构造函数，将对象赋值给这个构造函数的原型，然后返回新的构造函数实例。
		
function jicheng(obj){ var newfun = function(){};newfun.prototype = obj;return new newfun(); }

5.寄生式继承 不能函数复用。在原型式继承的基础上添加私有的属性和方法。

6.寄生式组合复用  在原型式继承的基础上将 返回新的对象的constructor 指向另一个需要继承的构造函数。

### 六十三、set map 区别

`Map` 二维数组  [['name','hahaha'],['bob',66]] ; Map实例有`get`,`has`,	`delete`,`set` 四个api，增删改查。

`Set` 一个不含重复项的一维数组。 Set 实例有 `add`，`delete` 两个api

### 六十四、object 的 复制。

1.浅复制。 Object.assign   ， array 的 slice()。

2.深复制    JSON.parse(JSON.stringify()); 如果属性存在 undefined，function ，symbol,会被忽略。

3.deepcopy 函数。深拷贝。
      function deepCopy(obj) {
	 var result = Array.isArray(obj) ? [] : {};
	 for (var key in obj) {
	    if (obj.hasOwnProperty(key)) {
		  if (typeof obj[key] === 'object') {
		    result[key] = deepCopy(obj[key]);   //递归复制
		  }else {
		      result[key] = obj[key];
		  }
	     }
	  }
	  return result;
       }

### 六十五。 元素竖向的百分比设定是相对于容器的高度吗？

**不是。

**元素竖向的百分比设定。  

1.`height` 百分比， 基于容器的高度。

2.`padding-left`,`padding-right` 基于容器的宽度。

3.`padding-top`，`padding-bottom` 也是基于容器的宽度。

4.`margin` 和`padding` 的基准是相同的。

5、border 设置百分比会报错。

### 六十六、FOUC 及 FOUC 的解决方法。

`FOUC` （无样式内容闪烁）  主要是由@import css  或者 多个style 标签或者css文件出现在页面底部引入导致的页面闪烁及花屏。

解决方法是 用link 加载css，放在head里。

### 六十七、非缓存模式刷新页面。

`location.reload(true)`  当参数为true时，会强制 非缓存刷新页面。

当参数为false 的时候，会对比`header` 中的 `If-Modified-Since` 来对比服务器是否使用缓存刷新。

### 六十八、为什么利用多个域名来存储网站资源会更有效？

1. CDN 缓存更方便。CDN是一种组合技术，其中包括源站、缓存服务器、智能DNS几个重要部分。

2. 突破浏览器并发限制

3. 节约 cookie 带宽

4. 节约主域名的连接数，优化页面响应速度

5. 防止不必要的安全问题

### 六十九、文件上传。

1. 使用`form` ，enctype = “multipart/form-data”,原生表单上传。

2.使用 `formData`  ，创建 var files = new FormData（）；添加 files.append(key,value)。通过ajax上传。

**form 的 enctype 的值有三种  

- `application/x-www-form-urlencoded`  再发送前编码所有字符。

- `multipart/form-data`   不对字符编码。在使用包含文件上传控件的表单时，必须使用该值。

- `text/plain`  空格转换为“+”加号，但不对特殊字符编码。

### 七十、redux 中间件

**由于redux只能处理同步的Action，但可以通过中间件来处理其他类型的action。

1.`redux-thunk`  主要返回一个函数，在未来某一时间 dispatch 一个action 。 体现在延迟动作的发送。 重复代码太多。

2.`redux-promise` 主要返回一个promise在payload中。 如果payload是一个promise，则会解析 resolve或者reject 的值。无法乐观更新。

3.`redux-promise-middleware` 相对与promise ，采取了更为温和渐进式的思路。保留了和redux-thunk类似的三个action,get,fuilfill,failed.

4.`redux-saga` 使用es6 的 generator 函数，避免回调地狱。

### 七十一、Echarts 与 HighCharts 的区别

1.`echarts`基于`canvas`。`highcharts`基于`SVG`。

2.`echarts` 是百度出品，国产，图表类型很多，地图类图表支持较多，地图功能强大。`highcharts` 图表类型较少，但是基于svg，则更易于扩展。

3.`highcharts` 收费，商业需授权 ， `echarts` 免费。

4.SVG 基于DOM操作，频繁的DOM操作会影响浏览器的性能，数据复杂度高的话，会较慢图表渲染速度，目前可以结合 React 的VirtualDom 技术减少DOM操作。

5.Canvas 则存在大量用户交互场景时，要为细粒度的元素添加事件处理器，必须涉及到边缘检测算法，无疑为开发带来了一定的难度，同时，采用这种方法并不一定精确。

6.兼容性问题，二者都兼容到IE6+。

所以基于 技术、生态、商业化、性能、定制化 五个方面来考虑。 有预算、定制化、交互较多 就选 highcharts。 无预算、通用类表、交互较少对细粒度无太高要求、地图类图表较多 选用 echarts。

### 七十二、判断一个对象是否为空。

1.使用 `JSON.stringify(obj)` 将 对象转换为字符串，与'{}'做对比，是否相等。

2.使用 Object.getOwnPropertyNames(obj) 来判断是否存在，返回值为一个数组，类似于Object.keys 返回的结果。

3.使用 for in 循环，for(let key in obj){return false} return true ，有缺点，即当Object.prototype上手动添加了属性，则一直返回false。

4.使用 ES6 `Object.keys()` 或者 `Object.values()`  的长度来判断。
