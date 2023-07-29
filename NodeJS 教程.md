:::info
[https://www.bilibili.com/video/BV1rA4y1Z7fd?p=4&vd_source=cd526129166eb01a825598d4f6e06479](https://www.bilibili.com/video/BV1rA4y1Z7fd?p=4&vd_source=cd526129166eb01a825598d4f6e06479)
:::
# 模块
**CommonJS 规范**

1. js要先暴露function，然后再在别的js文件里面导入
```javascript
var a = require('./a')
```
```javascript
function test(){
  console.log("test a");
}
module.exports = test
```

2. 如果想继续加新的function，不能直接复制然后粘贴，而是
```javascript
module.exports = {
  test,
  upper // 引用两个function，作为一个obj
  // 也可以test:test, upper:upper
}
```
然后再index里面使用`a.test()`来使用对应的方法

3. 其他的方式
```javascript
exports.test = test
exports.upper = upper
```
也是index里面使用`a.test()`来使用对应的方法

# NPM

1. npm的使用
```javascript
npm init ( 会生成package.json)
npm install 包名 -g  (uninstall,update)
npm install 包名 --save-dev
npm list -g 
npm info
npm install md5@1
npm outdated
```

2. 使用案例
:::info

1. 在某个文件夹内路径的终端 使用`npm init`然后输入相关信息
2. 使用`install`命令来安装包名 例如 
`npm install md5`
`npm i md` 简写 i = install
3. json 中会记录安装信息, 在`dependencies : {}` 里面可以看到
4. 其实上述安装指令和下面这个一样:
`npm i md5 --save`
在早期版本`--save`是一定要加的 (现在可以省略)
5. `lock`文件是用来锁定版本, 可以解决版本更新出现问题
6. `-dev`:  在json里面的dependencies会变成 devDependencies, 只是记录 开发环境要用的
7. `-D -S` 分别是dev和save的对应
8. `npm uninstall [package name]`卸载
9. `-g` global, 全局安装
10. `-list`列举安装的模块 加`-g` 就是全部安装的, 不安装的就是local的
11. `npm info [package]` 模块信息
12. 指定版本安装`npm -i md5@1` 会安装`md5@1.0.1` 版本, 可以指定版本
13. `npm outdated`会检查所有的包的版本, 会显示什么东西有更新
:::

3. 版本管理或者传输
:::info

1. 获取对应的`package.json & package-lock.json`
2. 在对应路径的终端直接 `npm i`
:::
在dpendencies中, 版本号前`^` 和 `~` 是不一样的含义, 这里有一个演示
`"md5": "^2.3.0"`  `^` 会锁定大版本号, 而不是小版本, `2.1.0` 也会安装2.3.0
`"md5": "~2.3.0"`  `~` 会锁定前两个, 2 和 3 会锁定, 但最后一个0不会锁定
`"md5": "2.3.0"`  绝对保持这个版本
`"md5": "*"`  想要最新版本

4. **全局安装
**

# NRM & YARN
NPM的镜像, 国外资源会慢, 但是这个会更快
YARN 速度快,缓存每一个下载的包, 并行下载, 校验每个包的完整性
:::info
**YARN**

1. `npm install -g yarn` 全局安装 YARN
2. `yarn init` 开始新项目
3. `yarn add [packet]` 安装项目
`yarn add md5@1`安装指定版本
`yarn add [packet] --dev`指定开发分类
4. `yarn upgrade md5@2` 升级md5到2版本
5. `yarn remove md5` 删除md5包
6. 其他开发人员获取所有module:
`yarn install` 获取所有依赖包
:::
# ES模块化
我们想使用js的ES6模块化开发
在index.js 中：`import moduleA from './module/moduleA'`
在moduleA.js中：`export default moduleA`
但在node直接这样是不行的
:::tips
#### 所以：

1. 我们先创建package.json 在相对应文件夹`npm init`
2. 在`type:` 后面加上 `"module"`
3. 确保导入文件后缀名 `moduleA.js`而不是`moduleA`
:::

:::tips
#### 其他导出方式
`export{ moduleA, moduleB, ... }` 在moudleB.js 里面
`import {moduleB} from './module/moduleB.js` 在index.js 里面
:::
**不能与CommonJS共用！！！**


---

# 内置模块
## HTTP

1. 导入 `const http = require('http');`
2. 创建服务器
`http.createServer((req,res) =>{}).listen(3000,()=>{})`
第一个回调函数是用户访问会返回该函数
第二个回调函数是当server创建后会返回的
3. `node server.js` 就可以启动服务器的
4. 例子
```javascript
const http = require("http");

http.createServer((req,res){
  // req 接受浏览器的参数
  // res 返回渲染内容

  res.write("Hello World"); // 可以多次写入
  res.end(); // 表示写入内容已经结束，否则会一直超时
}).listen(3000,()=>{
  console.log("server started");
}
```

5. `end()` 中也可以放东西，例如一个字符串
6. 写HTML 页面：
```javascript
const http = require("http");

http.createServer((req,res){
  // 200 是状态码，404 就是错误状态，无法处理
  res.writeHead(200,{"Content-Type":"text/html"}); // 改html 为 plain 是普通文本，
  res.write('
  	<html>
      <b> text </b>
    </html>
  '); // 可以多次写入
  res.end(); // 表示写入内容已经结束，否则会一直超时
}).listen(3000,()=>{
  console.log("server started");
}
```

7. 使用UTF-8 进行编码, 在writeHead里面写上
`res.writeHead(200,{"Content-Type":"text/html; charset=utf-8"}`
8. 判断URL
```javascript
const http = require("http");

http.createServer((req,res){
    // console.log(req); // 会返回一个很大的东西
    // 使用req.url来获取路径 例如localhost:3000/api/b 返回 /api/b
    res.writeHead(renderStatus(req.url),{"Content-Type":"text/html; charset=utf-8"});
    res.write(renderHTML(req.url));
    

}).listen(3000,()=>{
    console.log("server started");
}

function renderStatus(url){
    var arr = ["/home", "/list"];
    return arr.includes(url)?200:404;
}

function renderHTML(url){
    switch(url){
        case "/home":
            // return home ...
        case "/list":
            // return list ...
    }
}
```
:::info
### 小知识:
`/favicon.ico`也是会返回的一个请求, 它是一个对网页图标的请求, 有可能会使用缓存图标
所以我们可以增加if条件来return, 终止访问缓存
`if(req.url==="/favicon.ico"){ //TODO 读取本地图标; return; }`
:::

9. 其他的开服务器方法,是一样的:
```javascript
var server = http.createServer()
server.on("request",(req,res)=>{method;})

server.listen(3000, 90=>{
	method;
})
```

10. 自动重启服务器 nodemon
`npm i -g nodemon` 可以使用这个包
`nodemon filename` 这样启动服务器会自动重启, 如果改变的文件
11. 自动重启服务器之 dev
`npm i -g node-dev`
`node-dev filename` 也可以自动重启
## URL模块
在上一节中存在问题, 当访问/api/b?id=2 时, 会报错, 但后面的query我们不希望它成为url的一部分
所以我们使用URL模块

1. URL模块
```javascript
var url = require("url");
// url 是模块
console.log(url.parse(req.url)); // 解析原来的url, 打印一个对象
//在这个对象中, pathname就是路径
url.parse(req.url).pathname;
```

2. URLobj 里面还有很多有用的, 比如除了pathname 的路径还有 query的内容等等
现在来说, urlobj 返回的值也只是query : 字符串, 我们想要一个**对象**, 什么对应什么, 这时候:
```javascript
var urlobj = url.parse(req.url, True);
```

3. 提供数据, 返回url

![image.png](https://cdn.nlark.com/yuque/0/2023/png/29051556/1679288873884-c88c7c50-d5f1-4b4b-b482-20083aaeab42.png#averageHue=%23d0d3d0&clientId=u01ddbde1-2916-4&from=paste&height=419&id=u15dbbf32&originHeight=419&originWidth=724&originalType=binary&ratio=1&rotation=0&showTitle=false&size=102889&status=done&style=none&taskId=u588c7229-b665-4e53-a243-53619e6c7f8&title=&width=724)

4. Resolve: URL拼接

![image.png](https://cdn.nlark.com/yuque/0/2023/png/29051556/1679288932428-59675eda-7d65-4d59-aece-bcfa37509dc1.png#averageHue=%23f7f6f5&clientId=u01ddbde1-2916-4&from=paste&height=142&id=uba024bcd&originHeight=142&originWidth=705&originalType=binary&ratio=1&rotation=0&showTitle=false&size=63377&status=done&style=none&taskId=u86d227a0-d2be-4062-a5a7-f765b1dbc79&title=&width=705)

- 如果最后没有斜杠, arg2会替换arg1 的最后一个, 比如a的four会替换three
- b会替换第一个斜杠最后的所有东西, 也就是这里.com/ 后面的所有, 如果有.com/a/b/c, 也会变成.com/two
- c 就是正常的one/two

5. **新版**
有新的API, prase等等都是旧版了
```javascript
const myURL = new URL(req.url, 'http://127.0.0.1:3000');
var pathname = muURL.pathname; // pathname, not including query
var query = myURL.searchParams; // queries of the url
for(var [key,value] of myURL.searchParams){ do things to key and value}
for(var obj of myURL.searchParams){do things to obj}

var b = new URL('/one','http://127.0.0.1:3000/a/b') // 这个就是URL拼接, 和之前相反
url.format(myURL,{一些设置 bool})
// 包括一些默认值 unicode:false; auth:true; fragment:true; search: true;

const {fileURLToPath} = require('url');
fileURLToPath('filepath') //对windows的路径可以转义
```
## Query String模块

1. 引用模块 `var querystring = require("querystring")`
2. 使用 `var obj = querystring.parse(str)` str是我们想转换的query字符串
3. 也可以反过来，有一个obj也可以转换成form表单格式
```javascript
var myobj = {
  a:1,
  b:2,
  c:3
}

var mystr = querystring.stringify(myobj)
// a=1&b=2&c=3
```

4. escape/unescape 是用来转义的
## HTTP-JSONP
**jsonp 接口：**
```javascript
http.createServer((req,res)=>{
  var urlobj = url.parse(req.url, true)
  console.log(urlobj.query.callback)
  switch(urlobj.pathname){
    case "/api/aaa":
      res.end(`$(urlobj.query.callback}（${JSON.stringify({
              name:"kerwin",
              age:100
			})})`)
    	break;
		default:
    	res.end("404")
	}
}).listen(3000)

```
## HTTP-JSON-CORS 跨域
如果本机的3000端口网页访问5000端口的数据库, 那就会出现跨域问题, 所以可以在write head 里面添加一些东西解决:
```javascript
res.writeHead(200,{
  "Content-Type":"application/json;charset=utf-8",
  "access-control-allow-origin":"*"  // 允许所有域访问  cors 头
})
// 本文件是一个json接口
```
## HTTP-GET
将NODE作为中间层, 可以从其他数据库获取api, 然后转发给前端, 可以使用get 请求api
![](https://cdn.nlark.com/yuque/0/2023/jpeg/29051556/1680755382490-359d8342-4ce8-4bd5-9bc7-c1ccafdcc54a.jpeg)
```javascript
var url = require("url");
var http = require("http"); 
var https = require("https"); // 要注意 https

http.createServer((req,res)=>{
  var urlobj = url.parse(req.url, true)
  console.log(urlobj.query.callback)
  switch(urlobj.pathname){
    case "/api/aaa":
      httpget(res)
    	break;
		default:
    	res.end("404")
	}
}).listen(3000)

function httpget(response){
  var data = ""
	https.get('https://i.maoyan.com/api...',(res)=>{
    res.on("data",(chunk)=>{	// 不断的数据, 一部分一部分返回
    	data += chunk
    })
    res.on("end",()=>{ // 没有数据后, end是表示所有都收集完了
    	console.log(data) // 控制台print
      response.end(data) // 传回data
    })
  })
}

// 耦合度过高, 可以这样写, 在10行的代码改为:
httpget((data)=>{
  res.end(data)
})
// 然后httpget里面的17, 25行改为:
function httpget(cb)  //cb callback 回调函数
cb(data)
```
### ajax 例子
```html
<script> // 有跨域问题
  fetch("https://i.maoyan.com/api...").then(res=>res.json()).then(res=>{
    console.log(res)
  })
</script>

<script> // 没有问题  看上面的代码get
  fetch("http://localhost:3000/api/aaa").then(res=>res.json()).then(res=>{
    console.log(res)
  })
</script>
```
## HTTP-POST
```javascript
var url = require("url");
var http = require("http"); 
var https = require("https"); // 要注意 https

http.createServer((req,res)=>{
  var urlobj = url.parse(req.url, true)
  console.log(urlobj.query.callback)
  switch(urlobj.pathname){
    case "/api/aaa":
      httppost((data)=>{
        res.end(data)
      })
    	break;
		default:
    	res.end("404")
	}
}).listen(3000)

function httppost(cb){
  var data = ""
  var options = {
    hostname:"m.xiaomiyouping.com", // 只写域名
    port:"443", // https
    path:"/mtop/market/search/placeHolder", // 路径
    method:"POST",
    header:{
      "Content-Type":"application/json"
      // 还有一种格式: "x-www-form-urlencoded"
      // 那么write里面就是类似 "name=kerwin&age=100..."
    }
  }
  var req = http.request(options,()=>{
  	res.on("data",chunk=>{
      data+=chunk
    })
    res.on("end",()=>{
      cb(data)
    })
  })

  req.write(JSON.stringify([{},{"baseParam":{"upClient":1}}]))
  req.end()
}


```
## HTTP-爬虫
```javascript
var url = require("url");
var http = require("http"); 
var https = require("https"); // 要注意 https
var cheerio = require("cheerio")

http.createServer((req,res)=>{
  var urlobj = url.parse(req.url, true)
  console.log(urlobj.query.callback)
  switch(urlobj.pathname){
    case "/api/aaa":
      httpget((data)=>{
        res.end(spider(data)) // !! 返回过滤后的信息
      })
    	break;
		default:
    	res.end("404")
	}
}).listen(3000)

function httpget(cb){
  var data = ""

  // 这里是爬虫开始的地方!!
  // 现在我们直接选择一个页面, 而不是选择它的api
  https.get('https://i.maoyan.com',(res)=>{
    res.on("data",(chunk)=>{	// 不断的数据, 一部分一部分返回
    	data += chunk
    })
    res.on("end",()=>{ // 没有数据后, end是表示所有都收集完了
    	console.log(data) // 控制台print
      cb(data) // 传回data
    })
  })

}

function spider(data){
  // 分析html文件
  // cheerio 模块 可以分析  使用 npm init -> npm i --save cheerio
  let $ = cheerio.load(data) //$其实也可以是其他名字
  let $movielist = $(".column.content")
  let movies = []

  $movielist.each((index, value)=>{
    movies.push($(value).find(".title").text())
    // 或者
    movies.push({
      title:$(value).find(".title").text(),
      grade:$(value).find(".grade").text(),
      actor:$(value).find(".actor").text()
    })
  })
	return JSON.stringify(movies)
}
```
## Event module
```javascript
const EventEmitter = require("events")
const event = new EventEmitter()

event.on("play",(data)=>{
  console.log("触发事件",data)
})

event.emit("play")

setTimeout(()=>{
  event.emit("play",1111) // 1111会被返回到data
},2000) // 两秒后触发，setTimeout是延迟

```
```javascript
var url = require("url");
var http = require("http"); 
var https = require("https"); // 要注意 https
var EventEmitter = require("events")
var event = null


http.createServer((req,res)=>{
  var urlobj = url.parse(req.url, true)
  console.log(urlobj.query.callback)
  switch(urlobj.pathname){
    case "/api/aaa":
      event = new EventEmitter()
      event.on("play",()=>{
        console.log("something")
        res.end(data)
      })
      httpget()
    	break;
		default:
    	res.end("404")
	}
}).listen(3000)

function httpget(){
  var data = ""
  var req = http.request(options,()=>{
  	res.on("data",chunk=>{
      data+=chunk
    })
    res.on("end",()=>{
      event.emit("play",data)
    })
  })

  req.write(JSON.stringify([{},{"baseParam":{"upClient":1}}]))
  req.end()
}


```
## FS文件操作模块
```javascript
const fs = require("fs")

// 创建目录  若已经存在,err会报错  /当前文件夹/avatar
fs.mkdir("./avatar",(err)=>{
	//consoe.log(err)
  if(err && err.code==="EEXIST"){ // 查看err的var,里面有err的code
    console.log("目录已存在")
  }
})
```
```javascript
const fs = require("fs")

// 改名
fs.rename("./avatar","./avatar2",(err)=>{
	//consoe.log(err)
  if(err && err.code==="ENOENT"){
    console.log("目录不存在")
  }
})
```
```javascript
const fs = require("fs")

// 删除dir
fs.rmdir("./avatar2",(err)=>{
	//consoe.log(err)
  if(err && err.code==="ENOENT"){
    console.log("目录不存在")
  }
})

// 不能移除里面有文件的文件夹
```
```javascript
const fs = require("fs")

//不保留, 重写已存在文件
fs.writeFile("./avatar/a.txt","hello world", (err)=>{
  console.log(err)
})
```
```javascript
const fs = require("fs")

//追加内容
fs.appendFile("./avatar/a.txt","hello world", (err)=>{
  console.log(err)
})
```
```javascript
const fs = require("fs")

//error first 风格, 错误在前
fs.readFile("./avatar/a.txt", (err,data)=>{
  if(!err){
    console.log(data.toString("utf-8"))
  }
})

//也可以提前用utf8读取
fs.readFile("./avatar/a.txt", "utf-8", (err,data)=>{
  if(!err){
    console.log(data)
  }
})
```
```javascript
const fs = require("fs")

// 删除文件
fs.unlink("./avatar/a.txt", (err)=>{
  console.log(err)
})
```
```javascript
const fs = require("fs")

// 读取文件夹所有文件
fs.readdir("./avatar", (err,data)=>{
  if(!err){
    console.log(data)  // 返回一个数组, 包括文件夹的所有文件
  }
})
```
```javascript
const fs = require("fs")

// 查看一个文件的信息
fs.stat("./avatar", (err,data)=>{
  if(!err){
    console.log(data)  // 返回一个对象
    //里面有一个函数叫 isFile(), 还有isDirectory()
  }
})
```
```javascript
const fs = require("fs")

// 读取文件夹所有文件
fs.readdir("./avatar", (err,data)=>{
  data.forEach(item=>{
    fs.unlink(`./avatar/${item}`,(err)=>{}) // 这个是字符串模板
  })
	fs.rmdir("./avatar")
})
// 有可能删不掉目录, 因为这个unlink操作是异步进行的
```
```javascript
const fs = require("fs")
fs.mkdirSync("./avatar")  
//阻塞后面代码的执行

// 这种方法就一般要try 和catch error 防止挂机

//所以 同步的删除为:
const fs = require("fs")

// 读取文件夹所有文件
fs.readdir("./avatar", (err,data)=>{
  data.forEach(item=>{
    // 同步执行删除, 阻塞下面代码
    fs.unlinkSync(`./avatar/${item}`,(err)=>{}) // 这个是字符串模板
  })
  
	fs.rmdir("./avatar")
})
// 但是这种方法会导致服务器卡住, 所以还是异步比较多, 下面有一种方法
```
```javascript
const fs = require("fs").promises

// 基于 .then 的异步方法
fs.mkdir("./avatar").then(data=>{
  console.log(data)
})

// 所以 异步promises的recursion删除为:
fs.readdir("./avatar").then(async (data)=>{
  let arr = []
  data.forEach(item=>{
    arr.push(fs.unlinkSync(`./avatar/${item}`)) // push 对象到arr
  })
  // promise.all([]) 可以等待数组所有的对象后再执行, 等待异步
  await Promise.all(arr)
	await fs.rmdir("./avatar")
}).catch(...)


// 更简单的写法  将10-17行改为
	await Promise.all(data.map(item=>fs.unlink(`./avatar/${item}`)))
	await fs.rmdir("./avatar")
```
## Stream流模块
注意：http中的第二个参数res本身就是一个可写流，也就是可以用pipe（res）写入res的数据
```javascript
const fs = require('fs')
const rs = fs.creatReadStream("./1.txt","utf-8")//读取1.txt

rs.on("data",(chunk)=>{
  console.log(chunk)
})

rs.on("end",()=>{
  console.log("end")
})

rs.on("error",(err)=>{
  console.log("error")
})
// 占用内存小
```
```javascript
const fs = require('fs')
const ws = fs.createWriteStream("./2.txt","utf-8")
ws.write("11111")
ws.write("222")
ws.write("333")
ws.end()
//写入文件
```
```javascript
// 如果读取速度过快 而写入文件过慢, 使用read和write会出现一些问题
// 解决方法是管道 pipe

const fs = require('fs')
const readStream = fs.createReadStream("./1.txt")
const writeStream = fs.createWriteStream("./2.txt")

readStream.pipe(writeStream)
//这样子运行后就可以直接复制1 到2 了
```
## Zlib 模块
当html 或 其他静态文件比较大的时候, gzip压缩可以更快传输
普通写法： without gzip
![截屏2023-04-09 下午5.14.19.png](https://cdn.nlark.com/yuque/0/2023/png/29051556/1681085666998-2ac2da1c-b0e6-468b-af61-e48ad3471e8a.png#averageHue=%232f2e27&clientId=u7d5ed5f5-c11c-4&from=drop&id=uab663b25&originHeight=528&originWidth=1068&originalType=binary&ratio=2&rotation=0&showTitle=false&size=336429&status=done&style=none&taskId=u094c5872-daf6-4fe6-9abd-0704195bce2&title=)
用了压缩的 gzip
![截屏2023-04-09 下午5.17.26.png](https://cdn.nlark.com/yuque/0/2023/png/29051556/1681085850934-f85d8174-2627-4645-84ae-f5430c3e13de.png#averageHue=%23313028&clientId=u7d5ed5f5-c11c-4&from=drop&id=u55ad8d64&originHeight=472&originWidth=1136&originalType=binary&ratio=2&rotation=0&showTitle=false&size=392788&status=done&style=none&taskId=u0fe2401e-f1e2-46b0-ae6d-4cd512a816d&title=)
注意，head要变！！！
## Crypto模块 (p26 待学习)
这个模块就是提供加密和哈希的算法, 纯js是不可能实现的, 但node底层是c++所以用这个接口就很方便
# 路由(待学习)
# EXPRESS框架
记得创建项目要用npm init

1. 安装 `npm install express`
## 创建服务器
```javascript
// express 是个函数, 可以call
const app = express()

// 直接监听 / 的url
app.get("/",(req,res)=>{
  // res.write("Hello World")
  // res.end()
  // 其实这么麻烦, 有新的封装
  // 例如 1.
  //res.send("hello")
  // 可以返回html 或者对象 都可以自动识别
  // res.send(`
  //     <h1> hi2 </h1>
  // `)

  res.send({"name":"kerwin", age:100})
})

app.get("/login",(req,res)=>{
  res.send("login")
})

app.listen(3000,()=>{
  console.log("server starts")
})
```
## 基本路由
### 不同字符串
:::info

1. ?  匹配可选, abc 或者abcd 下面例子
`app.get('/ab?cd', function(req,res){ res.send(data); });`
2. :  占位符  例如 :id  id只是代表一个变量, 不是固定, 会满足 /ab/*** 但不满足 /ab
`app.get("/ab/:id",(req,res)=>{ res.send(data); });`

可以两个 `/ab/:id1/:id2`这样子

3. + 多个字符, ab+cd就是b可以重复多次
4. * 任意字符, ab*cd就是abcd任意多个字符都可以
5. () 就是要么有或者没有 ab(cd)?e 就是 abe 或者 abcde
:::
### 可以正则表达式

1. "/.*fly$/" 必须以fly结尾才能
### 中间件
app.get 里面可以放多个函数, 之前的回调函数我们在express中称为中间件, 可以这样写:
`app.get("/home",(req,res)=>{ // 验证用户token },(res.req)=>{// 查询数据库 返回内容})`

**注意点: 第三函数**
**我们需要第三函数来让它继续执行下一个中间件, 否则会卡在上一个**
`app.get("/home",(req,res,next)=>{ next();// 验证用户token },(res.req)=>{// 查询数据库 返回内容})`
**数组写法:**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/29051556/1681627881192-badbcb08-48e1-4632-abe5-f144886be74a.png#averageHue=%23f8f8f8&clientId=ubeb5d976-14c9-4&from=paste&height=361&id=u722cfbfc&originHeight=361&originWidth=729&originalType=binary&ratio=1&rotation=0&showTitle=false&size=68126&status=done&style=none&taskId=ud9da0558-1558-45b8-8e90-43dec718ca1&title=&width=729)
还可以 app.get("/list",[func1], (req,res)=>{ sth })写自己的函数
### 让中间件互相通信, 传输变量
用res的变量加属性:
res.kewin = "这是计算结果"
然后再在下一个function 调用 res.kewin
## 路由中间件
![image.png](https://cdn.nlark.com/yuque/0/2023/png/29051556/1681704783247-61459448-b27b-4e72-b4b9-f945abd2c774.png#averageHue=%23f3f2f2&clientId=u0f03676d-67a4-4&from=paste&height=613&id=u38e144a4&originHeight=613&originWidth=1259&originalType=binary&ratio=1&rotation=0&showTitle=false&size=193761&status=done&style=none&taskId=u12104d91-5626-4d74-85d9-3c7c0a57120&title=&width=1259)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/29051556/1681704831876-bf9a9ef7-7ce1-45df-9005-c040de7b5484.png#averageHue=%23f8f6f6&clientId=u0f03676d-67a4-4&from=paste&height=415&id=u3992d1c7&originHeight=415&originWidth=1244&originalType=binary&ratio=1&rotation=0&showTitle=false&size=115526&status=done&style=none&taskId=ua25efb97-53e9-4ce1-a031-e84f4cebedb&title=&width=1244)
### 应用级别中间件:
将中间件绑定到app对象, 使用app.use() 和 app.METHOD() , METHOD是处理HTTP请求的方法 例如GET PUT POST (小写)
```javascript
const func1 = (req,res,next)=>{}
app.get("/login",[func2])

// use是有先后顺序的, 在第二行之后的页面才会进行func1, 例如login不会首先运行func1但home会
// func1例如验证token
app.use(func1)

app.get("/home",[func2])
app.get("/else",(req,res,next)=>{})


// 还可以分开响应
app.use("/home",func1) //限制只在home
```
### 路由级别中间件:
不挂在app对象上的中间件
```javascript
//app.use("/", 路由模块)
// 文件顺序
// index2.js
// router2 (directory)
//   |---IndexRouter.js
const express = require("express")
const app = express()
const IndexRouter = require("./router2/IndexRouter")

app.use(function(req,res,next){
  console.log("验证token")
  next()
})

// 在/api这个情况下找router, 这个例子只能访问 /api/home 或者 /api/login
app.use("/api", IndexRouter)

app.listen(3000,()=>{
  console.log("启动")
})
```
```javascript
const express = require("express")

const router = express.Router()

router.get("/home",(req,res)=>{
  res.send("home")
})

router.get("/login",(req,res)=>{
  res.send("login")
})

moudule.exports = router
```
如果全部放api一个router太多, 我们可以这样写
```javascript
app.use("/home",HomeRouter)
app.use("/login",LoginRouter)
```
### 错误中间件
建议放在最后
```javascript
//前面都匹配不到才会运行, 所以要放到最后
app.use((req,res)=>{
  res.status(404).send("丢了")
})
```
### 内置中间件(简略)
express.static 是 express的唯一内置的中间件
他可以把文件夹转换为staic文件夹
```javascript
app.use(express.static('uploads'))
```
### 第三方中间件(简略)
安装所需功能的node 模块然后再应用中加载
![image.png](https://cdn.nlark.com/yuque/0/2023/png/29051556/1681708157398-d4bb3772-b0ae-482a-8181-042848d4fb5e.png#averageHue=%23f5f4f4&clientId=u0f03676d-67a4-4&from=paste&height=377&id=u24b3d347&originHeight=377&originWidth=864&originalType=binary&ratio=1&rotation=0&showTitle=false&size=142625&status=done&style=none&taskId=ue320623d-ab41-42e1-b58e-5ca36af4a0b&title=&width=864)
## 获取请求参数
我们想发送get和post 比如 
Get请求: /login?username=kerwin&password=123456 
```javascript
//直接用req.query 就可以获取请求
// 在router或者app中可以用req.query
router.get("/",(req,res)=>{
	console.log(req.query)
})
```
POST这个也很简单, 只需要配置一下中间件
在老版本中, 需要安装body-parser 但现在不需要了
```javascript
// 放在app的文件里 也可以放在本文件, 但是放在开头
app.use(express.urlencoded({extended:false})) 
//post参数获取url form编码
// 就是?username=kerwin&password=123456这样的 key=value&key=value...

//使用post来获取时会走这个路径
router.post("/",(req,res)=>{
  console.log(req.body) // 必须配置好中间件 配置解析post的中间件
  res.send({ok:1})
})
```
还有application json方式发送请求
```javascript
//前端发送json的样式
{"username":"kerwin", "password":"123"}

//这时候就要使用header 
Content-Type:application/json  // 后面会学怎么改

// 配置文件
app.use(express.json()) // 可以响应post请求
// 可以配置多个, 同时json和url form编码
app.use(express.urlencoded({extended:false})) 
// 放一块就可以 但是放在路径前
```
## 静态资源
使用`express.static`
```javascript
const express = require("express")
const app = express()
const HomeRouter = require("./route/HomeRouter")

//配置静态资源
app.use(express.static("public")) //当前路径下的文件夹 public

app.use(express.json()) // 可以响应post请求 json
app.use(express.urlencoded({extended:false})) // 可以响应post请求 url form

app.use(function(req,res,next){
  console.log("验证token")
  next()
})

app.listen(3000,()=>{
  console.log("启动")
})
```
使用: 比如localhost:3000/login.html 就可以直接访问静态文件夹的login.html文件, 不需要路径文件夹名字
```javascript
// 可以用这种方法访问固定路径
app.use('/static',express.static('public'))
//这样子只有加了路径后才能访问某个html
// /static/login.html
```
html使用css静态文件
```javascript
//直接使用静态文件的目录就可以, 不需要加static
// 例如有home.css在静态文件夹static下, 直接link home.css
<link rel="stylesheet" href="/home.css">
```
## 服务端渲染和客户端渲染

前后端分离： 客户端渲染
```html
//前端发送ajax请求
<script>
  fetch("/home/list").then(res=>res.json()).then(res=>{
    console.log(res)
  })
</script>

```
后端渲染：服务端渲染（方便爬虫）
### 服务端渲染（暂时跳过）P38
## 生成器
我们不想说通过index.html或者什么文件名来返回, 而是路径返回，路由
还是要用EJS模块
# MONGO DB
![截屏2023-04-17 上午11.47.55.png](https://cdn.nlark.com/yuque/0/2023/png/29051556/1681757294361-73e3c407-15b7-499e-897a-f9437235fa4f.png#averageHue=%23f8f8f8&clientId=ua1963d8a-7d7e-4&from=drop&height=471&id=u97467808&originHeight=1318&originWidth=1162&originalType=binary&ratio=2&rotation=0&showTitle=false&size=701703&status=done&style=none&taskId=ud2f482c4-b8aa-4a34-86dc-ed61716fcb3&title=&width=415)

1. mongodb 要先加collection再show才能显示新增的db，因为原本没文件
## nodeJS 操作
常见案例之一注册

1. 前提：安装 mongoose 模块来
`npm i mongoose`
```javascript
require("../config/db.config.js")
```
```javascript
//前提是在主js里面路由到 /api
//主js还要包括 require 这个router
const UserModel = require('../model/UserModel');

router.post("/user/add",(req,res)=>{
  console.log(req.body)//post请求的body
  //插入数据库
  // 1. 创建一个模型(user,可以限制类型)，一一对应集合(users)
  // user.create(insert) user.find 它会提供一些方法
  // 建议不放在这个文件中，而是做成模块

  const {username,password,age} = req.body;

  //前面的create创建的是一个promise对象，如果成功会运行then
  UserModel.create({
    username,password,age
    // 等于 username:username, password:password 只不过可以简写（待研究）
  }).then(data=>{
    console.log(data)
  })
  
	res.send({
    "ok":1
  }
})
```
```javascript
// user模型
// 多次链接就不需要加上connect，因为是同一个实例
const mongoose = require("mongoose")
const Schema = mongoose.Schema //只是为了更好看方便
//限制模型type
const UserType = {
  username:String,
  password:String,
  age:Number
}
//通过schema限制字段，多了不行，type不对也不 行
const UserModel = mogoose.model("user",new Schema(UserType))
//模型user将会对应users 集合，他会自动创建users，自动加s
//如果取名为aa 则会叫aas

module.exports = UserModel
```
也可以创建小模块来做conf
```javascript
//链接数据库
const mongoose = require("mongoose")

//固定的是mongodb然后后面的sandwich_db是数据库名字
//如果没有会自动创建
mongoose.connect("mongodb://127.0.0.1:27017/sandwich_db")
```
### post Json表单案例
```html
<div>
  <div>username: <input id="username"></div>
  <div>password: <input type="password" id="password"></div>
  <div><button id="register">Submit</button></div>
</div>

<script>
  var register = document.querySelector("#register")
  var username = document.querySelector("#username")
  var password = document.querySelector("#password")

  register.onclick = ()=>{
    console.log(username.value, password.value)
    fetch("http://localhost:3000/api/login",{
      method:"POST",
      body:JSON.stringify({
        username:username.value,
        password:password.value
      }),
      headers:{
        "Content-Type":"application/json"
      }
    }).then(res=>res.json()).then(res=>{
      console.log(res)
    })
  }
</script>
```
## nodeJS 操作2
### 更新和删除数据
HTML:
```javascript
<div>
  <div>username: <input id="username"></div>
  <div>password: <input type="password" id="password"></div>
  <div><button id="register">Submit</button></div>
  <div><button id="update">Submit</button></div>
  <div><button id="delete">Submit</button></div>
</div>

<script>
  var register = document.querySelector("#register")
	var update = document.querySelector("#update")
	var deletebutton = document.querySelector("#delete")
  var username = document.querySelector("#username")
  var password = document.querySelector("#password")

  register.onclick = ()=>{
    console.log(username.value, password.value)
    fetch("http://localhost:3000/api/login",{
      method:"POST",
      body:JSON.stringify({
        username:username.value,
        password:password.value
      }),
      headers:{
        "Content-Type":"application/json"
      }
    }).then(res=>res.json()).then(res=>{
      console.log(res)
    })
  }

  update.onclick = ()=>{
    // 我们可能会用不同的id作为url,动态url
    // 例如 api/update/someid:125131251
    fetch(`http://localhost:3000/api/update/${id}`,{
      method:"POST",
      body:JSON.stringify({
        username:username.value,
        password:password.value
      }),
      headers:{
        "Content-Type":"application/json"
      }
    }).then(res=>res.json()).then(res=>{
      console.log(res)
    })
  }

  deletebutton.onclick = ()=>{
    // 我们可能会用不同的id作为url,动态url
    // 例如 api/update/someid:125131251
    // 默认get请求
    fetch(`http://localhost:3000/api/delete/${id}`)
      .then(res=>res.json())
      .then(res=>{
      console.log(res)
    })
  }
</script>
```
router:
```javascript
//前提是在主js里面路由到 /api
//主js还要包括 require 这个router
const UserModel = require('../model/UserModel');

// 回顾之前 : 可以用于占位
router.post("/user/update/:myid",(req,res)=>{
  console.log(req.body, req.params)//post请求的body  post 还有参数!!
  //params 占位符:数据
  const {username,password,age} = req.body;

  //前面的create创建的是一个promise对象，如果成功会运行then
  UserModel.updateOne({
    _id:req.params.myid
  },{
    username
  }).then(data=>{
  	res.send({
      "ok":1
    }
  })
})

router.post("/user/delete/:myid",(req,res)=>{
  console.log(req.body, req.params)//post请求的body  post 还有参数!!
  //params 占位符:数据
  const {username,password,age} = req.body;

  //前面的create创建的是一个promise对象，如果成功会运行then
  UserModel.deleteOne({
    _id:req.params.myid  //_id 要和数据库的 域 一样
  }).then(data=>{
  	res.send({
      "ok":1
    }
  })
})
```
### 查询
```javascript
<table>
  <thead>
    <tr>
      <td>id</td>
    	<td>username</td>
  	</tr>
  </thead>
	<tbody>
  </tbody>

fetch("http://localhost:3000/api/list")
.then(res=>res.json())
.then(res=>{
  console.log(res)
  var tbody = document.querySelector("tbody")
  tbody.innerHTML = res.map(item=>`
    <tr>
      <td>${item._id}</td>
      <td>${item.username}</td>
    </tr>
  `).join("")
})
```
```javascript
router.get("/user/list",(req,res)=>{
  UserModel.find().then(data=>{
    res.send(data)
  })
})
```
### 查指定字段
```javascript
router.get("/user/list",(req,res)=>{
  // 只查询username 和 age 字段
  UserModel.find({},["username","age"]).then(data=>{
    res.send(data)
  })
})
```
### 排序
```javascript
router.get("/user/list",(req,res)=>{
  // 只查询username 和 age 字段
  // age 排序 1 为正序 -1 为倒序
  UserModel.find({},["username","age"]).sort({age:1}).then(data=>{
    res.send(data)
  })
})
```
### 分页功能, 指定数据
比如要第二页 每页十条
`api/list?page=2&limit=10`
```javascript
router.get("/user/list",(req,res)=>{
  console.log(req.query) // 会获得{page:1,limit:2}
  const {page,limit} = req.query
  UserModel.find({},["username","age"])
    .sort({age:1})
    .skip((page-1)*limit)
    .limit(limit)
    .then(data=>{
    res.send(data)
  })
})
```
# 接口规范
## RESTful 架构
**希望url地址中只包含名词表示资源, 使用http动词表示进行操作资源**
```javascript
GET /blog/Articles  //获取所有文章
POST /blog/Articles	//添加一篇文章
PUT /blog/Articles 	//修改一篇文章
DELETE /blog/Articles/1	//删除一篇文章
//如果有限制就可以加上?和键值对
/api/user?page=1&limit=10
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/29051556/1681883434352-c981c844-cc77-479f-ab14-cdf92e12fa76.png#averageHue=%23fcfcfb&clientId=ucb6a7d5e-b401-4&from=paste&height=398&id=u291bacdd&originHeight=430&originWidth=737&originalType=binary&ratio=1&rotation=0&showTitle=false&size=129850&status=done&style=none&taskId=u122a1715-e25e-406f-b0c9-758a4345e65&title=&width=683)
# 业务分层
router里面实现所有逻辑都在里面, 就会很乱
![image.png](https://cdn.nlark.com/yuque/0/2023/png/29051556/1681883801152-b815425a-d9e9-486f-9450-bceed0adbbbf.png#averageHue=%23f5f5f5&clientId=ucb6a7d5e-b401-4&from=paste&height=701&id=u69dc531c&originHeight=701&originWidth=1148&originalType=binary&ratio=1&rotation=0&showTitle=false&size=191653&status=done&style=none&taskId=u70f2646c-66d3-4e82-8fbd-d5cb2179bff&title=&width=1148)
controller: 就是获取数据和返回数据比如 ok:1 的过程, 比如const {username, password, age} = req.body
model: 就是处理增删改查的过程逻辑 (和数据库模型文件夹名字重复, 所以可以改名字, 有人叫**service**)
所以有以下文件夹:
index.js
|---routes
|---controllers
|---services
|---models
### 案例: 更改post请求
```javascript
router.post("/user/add",(req,res)=>{
  console.log(req.body)

  const {username,password,age} = req.body;

  UserModel.create({
    username,password,age
  }).then(data=>{
    console.log(data)
  })
  
	res.send({
    "ok":1
  })
})
```
#### 改为以下:
```javascript
const UserController = require("../controllers/UserController")
router.post("/user",UserController.addUser)
```
```javascript
const UserService = require("../services/UserService")

const UserController = {
  // 注意!!! async
  addUser:async (req,res)=>{
  	console.log(req.body)
 	 	const {username,password,age} = req.body;

    // 注意!!! await
    await UserService.addUser(username,password,age)

    // 为了防止异步导致直接发出, 我们需要用到await 和 async
    res.send({
    "ok":1
  	})
  }
}
module.exports = UserController
```
```javascript
const UserModel = require("../models/UserModel") // 教程用了model

const UserService = {
	addUser:(username,password,age)=>{
    return UserModel.create({
    	username,password,age
  	})
}

module.exports = UserService
```
# 登录鉴权 Session和Cookie
## 登录流程
```javascript
//和前面一样的post请求 然后fetch传递参数
```
```javascript
// 添加路由 api login的router
```
```javascript
const express = require("express");
const router = express.Router();
...

// api/login
// 看之前的结构
router.get('/login',UserController.login)
```
```javascript
login: async(req,res)=>{
  const {username,password} = req.body
  const data = await UserService.login(username, password)
  if(data.length===0){
    res.send({ok:0})
  }else{
    res.send({ok:1})
  }
  
```
```javascript
login: (username, password)=>{
  return UserModel.find({username,password})
}
```
## 登录鉴权: Cookie&Session
只用Cookie会导致后端无法直接注销, 被伪造后会很麻烦
Session最大的特点就是可以存在后端, 可以随时后端注销
![image.png](https://cdn.nlark.com/yuque/0/2023/png/29051556/1682055370071-2d8cc882-d930-4808-b334-a24db964ac40.png#averageHue=%23f8f5f5&clientId=uf7779cf3-94d6-4&from=paste&height=697&id=u08446c84&originHeight=697&originWidth=1207&originalType=binary&ratio=1&rotation=0&showTitle=false&size=173007&status=done&style=none&taskId=u25594208-708e-46c2-b212-6cc2c7ec200&title=&width=1207)
### 使用express-session 模块

1. 下载模块
`npm i express-session`
2. 添加中间件 [cookie可以在网页->检查->application里面看到]
```javascript
const session = require("express-session");
...
// 放在login前面
app.use(session({
  //注册session中间件
  name:"sandwichsystem", //可选
  secret:"flysandwichsecret", //必须,秘钥可以瞎写这样别人就猜不出来,实行编码
  cookie:{
    maxAge:1000 * 60 * 60, //必须,1000毫秒就是1秒 * 60sec * 60min
    secure:false //必须,在http中可以获取, 若true的话就只能用https
  },
  resave:true, //必须 如果在maxAge内任然操作,我们不想过期,只是闲置一个小时才想过期
  // 设置为true意味着,如果有动作,那么这个maxAge就重新计时
  // resave是重新设置session后会自动计算过期时间
  saveUninitialized:true //一开始就给你一个cookie, false就是不给cookie,一般没什么影响,因为我们会在后端check这个cookie
}))
```

3. 现在我们访问后获得一个无效的cookie, 在登录时我们设置session
```javascript
login: async(req,res)=>{
  const {username,password} = req.body
  const data = await UserService.login(username, password)
  if(data.length===0){
    res.send({ok:0})
  }else{
    //设置session req.ression是一个对象,他根据cookie值匹配
    req.session.user = data[0] //设置session对象,挂载一个额外字段 对应data0也就是username
    res.send({ok:1})
  }
  
```

4. 渲染其他路径前, 我们想判断req.session有没有user属性
```javascript
router.get('/',function(req,res,next){
  if(req.session.user){
    res.render('index',{title:'Express'});
  }else{
    res.redirect("/login")
  }
```

5. Session 默认存在内存中, 所以node服务器重启后就会刷新内存, session就会丢失, 一会可以存到数据库
### 弊端

1. 当cookie过期后, 仍然留在主页面, 主页面可能还是可以用, 但是没有重新登录
**也就是没有判断接口**解决方法: 应用级别中间件**
```javascript
//在session模块下设置一个中间件
app.use((req,res,next)=>{ // next 是必须的, 放行下一个
  // 排除login相关路由和接口
  if(req.url.includes("login")){
    next()
  }
  
	if(req.session.user){
    next()
  }else{
    res.redirect("/login")	//但是这样子也把login拦住了 所以要判断url
    // 前端如果发ajax请求,那么后端的重定向是不好用的 比如fetch
    // 所以前端负责跳转比较方便
    // 可以判断接口或者不是接口, 是接口返回错误码,否则重定向
    // 纯前后端就返回一个错误码就可以了
    res.status(401).json({ok:0})
  }
})
```
## 为什么后端自动获取cookie
因为浏览器自动发送
而且express-session 组件解析和设置已经帮我们解决了
## 强制销毁session, 退出按钮功能的实现

1. 在html中有一个退出登录的按钮
```javascript
<button id="exit"> exit</button>
<script>
  var exit = document.querySelector("#register")
  exit.onclick = ()=>{
    fetch("/api/logout").then(res=>res.json()).then(res=>{
      if(res.ok===1){
        location.href="/login"
      }
      
    })
</script>

```

2. 在api中写接口
```javascript
router.get("/logout",(req,res)=>{
	req.session.destroy(()=>{
  	res.send({ok:1})
  })
}) //把session摧毁后, cookie正确也无法进来
```
## 过期时间没有更新?
在使用的时候仍然不能更新, 为什么resave没有更新呢? 因为resave是**重新设置session后会自动计算过期时间**
**解决:**

- 在中间件中重新设置session
```javascript
//在session模块下设置一个中间件
app.use((req,res,next)=>{ // next 是必须的, 放行下一个
  // 排除login相关路由和接口
  if(req.url.includes("login")){
    next()
  }
  
	if(req.session.user){
    req.session.mydate = Date.now() // 每次访问改变一下值, 过期时间就会更新
    next()
  }else{
    res.status(401).json({ok:0})
  }
})
```
## 如何储存session到数据库? 不存在内存?

- connect-mongo 中间件 `npm i connect-mongo`
```javascript
var MongoStore = require("connect-mongo");

app.use(session({
  //注册session中间件
  name:"sandwichsystem", //可选
  secret:"flysandwichsecret", //必须,秘钥可以瞎写这样别人就猜不出来,实行编码
  cookie:{
    maxAge:1000 * 60 * 60, //必须,1000毫秒就是1秒 * 60sec * 60min
    secure:false //必须,在http中可以获取, 若true的话就只能用https
  },
  resave:true, //必须 如果在maxAge内任然操作,我们不想过期,只是闲置一个小时才想过期
  // 设置为true意味着,如果有动作,那么这个maxAge就重新计时
  // resave是重新设置session后会自动计算过期时间
  saveUninitialized:true, //一开始就给你一个cookie, false就是不给cookie,一般没什么影响,因为我们会在后端check这个cookie
  store: MongoStore.create({
    mongoUrl: "mongodb://127.0.0.1:27017/sandwich_session", //新创建了一个数据库,和之前的db不一样
    ttl: 100 * 60 * 60 // 过期时间,保持一致
  }),
}))
```
像以上这样用完后就可以自动保存到数据库然后过期后自动删除, 数据库也会更新过期时间
## Cookie-Session 致命缺点
用户太多会导致储存太多, 如果多服务器的话, session之间不互通
# GraphQL (去学!)
# React(去学!)
