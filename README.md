> 基础知识的货架，方便自己查看

# 🎁🎁1.计算机语言
## 1.1 python
 [python基础知识](https://github.com/LuckX/Knowledge-shelf/blob/master/doc/python/Python%E5%9F%BA%E7%A1%80.md)
## 1.2 js
 [js基础](https://github.com/LuckX/Knowledge-shelf/blob/master/doc/JavaScript/Js%E5%9F%BA%E7%A1%80.md)

 ---
# 😈😈2. 算法
## 2.1 算法思想
> 刷算法题一定要有算法思想，建议首先阅读[学习算法和刷题的思路指南](https://labuladong.gitee.io/algo/1/2/),截取重要的几句如下，**1.数据结构的存储方式只有两种：数组（顺序存储）和链表（链式存储）；2.分析问题，一定要有递归的思想，自顶向下，从抽象到具体；3.试着从框架上看问题，而不要纠结于细节问题；**
- 单调栈
- 并查集
- 滑动窗口（解决查找满足一定条件的连续区间性质的问题）
- 前缀和&&HASH（从一维到二维https://leetcode-cn.com/circle/discuss/SrePlc/）
- 拆分
- 拓扑排序
- BFS
- DFS
- 动态规划
- 贪心算法
- 字典树

---
# 👘👘3.Android
> 关于安卓Framework的一些基础知识，便于查看
- [服务](https://github.com/LuckX/Knowledge-shelf/blob/master/doc/Android/%E6%9C%8D%E5%8A%A1.md)
- 跨进程通信
   - [BindService](https://github.com/LuckX/Knowledge-shelf/blob/master/doc/Android/%E6%9C%8D%E5%8A%A1%E9%80%9A%E4%BF%A1%E2%80%94%E2%80%94BindService.md)（使用bindService方式启动服务；）
   - [Messenger](https://github.com/LuckX/Knowledge-shelf/blob/master/doc/Android/%E6%9C%8D%E5%8A%A1%E9%80%9A%E4%BF%A1%E2%80%94%E2%80%94Messenger.md)（实现比较简单，但是相比于AIDL,有两个缺点：1. 只能串行的解决请求,无法处理大量的并发请求；2. 只能通过Message来传递信息实现交互无法直接跨进程调用服务端的方法；）
   - [AIDL](https://github.com/LuckX/Knowledge-shelf/blob/master/doc/Android/%E6%9C%8D%E5%8A%A1%E9%80%9A%E4%BF%A1%E2%80%94%E2%80%94AIDL.md)（一种Android定义的接口语言，BindService的另外一种实现）
   - [AsyncChannel](https://github.com/LuckX/Knowledge-shelf/blob/master/doc/Android/%E6%9C%8D%E5%8A%A1%E9%80%9A%E4%BF%A1%E2%80%94%E2%80%94AsyncChannel.md)（Android内部实现的机制，并未对外提供，主要用于wifiservice）

