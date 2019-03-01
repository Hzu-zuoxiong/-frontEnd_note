# `Weex` 加载三端(`android ios web`)本地图片

由于三端图片路径不一致，所以需要写一个方法来分别转换路径：

```js
/**
* 获取图片在三端上不同的路径
* e.g. 图片文件名是 test.jpg, 转换得到的图片地址为
* - H5      : images/test.jpg    images和所在html路径同级
* - Android : local:///test      local代表项目各dpi目录,一般放在hdpi里一张即可
* - iOS     : local///test.jpg   local代表从项目中全局扫描 test.jpg可放至项目中任* 何目录
*/
getImgPath(imgName) {
  if (weex.config.eros) {
    return `bmlocal://assets/images/${imgName}`;
  }
  const { platform } = weex.config.env;
  let imgPath;
  if (platform === 'Web') {
    imgPath = `../images/${imgName}`;
  } else if (platform === 'android') {
    // android 不需要后缀
    const tmp = imgName.substr(0, imgName.lastIndexOf('.'));
    imgPath = `local:///${tmp}`;
  } else {
    imgPath = `../../images/${imgName}`;
  }
  return imgPath;
}
```
