# 插入排序

## 原理：

从第一个元素开始，认定该元素已被排序。取出下一个元素，在已经排序的元素序列中从后向前扫描，如果该元素（已排序）大于新元素，则将该元素移到下一位置。重复进行前一步操作，直到找到已排序的元素小于或等于新元素的位置。

## 代码示例：

```js
function sortArr(arr) {
	for(var i = 1; i < arr.length; i++) {
		var key = arr[i];
		var j = i - 1;
		while(j >= 0 && arr[j] > key) {
			arr[j + 1] = arr[j];
			j--;
		}
		arr[j + 1] = key;
	}
	return arr;
}
```

## 优化：

通过二分查找可以加速排序。

**二分查找：**也称为折半查找。首先找到一个中间值，通过不断与中间值比较，直到找到所在位置为止。

## 代码示例：

```js
function binarySearch(arr, dest) {
	var right = arr.length - 1;
	var left = 0;
	while(left <= right) {
		var mid = Math.floor((right + left) / 2);
		if(arr[mid] === dest) {
			return mid;
		} else if(dest > arr[mid]) {
			left = mid + 1;
		} else {
			right = mid - 1;
		}
	}
	return false;
}
```

## 二分查找的插入排序：

```js
function sortArr(arr) {
	for(var i = 1; i < arr.length; i++) {
		var key = arr[i];
		var left = 0;
		var right = i - 1;
		// 利用二分查找找到相应位置
		while(left <= right) {
			var mid = Math.floor((left + right) / 2);
			if(key < arr[mid]) {
				right = mid - 1;
			} else {
				left = mid + 1;
			}
		}
		// 相应位置到已排序末尾这一段都要向后移
		for(var j = i - 1; j >= left; j--) {
			arr[j + 1] = arr[j];
		}
		arr[left] = key;
	}
	return arr;
}
```



