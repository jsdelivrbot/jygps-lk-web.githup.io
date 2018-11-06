import babelpolyfill from 'babel-polyfill'
import Vue from 'vue'
import App from './App'
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-default/index.css'
//import './assets/theme/theme-green/index.css'
import VueRouter from 'vue-router'
import store from './vuex/store'
import Vuex from 'vuex'
import axios from 'axios';
//import NProgress from 'nprogress'
//import 'nprogress/nprogress.css'
import routes from './routes'
import Mock from './mock'
Mock.bootstrap();
import 'font-awesome/css/font-awesome.min.css'
import './iocnstyle/iconfont.css'
import './iocnstyle/iconfont.js'
import md5 from 'md5'
import 'babel-polyfill'
import promise from 'es6-promise'
import qs from 'qs';
import htmlToPdf from './components/htmlToPdf/htmlToPdf';
import 'babel-polyfill'
Vue.use(qs)

promise.polyfill();

window.md5 = require('md5');

Vue.use(ElementUI)
Vue.use(VueRouter)
Vue.use(Vuex)

Vue.use(htmlToPdf)

//NProgress.configure({ showSpinner: false });

// 页面刷新时，重新赋值token
if (window.sessionStorage.getItem('token')) {
  store.commit(types.LOGIN, window.sessionStorage.getItem('token'))
}

const router = new VueRouter({
  // mode: 'history',
  routes: routes
});

router.beforeEach((to, from, next) => {
  if (to.matched.some(r => r.meta.requireAuth)) {// 判断该路由是否需要登录权限
    if (store.state.token) {// 通过vuex state获取当前的token是否存在
      next();
    }
    else {
      next({
        path: '/login',
        query: {redirect: to.fullPath} // 将跳转的路由path作为参数，登录成功后跳转到该路由
      })
    }
  }
  else {
    next();
  }
})


new Vue({
  //el: '#app',
  //template: '<App/>',
  router,
  store,
  //components: { App }
  render: h => h(App)
}).$mount('#app')