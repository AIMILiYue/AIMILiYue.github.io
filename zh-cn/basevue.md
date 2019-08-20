

## 1.准备工作

开发环境：

```html
<!-- 开发环境cdn链接，包含命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

生产环境：

```html
<!-- 生产环境cdn链接，优化了大小和速度 -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

## 2.基本语法

### 2.1 渲染数据

#### 	2.1.1 插值表达式

```html
<div id="app">
    {{ info }}
</div>
```

```html
<script>
    var app = new Vue({
        el : '#app',
        data : {
            info : '我渲染了数据'
        }
    })
</script>
```

?> 因为vue是mvvm设计模式，所以，当data中的数据发生改变时，插值表达式的内容也会发生相应变化。例如，在控制台输入`app.info='改变了渲染内容'`，页面上的数据会发生变化

!> 如果想要让数据一次性的插值，可以使用`v-once`指令，这样，数据改变时，插值处的内容不会更新，但是会影响该节点上的其他数据绑定。



#### 	2.1.2  v-text

我们可以使用另外一种方式来渲染内容，作用与插值相同。

```html
<div id="app">
    <p v-text="info"></p>
</div>
```

```html
<script>
    var app = new Vue({
        el : '#app',
        data : {
            info : '我渲染了数据'
        }
    })
</script>
```

!> 因为插值表达式方式和`v-text`方式只会将数据解析为普通文本，而非html代码，如果数据中有html标签等内容，请使用`v-html`指令

#### 	2.1.3 v-html

`v-html`指令可以将数据内容中的标签等解析出来，而不仅仅是普通文本

```html
<div id="app">
    <p v-html="info"></p>
</div>
```

```html
<script>
    var app = new Vue({
        el : '#app',
        data : {
            info : '<span style="color:red">我渲染了数据</span>'
        }
    })
</script>
```

### 2.2 属性绑定

?> 属性绑定使用`v-bind`指令，例如`v-bind:src="imgSrc"`，也可使用简写形式`:src="imgSrc"`，以下示例中均使用简写形式

#### 	2.2.1 普通属性绑定

```html
<div id="app">
    <!-- 分别绑定title href src -->
    <p :title="title"></p>
    <a :href="href">click me</a>
    <img :src="src" />
</div>
```

```html
<script>
    var app = new Vue({
        el : '#app',
        data : {
            title : '这里是title',
            href : 'https://aimiliyue.github.io/',
            src : 'https://aimiliyue.github.io/_media/20151126034810278.gif'
        }
    })
</script>
```

!> 注意：`v-bind:属性="属性值"`中的属性值必须是data中的属性名，不能是一个既定数据



#### 	2.2.2 class属性绑定

?> 因为其他属性的属性值基本上是唯一的，所以我们可以使用通用方式去绑定。但class属性和style属性可以允许有多个属性值，所以我们必须要对class和style属性的绑定进行单独处理



?> 在绑定class和style属性时，Vue.js做了专门的加强，不仅支持字符串类型，还支持其他类型的值，如数组和对象。



```html
<div id="app">
    <!-- 绑定class属性 -->
    <p :class="classA">给p标签绑定class属性</p>
    <p :class="{active : classA}">如果data中的classA为true或非空字符串，则绑定class为active</p>
    <p :class="[classA,classB]">同时绑定多个class属性值</p>
    <p :class="[classA,{red : isB,blue : isC}]">混合形式-</p>
    <p class="active" :class="{active : isB}">:class与普通的class属性共存</p>
</div>
```

```html
<script>
    var app = new Vue({
        el : '#app',
        data : {
            classA : 'class1',
            classB : 'class2',
            classC : 'class3',
            isB : true,
            isC : false
        }
    })
</script>
```

**进阶使用**

一般情况下，我们这么去使用`:class`指令：

```html
<div :class="classObject"></div>
```

```html
<script>
    var app = new Vue({
        el : '#app',
        data : {
            classObject :{
                active: true,
    		   'text-danger': false
            }
        }
    })
</script>
```

> 除上述的使用方法之外，还可以绑定一个返回对象的`计算属性`。

```html
<div :class="classObject"></div>
```

```html
<script>
    var app = new Vue({
        el : '#app',
        data: {
          isActive: true,
          error: null
        },
        // 计算属性
        computed: {
          classObject: function () {
            // 最终返回的是一个对象
            return {
              // 可以进行判断
              active: this.isActive && !this.error,
              'text-danger': this.error && this.error.type === 'fatal'
            }
          }
        }
    })
</script>
```





#### 2.2.3 style属性绑定

style属性绑定和class属性绑定极其类似

```html
<div id="app">
    <!-- style属性绑定-->
    <div :style="color: activeColor, fontSize: fontSize + 'px'"></div>
    
    <!--但是通常情况下我们这么去绑定-->
    <div :style="styleObj"></div>
    <!--同样，style还支持数组形式-->
    <div :style=[fontStyles,bgColor]></div>
</div>
```

```html
<script>
    var app = new Vue({
        el : '#app',
        data: {
          activeColor: 'red',
          fontSize: 20,
          // 定义一个属性对象，存放多个style样式
          styleObj : {
              width : '400px',
              height: '400px',
              backgroundColor: 'blue',
              fonstStyles : {
                  fontSize: '25px',
                  color: red
              },
              bgColor: {
                  backgroundColor: 'yellow'
              }
          }
        }
    })
</script>
```



### 2.3 事件绑定

#### 	2.3.1 语法

> 事件绑定，需要使用`v-on:事件名`指令，可以简写为`@事件名`

```html
<div id="app">
    <button @click="count += 1">点击触发count</button>
    <!-- 也可以这样 -->
    <button @click="sums">点击触发sum</button>
    <p>
        点击事件触发了count  {{count}} 次,触发了sums {{sum}}次
    </p>
</div>
```

```html
<script>
    var app = new Vue({
        el : '#app',
        data: {
          count: 0,
          sum: 0
        },
        methods: {
            sums : function(event){
                // 这里的event是原生DOM事件（事件对象）
                this.sum ++
            },
            /*sums方法还可以简写为：
            sums(event){
            	this.sum ++
        	}*/
        }
    })
</script>
```

!> 关于事件的绑定，推荐的是去使用事件去处理数据逻辑部分，而不是去处理DOM事件细节，所以，尽可能少的在事件中去处理DOM，我们尽量去使用`事件修饰符`来处理DOM细节

#### 	2.3.2 事件修饰符

- `.stop`
- `.prevent`
- `.capture`
- `.self`
- `.once`
- `.passive`

以上是常见的事件修饰符，作用见下面代码。

```html
<div id="app">
    <!-- 阻止单击事件继续传播 -->
    <a href="#" @click.stop="doThis">.stop</a>
    
    <!-- 提交事件不再重载页面 -->
    <form @submit.prevent="onSubmit"></form>
    
    <!-- 修饰符可以链式(串联) -->
    <a href="#" @click.stop.prevent="doThat"></a>
    
    <!-- 只有修饰符的情况 -->
	<form @submit.prevent></form>
    
    <!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
	<!-- 即事件不是从内部元素触发的 -->
	<div @click.self="doThat">...</div>
</div>
```

!> 使用修饰符时一定要注意顺序！例如：`@click.prevent.self` 会阻止所有的点击，而 `@click.self.prevent` 只会阻止对元素自身的点击

>  新增

```html
<div id="app">
	<!-- 点击事件将只会触发一次 (2.1.4 新增)-->
    <a @click.once="doThis"></a>
</div>
```

#### 2.3.3 按键修饰符

```html
<div id="app">
	<!-- 只有在 `key` 是 `Enter` 时调用 `vm.submit()` -->
    <input @keyup.enter="submit">
    
    <!-- 还可以这样写 -->
    <!-- onPageDown函数只会在 $event.key 等于 PageDown 时被调用 -->
    <input @keyup.page-down="onPageDown">
</div>
```

```html
<script>
    var app = new Vue({
        el : '#app',
        data: {
        },
        methods: {
            onPageDown(){
                console.log('onPageDown事件被触发了')
            }
        }
    })
</script>
```



### 2.4 计算属性

基本语法：

```html
<div id="app">
    <table>
        <thead>
            <tr>
            	<th>ID</th>
            	<th>姓名</th>
            	<th>年龄</th>
            </tr>
        </thead>
        <tbody>
            <!-- 这里循环的是计算属性中的reprocessData，而不是data中的dataArr -->
        	<tr v-for="item in reprocessData" :key="item.id">
                <td>{{item.id}}</td>
                <td>{{item.name}}</td>
                <td>{{item.age}}</td>
            </tr>
        </tbody>
    </table>
</div>
```

```html
<script>
    var app = new Vue({
        create: {
            // 声明周期函数中调用getData方法来获取数据
            this.getData()
        },
        // 计算属性
        computed: {
            // 以函数的形式声明，但使用时当做变量去使用
            reprocessData(){
                // 将翻转之后的数据返回
                return this.dataArr.reverse()
            }
        },
        el : '#app',
        data: {
            dataArr: []
        },
        methods: {
            getData(){
                // 发送axios请求，获取数据（前提：已经引入axios）
                axios.get('http://127.0.0.1:6666/getdata').then(res=>{
                    // 将获取到的数据赋值给dataArr
                    this.dataArr = res.data.result
                })
            }
        }
    })
</script>
```

个人理解：

> 因html中不能有过于复杂的逻辑和数据再处理操作，所以使用computed来进行专门的数据再处理



### 2.5 其他

#### 	2.5.1 v-model

> 在官方解释中，v-model是专门用来处理用户输入，从而实现表单输入和应用状态之间的双向绑定

+ 限制

v-model用于在`<input>`、`<textarea>`及`<select>`表单控件中进行双向绑定

!> v-model会忽略表单元素的`value`、`checked`、`selected`等初始值，而是将vue实例中的data作为数据来源，如果想用初始值，可以在data中声明初始值

```html
<div id="app">
    <!-- v-model处理普通文本输入，并实时显示 -->
    <input v-model="message">
	
    <!-- v-model处理textarea输入，并实时显示 -->
    <textarea v-model="message"></textarea>
    <p>Message is: {{ message }}</p>
    
    <!-- v-model处理单个复选框，并实时显示 -->
    <input type="checkbox" id="checkbox" v-model="checked">
    <label for="checkbox">{{ checked }}</label>
    
    <!-- 处理多个复选框,需绑定到同一数组中 -->
    <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
  	<label for="jack">Jack</label>
    <input type="checkbox" id="john" value="John" v-model="checkedNames">
    <label for="john">John</label>
    <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
    <label for="mike">Mike</label>
    <br>
    <span>Checked names: {{ checkedNames }}</span>
    
</div>

```

```html
<script>
    var app = new Vue({
        el : '#app',
        data: {
            message: ''，
            checked: false，
            checkedNames: []
        }
    })
</script>
```









