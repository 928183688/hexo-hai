---
title: 算法学习笔记
cover: https://upload.dianshangdanao.com/201912281848182428.jpg
---
## 数组和链表
数组在内存中是相连的。
链表中的元素可储存在内存的任何地方，每个元素都存储了下一个元素的地址，将一系列随机的内存串在一起。
	    数组	链表
读取	O(1)	O(n)
插入	O(n)	O(1)
删除	O(n)	O(1)
链表插入元素很简单，只需修改它前面的元素指向的地址
数组插入元素必须将后面的元素都向后移动
删除元素同理
数组支持随机访问，而链表只支持顺序访问
二分查找
输入：有序的元素列表
运行时间：O(log(n))

具体实现：
```bash
function search(arr,key) {
    // 数组第一个元素的索引
    var low=0;
    // 数组最后一个元素的索引
    var height=arr.length-1;
    // 中间值
    var mid;
    while(low<=height){
        // 中间值要取整
        mid=Math.floor((low+height)/2); 
        if(arr[mid]==key){
            // 找到了，返回该索引
            return mid;
        }else if(arr[mid]<key){
            // 中间值小于查找值，就用中间值代替最小值
            low=mid+1;            
        }else{
            // 高于查找值就代替最大值
            height=mid-1;
        }
    }
    // 找不到就不存在了
    return -1;
}
```
选择排序
暴力遍历数组，O(n²)
## 栈
栈就像一叠便签，最上面压入新的便签，阅读后也是删除最上面的便签

调用栈(call stack)
```bash
 function boo (a) {
    return a
  }
  function foo (b) {
    return boo(4)
  }
  console.log(foo(3))

```
console.log(foo(3)) 执行，形成一个栈帧，调用foo函数，再形成另一个栈帧。
新的栈帧压在上一个栈帧之上，继续执行代码，foo函数中又调用了boo函数，形成了另一个栈帧压在旧栈帧之上。然后执行boo。
当执行完boo时候，返回值给foo函数之后,boo被推出调用栈,foo函数继续执行，然后foo函数执行完，返回值给console.log,foo函数被推出调用栈，console.log得到foo函数的返回值，运行，输出结果，最后console.log也被推出调用栈，该段程序执行完成。
所有函数调用都进入调用栈
调用栈可能很长，这将占用大量内存


递归
如果使用循环，程序的性能可能更高；如果使用递归，程序可能更容易理解
每个递归函数都有两部分：基线条件和递归条件

基线条件(base case)：函数不再调用自己，避免无限循环
递归条件(recursive case)：函数调用自己
递归调用栈
