layout: post
title: atool-build协同html-webpack-plugin解决bundle文件cache问题
comment: true
----



# 问题：

atool-build默认编译出index.js, index.css， 当项目发布新版本，但用户游览器加载的经常还是上个版本的缓存，用户不得不手动去清除缓存。



# 方案：

引入webpack的插件html-webpack-plugin，应用html模版。

1. install html-webpack-plugin. 

```
install html-webpack-plugin --save-dev
```

2. 更改package.json

   npm run clear: 清除之前版本的js/css文件。

   —hash atool-build的选项，设置了之后webpack将会编译出带hash的js/css文件。

```
"deploy": "npm run clear && atool-build  --hash ",
```

3. 更改webpack.config.js,指定该index.ejs为html模版文件

```
  webpackConfig.plugins.push(
    new HtmlWebpackPlugin({
      template: "./src/entries/index.ejs"
    })
  );
```

4. index.ejs. 注：在调试过程中发现必须后缀名为.ejs.不然模版不起做用，因时间关系没有深究。

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>我的title</title>
  </head>
  <body>
  <div id="root"></div>
  </body>
</html>
```

5. 编译后生成的index.html

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <meta charset="UTF-8">
    <meta name="renderer" content="webkit">
    <title>我的title</title>
  <link href="index-cfdc7c9610845019c2f2.css" rel="stylesheet"></head>
  <body>
  <div id="root"></div>
  <script type="text/javascript" src="common-a435e8ead877d7765d7e.js"></script>
  <script type="text/javascript" src="index-cfdc7c9610845019c2f2.js"></script>
  </body>
</html>
```



