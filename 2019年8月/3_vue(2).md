# vue-element模板使用
## ElementUIAdmin2-master
基于vue3.0
## 菜单添加天气
+ 编写src\views\weatherDisplay\index.vue
+ 在src\menu\menu.js中添加
```vue
menu.weather_manage = {
  name: '天气',
  icon: 'fa fa-tag',
  children: {}
};
let WeatherManage = menu.weather_manage.children;
WeatherManage.role = {
  name: '今日',
  path: '/weather_display',
};
```
+ 在src\router\index.js中添加
```vue
{
  path: '/weather_display',
  name: 'WeatherDisplay',
  meta: {
    title: '天气管理',
    keepAlive: true
  },
  component: resolve => require(['@/views/weatherDisplay/Index.vue'], resolve),
},
```
