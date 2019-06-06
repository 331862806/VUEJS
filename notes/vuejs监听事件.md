### v-on监听事件
1. 在 js里为 Vue 对象的数据设置为 clickNumber
```vuejs
data: {
    clickNumber:0
}
```
2. 新建一个方法： count， 其作用是 clickNumber 自增1
```vuejs
methods:{
    count: function(){
         this.clickNumber++;
    }
}
```
3. 在按钮上增加 click 监听，调用 count 方法
```html
<button v-on:click="count">点击</button>
```

完整示例:
```vue
<div id="div1">
  
  <div>一共点击了  {{clickNumber}}次</div> 
  <button v-on:click="count">点击</button>
  
</div>
   
<script>
   
new Vue({
      el: '#div1',
      data: {
          clickNumber:0
      },
      methods:{
          count: function(){
              this.clickNumber++;
          }
      }
    })
   
</script>
```

### v-on 缩写为 @
顶 折 纠 问
v-on 还可以缩写为 @
原来写法：
```html
<button v-on:click="count">点击</button>
```
缩写之后：
```html
<button @click="count">点击</button>
```

### 修饰符
vue.js 还提供了各种事件修饰符来方便开发者使用。
 ```
.stop
.prevent
.capture
.self
.once
 ```

这些都是什么用呢？ 接下来就会一一展开

### 冒泡这件事
事件修饰符 里面有几个都是关于冒泡的，那么什么是冒泡呢? 简单的说就是 父元素里有 子元素， 如果点击了 子元素, 那么click 事件不仅会发生在子元素上，也会发生在其父元素上，依次不停地向父元素冒泡，直到document元素。

尝试点击下面的任意一个 div，就会观察到冒泡现象

具体用法查看内置命令中的 5.7.1 示例

```vue
<style type="text/css">
   * {
       margin: 0 auto;
       text-align:center;
       line-height: 40px;
   }
   div {
       width: 100px;
       cursor:pointer;
   }
   #grandFather {
       background: green;
   }
   #father {
       background: blue;
   }
   #me {
       background: red;
   }#son {
        background: gray;
    }
</style>
    
<div id="content">
    <div id="grandFather"  v-on:click="doc">
        grandFather
        <div id="father" v-on:click="doc">
            father
            <div id="me" v-on:click="doc">
                me
                <div id="son" v-on:click="doc">
                son
            </div>
            </div>
        </div>
    </div>
</div>
  
<script>
    var content = new Vue({
        el: "#content",
        data: {
            id: ''
        },
        methods: {
            doc: function () {
                this.id= event.currentTarget.id;
                alert(this.id)
            }
        }
    })
</script>

```

### 事件修饰符 阻止冒泡 .stop
在me上的click后面加一个 .stop， 那么冒泡到了这里就结束了，就不会冒到father上面去了。
<div id="me" v-on:click.stop="doc">

```vue
<div id="content">
    <div id="grandFather"  v-on:click="doc">
        grandFather
        <div id="father" v-on:click="doc">
            father
            <div id="me" v-on:click.stop="doc">
                me
                <div id="son" v-on:click="doc">
                son
            </div>
            </div>
        </div>
    </div>
 
</div>
```

### 事件修饰符 优先触发 .capture
在father 上增加一个.capture
<div id="father" v-on:click.capture="doc">
那么当冒泡发生的时候，就会优先让father捕捉事件。
点击son或者me的时候，都会优先触发它

### 事件修饰符 只有自己能触发，子元素无法触发.self
修改father,增加 .self
<div id="father" v-on:click.self="doc">
这样点击son 和 me都不会导致其触发click事件，只有点击father自己，才会导致事件发生。

### 事件修饰符 只能提交一次 .once
修改father,增加 .once
<div id="father" v-on:click.once="doc">
这样father点击一次之后，就不会再监听到click了，但是有意思的是，grandFather还能监听到~

### 事件修饰符 阻止提交 .prevent
通过在 click 后面添加 .prevent 可以阻止页面刷新。
```vue
@click.prevent="jump"
```
也可以直接用@click.prevent后面不跟函数
```vue
@click.prevent
```

注： 只有超链和form这种会导致页面刷新的操作，.prevent 才有用。 普通的不导致页面刷新的按钮，加上这个没有任何变化。


