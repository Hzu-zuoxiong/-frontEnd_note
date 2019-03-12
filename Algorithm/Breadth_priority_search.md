# 广度优先搜索

## 定义

> 广度优先算法（`Breadth-First Search`），同广度优先搜索，又称作宽度优先搜索，或横向优先搜索，简称 BFS，是一种图形搜索演算法。简单的说，BFS 是从根节点开始，沿着树的宽度遍历树的节点，如果发现目标，则演算终止。 ——《百度百科》

## 思想

`BFS` 是一种盲目搜寻法，目的是系统地展开并检查图中的所有节点，以找寻结果。换句话说，它并不考虑结果的可能位置，彻底地搜索整张图，直到找到结果为止。`BFS` 并不使用经验法则算法。从算法的观点，所有因为展开节点而得到的子节点都会被加进一个先进先出的队列中。

## 实现

通过一个例子对广度优先搜索进行举例说明：遍历打印出一个对象中所有的 `name` 属性。

```js
// 打印 root 对象中的所有 name 属性
let root = {
  name: 'root',
  children: [
    {
      name: '1-1',
      children: [
        {
          name: '2-1',
          children: [
            {
              name: '3-1'
            },
            {
              name: '3-2'
            }
          ]
        },
        {
          name: '2-2',
          children: [
            {
              name: '3-3'
            }
          ]
        }
      ]
    },
    {
      name: '1-2',
      children: []
    }
  ]
};

function bfs(root) {
  let res = [];
  let list = [];
  list.push(root);
  while (list.length) {
    let len = list.length;
    for (let i = 0; i < len; i++) {
      let cur = list[0];
      res.push(list[0].name);
      if (cur.children && cur.children.length) {
        cur.children.map(item => list.push(item));
      }
      list.shift();
    }
  }
  console.log(res); // ["root", "1-1", "1-2", "2-1", "2-2", "3-1", "3-2", "3-3"]
}

bfs(root);
```
