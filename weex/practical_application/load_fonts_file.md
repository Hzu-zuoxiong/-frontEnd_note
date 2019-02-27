# 离线加载本体字体文件

## 前提知识

1. `Android` 或者 `iOS` 访问本地图片或者字体,在 `weex` 中统一以 `local://` 为前缀；
2. `'/'` 在 `android` 下如果加载的是字体对应的就是 `assets` 目录,若果加载的图片就从 `drawable` 目录, 所以 `iconfont.ttf` 放置在 `src/assets` 目录下的话,字体的 `url` 加载方式应该为(`local:///iconfont.ttf`)；
3. `iOS` 同理,不过资源文件在 `bundle resources` 下.(加载方法未测试)

`weex` 官方参考链接：

- [dom 加载自定义字体](https://weex.apache.org/zh/docs/modules/dom.html#addrule)
- [本地资源与远程资源](https://weex.apache.org/zh/guide/advanced/asset-path.html#schemes)
- [相对路径](https://weex.apache.org/zh/guide/advanced/asset-path.html#%E7%9B%B8%E5%AF%B9%E8%B7%AF%E5%BE%84)

## 实现代码

```js
loadCustomFont() {
  const domModule = weex.requireModule('dom');
  const platform = weex.config.env.platform.toLowerCase();
  let url;
  if (weex.config.eros) {
    // android 不需要后缀
    url = 'bmlocal://iconfont/iconfont.ttf';
  } else if (platform === 'android') {
    // 本地资源采用'local:// '的方式加载
    // 第三个斜杠是代表当前目录,对于android来说,如果加载的是字体,那么就是assets目录
    // 同理`/iconfont.ttf'`就是加载`assets`目录下的iconfont.ttf文件
    // 注意我这里是将字体文件放在'assets/dist/''目录下的,所以
    url = 'local:///dist/iconfont.ttf';
  } else if (platform === 'ios') {
    url = 'local:///iconfont/iconfont.ttf';
  } else {
    url = '//at.alicdn.com/t/xxxxxxxx.ttf';
    // url = '/src/assets/iconfont/iconfont.ttf';
  }
  domModule.addRule('fontFace', {
    fontFamily: 'iconfont',
    src: `url('${url}')`,
  });
}
```

## 动态绑定本地字体时显示异常

加载本地的字体库的话，静态写死在 `<text>` 元素下，如 `<text class='iconfont'>&#xe600;<text>`，这样正常，但是如果通过 `Mustache` 进行数据绑定 `{{font}}`（这里的`font='&#xe600;'`）时，页面中显示是错误的。

通过正则表达式将 `iconfont` 的字符取出替换,用 `String.fromCharCode()` 方法处理。

```js
decodeFontUnicode(text) {
  const regExp = /&#x[a-z|0-9]{4,5};?/g;
  let result;
  if (regExp.test(text)) {
    result = text.replace(new RegExp(regExp), (iconText) => {
      const replace = iconText.replace(/&#x/, '0x').replace(/;$/, '');
      return String.fromCharCode(replace);
    });
  }
  return result;
}
```
