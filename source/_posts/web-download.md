今天有个急需求要当天上线，需要从web下载一个xml,这个xml基于一个固定模板再填充一定参数。后端小伙看了下说这个用java写比较麻烦。考虑到这需求很急，好吧咱大前端来搞定。

```
export function downloadXML(data){
  let fileName = data[0]+'-'+data[4]+'.xml'
  let xmlStr = template;
  data.map(value=>xmlStr = xmlStr.replace('?',value));
  xmlStr = iconvLite.encode(xmlStr,'gbk');
  //var blob = new Blob([xmlStr], {type: 'octet/stream'});
  var blob = new Blob([xmlStr], {type: 'text/xml;charset=GBK2312', encoding:'GBK2312'});
  ///var csvURL = window.URL.createObjectURL(blob);
  //window.open(csvURL);
  saveData(blob,fileName);
}

let saveData = (function () {
  var a = document.createElement("a");
    document.body.appendChild(a);
    //a.style = "display: none";
    return function (blob, fileName) {
        let url = window.URL.createObjectURL(blob);
        a.href = url;
        a.download = fileName;
        a.click();
        //window.URL.revokeObjectURL(url);
    };
}());
```

1. 需要弹出一个文件保存框并指定文件名
2. 目标客户需要这个xml为gbk编码