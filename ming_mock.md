Mokejs



> 
>
> ming_mock是在浏览器端用express风格mock后端接口小工具,为了优雅的访问mysql, ming_mock使用feach经过node用 临时存储过程 访问mysql,所以M.doSql方法中的sql可以写的非常复杂. 最新版移除了对jQuery的依赖,如果项目中有jQuery,ming_mock就用jQuery,如果没有就使用默认实现的jQuery

简化板的ming_mock是简化理解ming_mock的实现原理 地址为 <https://minglie.github.io/js/M_mock0.js>

# 0.标准版的ming_mock是ming_node的浏览器版本,大多数方法是通用的可参考ming_node中文件型数据库与web服务等章节

<https://minglie.github.io/os/ming_node/>

# 1. React中使用ming_mock

```
$ npm install ming_mock
```

```
//MIO=M.IO
import  {M, app,MIO} from "ming_mock";
 
app.get("/listAll",function (req,res) {
    console.log("params--->",req.params)
    let sql=`select ${req.params.id};`;
    M.doSql(sql,(d)=>{
        res.send(d.data[0])
    })
})
 
MIO.listAll({id:8}).then((u)=>{
    console.log("<----u",u);
})
 
export  {MIO}
```

# 2. 普通页面使用ming_mock

```
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<script src="https://minglie.github.io/js/M_mock.js"></script>
<body>
  <button id="btn1">run1</button>
  <button id="btn2">run2</button>
<script type="text/javascript">
    
        app.get("/listByPage",(req,res)=>{
            res.send(req.params)
        })
 
 
        btn1.onclick=function () {
            M.IO.listByPage({startPage:4,limit:5}).then((u)=>console.log(u))
        }
        
        btn2.onclick=function () {
            console.log(M.get("/listByPage",{startPage:1,limit:2}))
        }
</script> 
</body>
</html>
 
```

# 3. 普通页面用React使用ming_mock的CRUD

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src="https://cdn.bootcss.com/react/16.4.0/umd/react.development.js"></script> 
    <script src="https://cdn.bootcss.com/react-dom/16.4.0/umd/react-dom.development.js"></script> 
    <script src="https://cdn.bootcss.com/babel-standalone/6.26.0/babel.min.js"></script> 
    <script src="https://minglie.github.io/js/M_mock.js"></script> 
</head>
 
<script>
    app.get("/test",(req,res)=>{
        document.getElementById("requestDiv").innerHTML=JSON.stringify(req.params)
        res.send(M.result("ok "+req.params.id))
    })
</script> 
 
<body>
<div id="requestDiv" ></div>
<hr/>
<hr/>
<div id="resultDiv" ></div>
<hr/>
<hr/>
 
 
 
<div id="example" class="example"></div>
 
<script type="text/babel">
 
    class TodoItem extends React.Component {
        constructor(props){
            super(props);
        }
 
        handleDelete(id){
            this.props.delete(id)
        }
 
        render() {
            const {content}= this.props;
            const r= content.map((u,index)=>{
                    return(
                        <li key={u.id}>
                            {u.id} {u.name}   {u.age}
                            <button onClick={this.handleDelete.bind(this,u.id)}>删除</button>
                        </li>
                    )
                }
            );
            return (
                <div>{r}</div>
            )
        }
    }
 
    var M_this={};
    class TodoList extends React.Component {
        // 初始化数据
        constructor(props){
            super(props);
            this.state={
                list:[],
            };
            M_this=this;
        }
 
        componentDidMount() {
            this.setState({
                list:M.listAll()
            });
        };
        handleAdd(e) {
            M.add({
                name:this.refs.name.value,
                age:this.refs.age.value,
            });
            this.setState({
                list:M.listAll()
            });
        }
 
        handleUpdate(e) {
            M.update({
                id:this.refs.id.value,
                name:this.refs.name.value,
                age:this.refs.age.value,
            });
            this.setState({
                list:M.listAll()
            });
        }
 
        handledelete(e) {
            M.deleteById(e);
            M_this.setState({
                list:M.listAll()
            });
        }
 
 
        handleTest1(e) {
          let  r=M.get("/test?id=77");
          document.getElementById("resultDiv").innerHTML=JSON.stringify(r)
        }
 
        handleTest2(e) {
            M.IO.test({id:78}).then(r=>{
              document.getElementById("resultDiv").innerHTML=JSON.stringify(r)
            })
       
        }
 
        handleTest3(e) {
            MIO.test({id:79}).then(r=>{
              document.getElementById("resultDiv").innerHTML=JSON.stringify(r)
            })
        }
 
 
        render() {
            return (
                <div>
                    <input type="text" ref="id" id="id" placeholder="id" autoComplete="off"/>
                    <input type="text" ref="name" id="name" placeholder="name" autoComplete="off"/>
                    <input type="text" ref="age" id="age" placeholder="age" autoComplete="off"/>
                    <button onClick={this.handleAdd.bind(this)}>添加</button>
                    <button onClick={this.handleUpdate.bind(this)}>修改</button>
                    <button onClick={()=>this.handleTest1("test1")}>测试1</button>
                    <button onClick={()=>this.handleTest2("test2")}>测试2</button>
                    <button onClick={()=>this.handleTest3("test3")}>测试3</button>
                    <TodoItem content={this.state.list} delete={this.handledelete}/>
                </div>
            );
        }
    }
    ReactDOM.render(
        <div>
            <TodoList />
        </div>,
        document.getElementById('example')
    );
</script> 
 
 
 
</body>
</html>
```

# 4. ming_mock提供了基本的增,删,改,查,分页,条件查询接口可直接使用比如添加接口MIO.add({name:"zs"}),内部随机生成一个ID，可以把这些方法覆盖掉

```
        app.post("/add",(req,res)=>{
            r=M.add(req.params);
            res.send(M.result(r));
        });
 
        app.get("/delete",(req,res)=>{
            M.deleteById(req.params.id);
            res.send(M.result("ok"));
        });
 
        app.post("/update",(req,res)=>{
            M.update(req.params);
            res.send(M.result("ok"));
        });
 
        app.get("/getById",(req,res)=>{
            r=M.getById(req.params.id);
            res.send(M.result(r));
        });
 
        app.get("/listAll",(req,res)=>{
            r=M.listAll();
            res.send(M.result(r));
        });
 
        app.get("/listByParentId",(req,res)=>{
            r=M.listByProp({parentId:req.params.parentId});
            res.send(M.result(r));
        });
 
        app.get("/listByPage",(req,res)=>{
            r=M.listByPage(req.params.startPage,req.params.limit);
            res.send(M.result(r));
        })
```

> 说明

> app.get("/xxx",(req,res)=>{})或app.post("/xxx",(req,res)=>{})会在M.IO上注册xxx方法, 用M.IO.xxx({})调用xxx方法,方法的参数必须是对象,app.get的回调函数中通过req.params拿到该对象, 这完全是express风格的写法

> M.doSql是前后端传递数据的唯一方法,可通过M.host=http://localhost:8888修改后端地址 M_mock的案例如下,一次可以执行多条sql

<https://github.com/minglie/minglie.github.io/blob/master/Snippets/manager/server.js>

# 3.访问mysql,需要启动一个服务,用ming_node搭建,ming_node也是express风格,是单个文件且无依赖

```
$ npm install ming_node
$ npm install mysql
$ node index.js
```

index.js内容为

```
var M=require("ming_node");
var mysql  = require('mysql');
var app=M.server();
app.listen(8888);
var Db = mysql.createConnection(
{
    "host"     : "127.0.0.1",
    "user"     : "root",
    "password" : "123456",
    "port"     : "3306",
    "database" : "ming-lie",
    "multipleStatements": true
});
Db.doSql=function(sql){
    var promise = new Promise(function(reslove,reject){
            Db.query(sql,
                function (err, result) {
                    if(err){
                        console.error(err);
                        reject(err);
                    }
                    reslove(result);
                });
    })
    return promise;
}
prefixSql=`
DROP PROCEDURE IF EXISTS p;    
CREATE PROCEDURE p()
BEGIN
    `;
SuffixSql=`
 END;
call p
 `;
app.get("/",async function (req,res) {
    app.redirect("/index.html",req,res);
});
app.post("/doSql",async function (req,res) {
   console.log(req.params);
   try{
       var rows= await Db.doSql(prefixSql+req.params.sql+SuffixSql);
       var r=rows.slice(2);
       res.send(M.result(r));
   }catch (e){
       res.send(M.result(e,false));
   }
})
```

# 4在线sql测试 <https://mucfpga.github.io/codeEdit/index.html>

## Keywords

- [mock](https://www.npmjs.com/search?q=keywords:mock)
- [sql](https://www.npmjs.com/search?q=keywords:sql)