### 组件基础

#### 创建组件

```javascript
Vue.component('self-component', {
            data(){
                return {
                    num: 0
                }
            },
            template: '<button v-on:click="num++">{{num}}</button>'
        })
        new Vue({
            el: '#app'
        })
```

上面创建一个叫做self-component的自定义组件。

其中data里面是组件数据状态，template是组件的模板内容，最后我们通过new Vue创建Vue的根实例，通过在根实例内部引入自定义组件标签，来使用自定义组件：

```html
<div id="app">
        <self-component></self-component>
    </div>
```

组件是可复用的Vue实例，所以具有和new Vue相同的选项，例如data、methods、生命名周期函数等。

#### 组件复用

```html
<div id="app">
        <self-component></self-component>
        <self-component></self-component>
        <self-component></self-component>
    </div>
```

> 注意: 组件的data属性，必须是一个函数，返回一个对象的形式。



#### 组件通信

* 通过prop向子组件传递数据

  Prop 是你可以在组件上注册的一些自定义 attribute。当一个值传递给一个 prop attribute 的时候，它就变成了那个组件实例的一个 property。为了给博文组件传递一个标题，我们可以用一个 `props` 选项将其包含在该组件可接受的 prop 列表中：

  ```js
  Vue.component('blog-title', {
              props: ['title'],
              template: `<h1>{{title}}</h1>`
          })
  ```

  一个组件默认可以拥有任意数量的 prop，任何值都可以传递给任何 prop。在上述模板中，你会发现我们能够在组件实例中访问这个值，就像访问 `data` 中的值一样。

  一个 prop 被注册之后，你就可以像这样把数据作为一个自定义 attribute 传递进来：

  ```html
  <blog-title :title="title"></blog-title>
  ```

* 通过$emit子组件向外部传递数据

  ```
  <div id="app">
          <h4>子组件点击了{{count}}次</h4>
          <count-button v-on:add-count="onAddCount"></count-button>
      </div>
      
      Vue.component('count-button', {
              template: `<button @click="addCount">点击父级组件++</button>`,
              methods: {
                  addCount(){
                      this.$emit('add-count', 1)
                  }
              }
          })
          new Vue({
              el: '#app',
              data: {
                  count: 0
              },
              methods: {
                  onAddCount(num) {
                      this.count += num
                  }
              }
          })
  ```

  #### 在组件上使用v-model

  input上的v-model等同于：

  ```html
  <input type="text" v-bind:value="val">
  ```

  当在组件上使用时，v-model则这样：

  ```html
  <cus-inpt v-on:value="value" v-on:input="value=$event"></cus-inpt>
  ```

  为了让它正常工作，这个组件内的 `<input>` 必须：

  - 将其 `value` attribute 绑定到一个名叫 `value` 的 prop 上
  - 在其 `input` 事件被触发时，将新的值通过自定义的 `input` 事件抛出

  写成代码之后是这样的：

  ```javascript
  Vue.component('cus-inpt', {
              props: ['value'],
              template: `<input v-bind:value="value" v-on:input="$emit('input', $event.target.value)">`
          })
  ```

  使用：

  ```html
  <cus-inpt v-model="val"></cus-inpt>
  ```

  

#### 通过插槽分发内容

和 HTML 元素一样，我们经常需要向一个组件传递内容，Vue自定义的<slot>元素可以帮我们实现：

```html
<test-slot>
            <h2>我是h2</h2>
        </test-slot>
        Vue.component('test-slot', {
            template: `<div><h1>我是h1</h1><slot></slot></div>`
        })
```

#### 局部注册组件

全局注册往往是不够理想的。比如，如果你使用一个像 webpack 这样的构建系统，全局注册所有的组件意味着即便你已经不再使用一个组件了，它仍然会被包含在你最终的构建结果中。这造成了用户下载的 JavaScript 的无谓的增加。

```html
 var componentA = {
            template: '<h1>componentA</h1>'
        }
```

然后在 `components` 选项中定义你想要使用的组件：

```javascript
new Vue({
            el: '#app',
            components: {
                componentA
            }
        })
```

