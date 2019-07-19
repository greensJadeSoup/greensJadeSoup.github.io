## ES6
js的一个标准
### 声明
let 声明的变量只在 let 命令所在的代码块内有效。
const 声明一个只读的常量，一旦声明，常量的值就不能改变。
 >上面两个用来代替varhttps://github.com/greensJadeSoup/greensJadeSoup.github.io
 ### 结构表达式
 以下一些要放在script里，函数则在网页调用
```js
 let arr = {1,2,3,4,5}
 let [x,y] = arr ;
 x=1
 y=2
 let [,,a,b] = arr ;
 a=3
 b=4
 let [,...rest] = arr;
 rest={2,3,4,5}
```
 ```js
 let p = {name:"jack",age:21}
 let {name,age} = p
 name = "jack"
 age = 21
 let {name:n} = p
 n="jack"
 let {...obj} = p
 obj = {name:"jack",age:21}
```
```js
const add = (x,y) => a + b;
add(1,2) = 3
const p = {
    name : "jack",
    age : 21,
    say(){
        console.log("hello");
    }
}
p.say();
const hello = ({name.age}) => console.log{name,age};
hello(p);
```
```js
let arr = {"1","2","3","4","5"};
let arr2 = arr.map(s => parseInt(s))
arr2 = {1,2,3,4,5}
let arr3 = arr2.reduce((a,b) => a+b)
arr3 = 15
```
## Vue
一个渐进式框架，一款MVVM模型的框架
推荐npm引入
## node.js及npm安装
两者绑定，安装其一之另一方也有了
在node官网下载，一路确定
node -v,npm -v检查安装版本
### nrm
默认的npm仓库在国外一款，通过nrm更换仓库
npm install nrm -g 安装nrm，-g表示全局安装
nrm ls查看可更换的仓库
use taobao一般采用淘宝镜像，比较快
nrm test taobao测试下载速度
### 后言
今天java学到这里，接下来背英语四级和看软考，软考想了想就不写了，基本都是概念，把重要的画在书里就行了
