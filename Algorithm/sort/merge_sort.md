# 归并排序

> 和选择排序一样，归并排序的性能不受输入数据的影响，但表现比选择排序好的多，因为始终都是 `O(nlogn)`的时间复杂度。代价是需要额外的内存空间。

## 具体算法

1. 把长度为 `n` 的输入序列分成两个长度为 `n/2` 的子序列；
2. 对这两个子序列分别采用归并排序；
3. 将两个排序好的子序列合并成一个最终的排序序列。

## 算法分析

- 最佳情况：`T(n) = O(n)`
- 最差情况：`T(n) = O(nlogn)`
- 平均情况：`T(n) = O(nlogn)`

### 代码实现

```js
function mergeSort(arr) {
  var len = arr.length;
  if (len < 2) {
    return arr;
  }
  var middle = Math.floor(len / 2),
    left = arr.slice(0, middle),
    right = arr.slice(middle);
  return merge(mergeSort(left), mergeSort(right));
}

function merge(left, right) {
  var result = [];
  while (left.length && right.length) {
    if (left[0] <= right[0]) {
      result.push(left.shift());
    } else {
      result.push(right.shift());
    }
  }
  while (left.length) result.push(left.shift());
  while (right.length) result.push(right.shift());
  return result;
}
var arr = [3, 44, 38, 5, 47, 15, 36, 26, 27, 2, 46, 4, 19, 50, 48];
console.log(mergeSort(arr)); // [2, 3, 4, 5, 15, 19, 26, 27, 36, 38, 44, 46, 47, 48, 50]
```

## 动图演示

![](/assets/Algorithm/sort/merge_sort)

> 参考连接：[十大经典排序算法总结](https://juejin.im/post/57dcd394a22b9d00610c5ec8#heading-34)
