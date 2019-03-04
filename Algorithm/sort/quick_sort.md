# 快速排序

## 时间复杂度：

* 快速排序的平均时间复杂度为O\(NlogN\)
* 快速排序的最差时间复杂度为O\(N²\)

## 实例：

假设现在对“`6 1 2 7 9 3 4 5 10 8`”这10个数进行排序。首先在这个序列中随便找一个数作为**基准数**为了方便，就让第一个数6作为基准数吧。接下来，需要将这个序列中所有比基准数大的数放在6的右边，比基准数小的数放在6的左边，类似下面这种排列：`3 1 2 5 4 `**`6`**` 9 7 10 8`

## 算法解释：

分别从初始序列“`6  1  2 7  9  3  4  5 10  8`”两端开始“探测”，用两个变量i和j，分别指向序列最左边和最右边。先从右往左找一个小于6的数，再从左往右找一个大于6的数，然后交换他们。刚开始的时候让 i 指向序列的最左边（即i=0），指向数字6。让 j 指向序列的最右边（j=9），指向数字8。

![](/assets/快排1.png)

首先 j 开始出动。因为设置的基准数是最左边的数，所以需要让 j 先出动，这一点非常重要（可以保证最后一个数跟基数对换的时候总比基数小）。j 向左挪动（即j--），直到找到一个小于6的数停下来。接下来 i 向右挪动（即i++），直到找到一个数大于6的数停下来。最后 j 停在了数字5面前，i 停在了数字7面前。交换 i 和 j 所指向的元素的值。交换之后的序列如下：`6 1 2 `**`5`**` 9 3 4 `**`7`**` 10 8`

![](/assets/快排2.png)

第一次交换结束。接下来 j 继续向左挪动。他停在4。i 也继续向右挪动的，他停在9。此时再次进行交换，交换之后的序列如下：`6 1 2 5 `**`4`**` 3 `**`9`**` 7 10 8`

![](/assets/快排3.png)

第二次交换结束。j 继续向左挪动，他停在3。i 继续向右移动，此时 i 和 j 相遇了，i 和 j 都走到3面前。说明此时“探测”结束。我们将基准数6和3进行交换。交换之后的序列如下：**`3`**` 1 2 5 4 `**`6`**` 9 7 10 8`

![](/assets/快排4.png)

第一轮“探测”真正结束。此时以基准数6为分界点，6左边的数都小于等于6，6右边的数都大于等于6。

接下来，对左边和右边的序列分别按上述步骤进行调整即可。

![](/assets/快排5.png)

## JS代码：

```js
var arr = [6,1,2,7,-5,102,9,6,3,4,5,10,8];
var quickSort = function(arr,left, right) {
	if(left > right) {
		return;
	}
	var pivot = arr[left];
	var i = left;
	var j = right;
	while(i != j) {
		// 从右边开始找小于基数
		while(arr[j] >= pivot && i < j){
			j--;
		}
		// 从左边开始找大于基数
		while(arr[i] <= pivot && i < j) {
			i++;
		}
		// 两数进行交换
		if(i < j) {
			var temp = arr[i];
			arr[i] = arr[j];
			arr[j] = temp;
		}
	}
	// 与基数进行交换
	arr[left] = arr[i];
	arr[i] = pivot;

	quickSort(arr, left, i-1);
	quickSort(arr, i + 1, right);
}
console.log(arr);
quickSort(arr, 0, 12);
console.log(arr);
```

参考阮一峰老师的代码：

```js

var quickSort = function(arr) {
    if (arr.length <= 1) { return arr; }
    // var pivotIndex = Math.floor(arr.length / 2);   //基准位置（理论上可任意选取）
    // var pivot = arr.splice(pivotIndex, 1)[0];  //基准数
    var pivot = arr[0];  // 第一个数为基准数
    var left = [];
    var right = [];
    for (var i = 0; i < arr.length; i++){
        if (arr[i] < pivot) {
            left.push(arr[i]);
        } else {
            right.push(arr[i]);
        }
    }
    return quickSort(left).concat([pivot], quickSort(right));  //链接左数组、基准数构成的数组、右数组
};
```

参考链接：[http://developer.51cto.com/art/201403/430986.htm](http://developer.51cto.com/art/201403/430986.htm)

