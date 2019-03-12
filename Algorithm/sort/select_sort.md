# 选择排序

## 原理：

在未排序序列中找到最小（最大）元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（最大）元素，然后放到已排序序列的末尾。以此类推，直到所有排序均排序完毕。

## 代码实例：

```js
function sortArr(arr) {
  for (var i = 0; i < arr.length - 1; i++) {
    var minIndex = i;
    for (var j = i + 1; j < arr.length; j++) {
      if (arr[j] < arr[minIndex]) {
        minIndex = j;
      }
    }
    var temp = arr[i];
    arr[i] = arr[minIndex];
    arr[minIndex] = temp;
  }
  return arr;
}
```
