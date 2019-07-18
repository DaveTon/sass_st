* [编程书籍索引](https://github.com/DaveTon/sass/blob/master/docs/free_prog.md)

## sass 应用

* [example](https://github.com/DaveTon/sass/blob/master/docs/example_st.md)

## vscode 配置 sass 编译环境

* 安装[node](https://nodejs.org/zh-cn/)
* 安装ruby

```
npm install ruby
```

* 安装sass

```
npm install -g sass
```

* 安装[VScode](https://code.visualstudio.com/)
* 安装easysass插件
* 配置编译环境
```js
{
    /** Easy Sass 插件 **/
    "easysass.compileAfterSave": true,  // 保存后是否自动编译
    "easysass.excludeRegex": "_",  // 配置忽略编译的文件格式 eg：_variable.scss
    "easysass.formats": [
        {
            "format": "expanded",  // 未压缩的css文件
            "extension": ".css"
        },
        {
            "format": "compact",  // 简洁格式的 css 代码(代码风格)
            "extension": ".min.css"
        }
    ],
    "easysass.targetDir": "resource/css"  // 自定义css输出文件路径（相对于根目录）
}
```

## Template

* scss
```scss
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}

@mixin border-radius($radius) {
  -webkit-border-radius: $radius;
     -moz-border-radius: $radius;
      -ms-border-radius: $radius;
          border-radius: $radius;
}

nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  li { @include border-radius(10px); }

  a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
  }
}
```

* css
```css
body {
  font: 100% Helvetica, sans-serif;
  color: #333;
}

nav ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

nav li {
  -webkit-border-radius: 10px;
  -moz-border-radius: 10px;
  -ms-border-radius: 10px;
  border-radius: 10px;
}

nav a {
  display: block;
  padding: 6px 12px;
  text-decoration: none;
}

```
