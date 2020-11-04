import Vue from 'vue'
import App from './App'
import { authorization, getRequestCount } from './utils/utils.js'

//定义全局变量
Vue.prototype.$authorization = authorization;
Vue.prototype.$getRequestCount = getRequestCount;
Vue.config.productionTip = false

// Vue.prototype.$reUrl = "https://test.callas2.caih.com" //测试
Vue.prototype.$reUrl = "https://api.caih.com" //生产
App.mpType = 'app'

const app = new Vue({
    ...App
})
app.$mount()