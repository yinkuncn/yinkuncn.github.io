node.js

```js
let http = require('http');
let fs = require('fs');
let mime = require('mime');
let url = require('url');
http.createServer(function (req, res) {
    // 用parse()方法解析请求URL，得到对象，并取其中pathname与query两项赋值给同名变量以使用，其中pathname为字符串，query为数组
    let {
        pathname,
        query
    } = url.parse(req.url, true);
    // 正则排除带后缀的文件类请求
    let reg = /\.(\w+)$/;
    if (reg.test(pathname)) {
        res.setHeader("Content-Type", mime.getType(pathname));
        fs.readFile('.' + pathname, function (err, data) {
            res.end(data);
        })
        // return跳出 createServer 避免执行后续代码
        return;
    }
    // 建立数据“中转仓库”
    let obj = {
        code: 0,
        msg: "请求成功"
    }
    // 数据读取路径
    let path = "./json/custom.json";
    // 接收路径前缀：getList
    if (pathname === '/getList') {
        fs.readFile(path, 'utf-8', function (err, data) {
            // 把请求的数据存入“中转仓库”data属性下
            obj.data = data;
            // 序列化数据后传出（标准处理）
            res.end(JSON.stringify(obj.data));
        })
    };
    // 接收路径前缀：removeInfo
    if (pathname === '/removeInfo') {
        fs.readFile(path, 'utf-8', function (err,data) {
            // readFile读取出data文件里的json数组，解析为数组
            let arr =JSON.parse(data);
            arr=arr.filter((item) => {
                // 通过得到的ID进行比较，与查询ID不同，则保留
                return item.id!=query['id'];
            })
            // 鉴于数据文件custom.json是JSON格式，将数据先序列化
            arr=JSON.stringify(arr);
            // 写入文件
            fs.writeFile(path, arr, 'utf-8', function () {
                obj.msg = "删除成功";
                // 序列化中转仓库并传出
                res.end(JSON.stringify(obj));
            });
        })
    }
}).listen(60001, function () {
    console.log("success");
})
```

