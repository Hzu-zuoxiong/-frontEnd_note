## 冒泡排序

## 原理：

依次比较相邻的两个值，如果后面的比前面的小，则将小的元素排到前面。依照这个规则进行多次并且递减的迭代。

## 时间复杂度、空间复杂度、稳定性：

- 平均时间复杂度：O（N\*N）
- 最好情况：O（N）
- 最差情况：O（N\*N）
- 空间复杂度：O（1）
- 稳定性：稳定

## 代码：

```js
var arr = [5, 4, 3, 7, 9, 4, 2, 1, 6];
function sortArr(arr) {
  for (var i = 0; i < arr.length - 1; i++) {
    for (var j = 0; j < arr.length - 1 - i; j++) {
      if (arr[j] > arr[j + 1]) {
        var temp = arr[j];
        arr[j] = arr[j + 1];
        arr[j + 1] = temp;
      }
    }
  }
}
sortArr(arr);
console.log(arr); // [1, 2, 3, 4, 4, 5, 6, 7, 9]
```
