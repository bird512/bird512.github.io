title:  Web前端直接生成文件下载 
comment: true
tags: [技术, javascript, html5]
date: 2016-9-14 10:40:50
----
今天有个急需求要当天上线，需要从web下载一个xml,这个xml基于一个固定模板再填充一定参数。后端小伙看了下觉的这个用java写比较麻烦。考虑到这需求很急，好吧咱大前端来搞定。
----
这个基本思路是用html5的Bolb来生成文件流供下载。

```
import iconvLite from 'iconv-lite';
const template ='xxxxx'  //template是一个预定义的xml String. 

export function downloadXML(data){
  let fileName = data[0]+'-'+data[4]+'.xml'
  let xmlStr = template;	
  data.map(value=>xmlStr = xmlStr.replace('?',value));
  xmlStr = iconvLite.encode(xmlStr,'gbk');
  var blob = new Blob([xmlStr], {type: 'text/xml;charset=GBK2312', encoding:'GBK2312'})；
  saveData(blob,fileName);
}

let saveData = (function () {
  var a = document.createElement("a");
    document.body.appendChild(a);
    return function (blob, fileName) {
        let url = window.URL.createObjectURL(blob);
        a.href = url;
        a.download = fileName;
        a.click();
    };
}());
```

### 遇到问题及解决方法：
1. 需要弹出一个文件保存框并指定文件名。方法saveDate()就是为了处理这个问题。通过建一个link来实现文件弹出框并设定文件名。 闭包方法是为了确保这个link只被创建一次。

2. 目标客户需要这个xml为gbk编码。正常情况下javascript运行时的encoding是依赖于浏览器的编码设，一般都为utf. 所以当一个string放到Bolb里后它的encoding是utf-8。解决方法：在github上找到了一个encoding转换的lib叫iconv-lite。 在string放入Blob前先用iconv-lite转码成gbk的buffer。 

### 好处：
前端直接生成xml供下载，避免了一次http请求。对用户来说体验更发反应速度更快，对系统来说降低了后台负荷及复杂性。

# 追加：
第二天客户追加一新需求，要求一次下载多个文件并打包成一个zip下载。 

先install一个JSZip包:
```
npm install jszip
```

代码:
```
import JSZip from 'jszip';
var zip = new JSZip();
zip.file("Hello.txt", "Hello World\n");
zip.file("Hello2.txt", "Hello World2\n");

zip.generateAsync({type:"blob"})
.then(function(content) {
    saveData(content, "example.zip");
});
```

