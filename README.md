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

@import './base/_base_function.scss';
@import './base/_base_constants.scss';
@import './base/_base_mixin.scss';

/*-------------------------------   Button   -------------------------------*/

@include comp(eu-button){
  $_space:getSpace($height-button,0,$line-height-normal);

  @include main{
      @include shape(null $width-button,$height-button,$radius);
      @include space(padding,$_space $padding $_space $padding);
      @include space(margin,(-$_space) null (-$_space) null);
      @include font($font-size-normal,$line-height-normal,$color-white);
      @include transition(box-shadow);
      @include prefixer(user-select,none,$prefix);
  }

  @include state-hover{
      background-color: $color-primary-light;
  }

  @include state-active{
      padding-bottom: 2px;
      background-color: $color-primary;
      box-shadow: 1px 1px 0px $color-primary-darker inset;
  }

  &::before{
      @include transition(color);
  }

  span{
      display: inline;
  }

  .eu-icon:not(:only-child){
      margin-right: .5em;
  }
}
```

* css
```css
.eu-button {
  min-width: 9.5rem;
  height: 2.16667rem;
  border-radius: 0.33333rem;
  padding-top: 0.08333rem;
  padding-right: 1rem;
  padding-bottom: 0.08333rem;
  padding-left: 1rem;
  margin-top: -0.08333rem;
  margin-bottom: -0.08333rem;
  font-size: 1.16667rem;
  line-height: 2rem;
  color: #FFFEFD;
  -webkit-transition-property: box-shadow;
  -moz-transition-property: box-shadow;
  transition-property: box-shadow;
  -webkit-transition-duration: 0.1s;
  -moz-transition-duration: 0.1s;
  transition-duration: 0.1s;
  -webkit-transition-timing-function: ease;
  -moz-transition-timing-function: ease;
  transition-timing-function: ease;
  -webkit-user-select: none;
  -moz-user-select: none;
  user-select: none;
}

.eu-button:hover {
  background-color: #8bcf70;
}

.eu-button.active, .eu-button:focus, .eu-button:active {
  padding-bottom: 2px;
  background-color: #6CC24A;
  box-shadow: 1px 1px 0px #55a237 inset;
}

.eu-button::before {
  -webkit-transition-property: color;
  -moz-transition-property: color;
  transition-property: color;
  -webkit-transition-duration: 0.1s;
  -moz-transition-duration: 0.1s;
  transition-duration: 0.1s;
  -webkit-transition-timing-function: ease;
  -moz-transition-timing-function: ease;
  transition-timing-function: ease;
}

.eu-button span {
  display: inline;
}

.eu-button .eu-icon:not(:only-child) {
  margin-right: .5em;
}

```
