# el-date-picker 显示的月份、日期由英文改为中文

# el-date-picker显示的月份、日期由英文改为中文

### 1、英文效果图：

![image-20221222194252915](assets/image-20221222194252915-20230114165941-sij4jl9.png)

### 2、全局设置 src/main.js

```js
import Vue from 'vue'

import 'normalize.css/normalize.css' // A modern alternative to CSS resets

import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'
// import locale from 'element-ui/lib/locale/lang/en' // lang i18n

import '@/styles/index.scss' // global css

import App from './App'
import store from './store'
import router from './router'

import '@/icons' // icon
import '@/permission' // permission control

/**
 * 如果不想使用模拟服务器
 * 您希望将 Mock.Js 用于模拟 api
 * 可以执行：mockXHR（）
 * 目前 Mock.Js 将用于生产环境，
 * 请在联机前删除它！
 */
if (process.env.NODE_ENV === 'production') {
  const { mockXHR } = require('../mock')
  mockXHR()
}

// set ElementUI lang to EN (设置英文版声明)
// Vue.use(ElementUI, { locale })

// 如果想要中文版 element-ui，按如下方式声明
Vue.use(ElementUI)

Vue.config.productionTip = false

new Vue({
  el: '#app',
  router,
  store,
  render: h => h(App)
})

```

### 3、修改完之后的效果图：

![image-20221222195036011](assets/image-20221222195036011-20230114165941-eh5m9g7.png)

### 参考文档：

[element-ui日期控件el-date-picker显示的月份、日期由英文改为中文 - 简书 (jianshu.com)](https://www.jianshu.com/p/831fc32da5dc)

[(20条消息) element plus 日期组件默认为英语，修改为中文的方法__234的博客-CSDN博客_element plus 日期中文](https://blog.csdn.net/weixin_64012291/article/details/124405514)

‍
