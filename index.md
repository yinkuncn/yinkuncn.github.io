localStorage

```
// 存储
localStorage.setItem("lastname", "Smith");
// 检索
document.getElementById("result").innerHTML = localStorage.getItem("lastname");
// 删除
localStorage.removeItem("key");
// 清空
localStorage.clear();
```

localStorage 是对象，但有length属性，可以用for循环遍历键值对

```
var storage=window.localStorage;
storage.a=1;
storage.setItem("c",3);
for(var i=0;i<storage.length;i++){
    var key=storage.key(i);
    console.log(key);
}
```

使用 key() 方法，向其中出入索引即可获取对应的键。

一般我们会将 JSON 存入 localStorage 中，但是在 localStorage 会自动将 localStorage 转换成为字符串形式。

这个时候我们可以使用 JSON.stringify() 这个方法，来将 JSON 转换成为 JSON 字符串。

实例：

```
if(!window.localStorage){
    alert("浏览器不支持localstorage");
}else{
    var storage=window.localStorage;
    var data={
        name:'xiecanyong',
        sex:'man',
        hobby:'program'
    };
    var d=JSON.stringify(data);
    storage.setItem("data",d);
    console.log(storage.data);
}
```

读取之后要将 JSON 字符串转换成为 JSON 对象，使用 JSON.parse() 方法：

```
var storage=window.localStorage;
var data={
    name:'xiecanyong',
    sex:'man',
    hobby:'program'
};
var d=JSON.stringify(data);
storage.setItem("data",d);
//将JSON字符串转换成为JSON对象输出
var json=storage.getItem("data");
var jsonObj=JSON.parse(json);
console.log(typeof jsonObj);
```

![img](https://www.runoob.com/wp-content/uploads/2018/09/728493-20160626124919578-638115637.png)

打印出来是 Object 对象。

另外还有一点要注意的是，其他类型读取出来也要进行转换。

### LocalStorage 超出大小

When quota exceeds on [LocalStorage or SessionStorage](http://www.w3.org/TR/webstorage/), catch exception using `try catch`. To determine if the error is caused by over quota, check if the `name` property of `DOMException` is either "`QuotaExceededError`" or "`NS_ERROR_DOM_QUOTA_REACHED`" (on Firefox).

```
try {
  localStorage.setItem(data.name, JSON.stringify(data));
} catch(domException) {
  if (domException.name === 'QuotaExceededError' ||
      domException.name === 'NS_ERROR_DOM_QUOTA_REACHED') {
    // Fallback code comes here.
  }
}
```