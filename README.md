# Icon.svg 生成 webfonts 及项目引入

> 雪碧图一直是前端对于icon处理的普遍方式，对于icon的存储以及使用方式一直随着技术的更新而变化，本文将介绍相对流行的 `webfont` 技术的原理及使用

## 原理

css中 `@font-face` 允许网页开发者为其网页指定在线字体。通过这种作者自备字体的方式，可以消除对用户电脑字体的依赖。

### 引用方式

```css
.webfont::before {
  /* 在引用svg中定义的unicode */
  content: "\ea01";
}
.webfont {
  font-family: "webfont";
  font-style: normal;
}
@font-face {
  font-family: "webfont";
  src: url("./webfont.eot");
  src: url("./webfont.eot?#iefix") format("embedded-opentype"), url("./webfont.woff2") format("woff2"), url("./webfont.woff") format("woff"), url("./webfont.ttf") format("truetype"), url("./webfont.svg#webfont") format("svg");
}
```
```html
  <div>
    <i class="webfont"></i>
    <span>webfont-avatar</span>
  </div>
```

效果展示
![](https://i.loli.net/2020/02/12/6TQ9yAvaphr384Z.png)

### 支持度

可以看到目前 ***web fonts*** 的支持度还是很高的

![](https://i.loli.net/2020/02/12/vkTAnxzE9i1byQ3.png)


## 我现在只有svg如何转换成字体引入项目呢？

1、 引入npm插件 [icon-font-generator](https://www.npmjs.com/package/icon-font-generator)

```javascript
npm install --save-dev icon-font-generator
```

2、 引入 `package.json` script, npm run font 生成打包字体

```javascript
"scripts": {
  // 将 src/assets 里的svg打包成所有兼容字体类型到fonts目录下
  "font": "icon-font-generator src/assets/*.svg -o fonts"
},
```

3、 将生成的css引入到项目中

```css
@font-face {
    font-family: "icons";
    src: url("./icons.eot?5abc260afe1fb00af7f4a3ad54677bcb?#iefix") format("embedded-opentype"),
    url("./icons.woff2?5abc260afe1fb00af7f4a3ad54677bcb") format("woff2"),
    url("./icons.woff?5abc260afe1fb00af7f4a3ad54677bcb") format("woff"),
    url("./icons.ttf?5abc260afe1fb00af7f4a3ad54677bcb") format("truetype"),
    url("./icons.svg?5abc260afe1fb00af7f4a3ad54677bcb#icons") format("svg");
}

i[class^="icon-"]:before, i[class*=" icon-"]:before {
    font-family: icons !important;
    font-style: normal;
    font-weight: normal !important;
    font-variant: normal;
    text-transform: none;
    line-height: 1;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
}

.icon-avatar:before {
    content: "\f101";
}
.icon-envelope:before {
    content: "\f102";
}
.icon-phone-call:before {
    content: "\f103";
}
```

4、 同时一起打包的还有字体的html可以用来查看字体对应效果

![](https://i.loli.net/2020/02/16/kMCR2lUtzDYqSLs.png)



## 为什么src写那么多个format？

不同浏览器对不同字体的支持度不一样，浏览器按照从前往后的顺序依次识别是否支持当前字体，然后加载

