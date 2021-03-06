# 正则表达式

## 元字符

| 元字符 |                                 作用                                 |
| :----: | :------------------------------------------------------------------: |
|   .    |                    匹配任意字符除了换行符和回车符                    |
|   []   |      匹配方括号内的任意字符。比如 [0-9] 就可以用来匹配任意数字       |
|   ^    | ^9，这样使用代表匹配以 9 开头。[^9]，代表不匹配方括号内除了 9 的字符 |
| {1, 2} |                          匹配 1 到 2 位字符                          |
| (yck)  |                         匹配 \| 前后任意字符                         |
|   \|   |                       只匹配和 yck 相同字符串                        |
|   \    |                                 转义                                 |
|   \*   |                  只匹配出现 0 次及以上 \* 前的字符                   |
|   +    |                   只匹配出现 1 次及以上 + 前的字符                   |
|   ?    |                            ? 之前字符可选                            |

## 修饰语

| 修饰语 |    作用    |
| :----: | :--------: |
|   i    | 忽略大小写 |
|   g    |  全局搜索  |
|   m    |    多行    |

## 字符简写

| 简写 |         作用         |
| :--: | :------------------: |
|  \w  | 匹配字母数字或下划线 |
|  \W  |      和上面相反      |
|  \s  |   匹配任意的空白符   |
|  \S  |      和上面相反      |
|  \d  |       匹配数字       |
|  \D  |      和上面相反      |
|  \b  | 匹配单词的开始或结束 |
|  \B  |      和上面相反      |

## 常用正则表达式

1. 匹配中文: `[\u4e00-\u9fa5]`
2. 英文字母：`[a-zA-Z]`
3. 匹配数字：`[0-9]`
4. 匹配中文、英文字母、数字及下划线：`^[\u4e00-\u9fa5_a-zA-Z0-9]+$`
5. 由数字、26个英文字母或者下划线组成的字符串：`^\w+$`
6. 最长不得超过7个汉字或14个字节（数字、字母和下划线）：`^[\u4e00-\u9fa5]{1,7}$|^[\dA-Za-z_]{1,14}$`
7. 匹配首尾空白字符：`[^s*|s*$]`
8. 匹配 `Email` 地址：`^[a-zA-Z0-9][\w\.-]*[a-zA-Z0-9]@[a-zA-Z0-9][\w\.-]*[a-zA-Z0-9]\.[a-zA-Z][a-zA-Z\.]*[a-zA-Z]$`
9. 匹配手机号：`^((13[0-9])|(14[0-9])|(15[0-9])|(17[0-9])|(18[0-9]))\d{8}$`
10. 身份证：`(^\d{15}$)|(^\d{17}([0-9]|X|x)$)`
11. 匹配腾讯QQ号：`[1-9][0-9]{4,}` （腾讯QQ号从10000开始）
12. 匹配IP地址：`d+.d+.d+.d+`


