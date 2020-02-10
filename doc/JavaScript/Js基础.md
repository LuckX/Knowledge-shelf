# 1.数据类型
##  1.1.Number 
不区分整数和浮点数
```
123; // 整数123
0.456; // 浮点数0.456
1.2345e3; // 科学计数法表示1.2345x1000，等同于1234.5
-99; // 负数
NaN; // NaN表示Not a Number，当无法计算结果时用NaN表示
Infinity; // Infinity表示无限大，当数值超过了JavaScript的Number所能表示的最大值时，就表示为Infinity
```
## null和undefined
null表示一个“空”的值,undefined表示值未定义。
## 1.2.字符串
以单引号'或双引号"括起来的任意文本
字符串是不可变的
```
s.length 求字符串长度
s.toUpperCase() 把一个字符串全部变为大写
s.toLowerCase() 把一个字符串全部变为小写
s.indexOf() 会搜索指定字符串出现的位置
s.substring() 返回指定区间的字符串
s[] 返回指定位置字符串
```
## 1.3.布尔值
布尔值只有true、false两种值
## 1.4.数组
JavaScript的Array可以包含任意数据类型，并通过索引来访问每个元素。
**请注意：1.如果通过索引赋值时，索引超过了范围，同样会引起Array大小的变化；2.直接给数组的length赋值也会改变数组的大小**
```
arr.indexOf() 索一个指定的元素的位置
arr.slice(, ) slice()就是对应String的substring()版本，它截取Array的部分元素，然后返回一个新的Array;
arr.push() 向Array的末尾添加若干元素
arr.pop() 则把Array的最后一个元素删除掉：
arr.join() 把当前Array的每个元素都用指定的字符串连接起来，然后返回连接后的字符串
arr.splice() 从指定的索引开始删除若干元素，然后再从该位置添加若干元素：
arr.reverse() 反转字符串
arr.sort() 对当前Array进行排序
```
## 1.5.Map
Map是一组键值对的结构，可以使用set方法添加元素，get方法获取元素，delete删除元素
```
var mymap = new Map([['Michael', 95], ['Bob', 75], ['Tracy', 85]]);
mymap.get('Michael');
mymap.set('Adam', 67);
mymap.delete('Adam');
```
## 1.6.Set
一组key的集合，但不存储value。由于key不能重复，所以，在Set中，没有重复的key。
```
var s = new Set([1, 2, 3]);
s.add(4);
s.delete(3);
```
## 1.7.iterable
为了遍历Map和Set，引入了iterable类型，Array、Map和Set都属于iterable类型，具有iterable类型的集合可以通过新的for ... of循环来遍历；
```
var s = new Set(['A', 'B', 'C']);
var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);

for (var x of s) { // 遍历Set
    console.log(x);
}
for (var x of m) { // 遍历Map
    console.log(x[0] + '=' + x[1]);
}
```
可以直接使用iterable内置的forEach方法进行迭代，它接收一个函数，每次迭代就自动回调该函数。
```
var s = new Set(['A', 'B', 'C']);
s.forEach(function (element, sameElement, set) {
    console.log(element);
})

var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
m.forEach(function (value, key, map) {
    console.log(value);
});
```
## 1.8.变量
变量使用var进行定义，可以把任意数据类型赋值给变量;
```
var a = 155
a = 'adc'
```
# 2.运算符
运算符基本上同其他语言，需要特别注意的是==和===运算
== 它会自动转换数据类型再比较，很多时候，会得到非常诡异的结果
=== 不会自动转换数据类型，如果数据类型不一致，返回false
!= 不等于
!== 不绝对等于（值和类型有一个不相等，或两个都不相等）
# 3.控制流
## 3.1.条件判断
同java
使用使用if () { ... } else { ... }来进行条件判断;
```
if (条件)
{
    ...
}
else //可省略
{
    ...
}
```
使用多个if...else...的组合进行条件判断
```
if (条件)
{
    ...
}
else if(条件)//可省略
{
    ...
}
else if(条件)//可省略
{
    ...
}
...
else
{
    ...
}
```
## 3.2.循环
使用for循环，for in循环、while循环实现遍历（同java）
break 语句可用于跳出循环；
continue 语句中断循环中的迭代；
# 4.函数
# 5.面向对象
test
