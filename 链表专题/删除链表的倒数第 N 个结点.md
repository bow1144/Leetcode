#  删除链表的倒数第 N 个结点

> [删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/description/)


## 题目大意
给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点

### 题目关键
* 找到**倒数**第`n`个节点
* 找到它的**前一个节点**方便删除

## 方法一、栈

### 思路
1. 数据结构**栈**的特点：后入先出
2. 寻找倒数第几个入栈的就是寻找正数第几个出栈的
3. 由于栈的特性，可以很容易找到删除点的前一个节点

### 整体代码
```
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy = new ListNode(0, head);
        ListNode* curr = dummy;
        stack<ListNode*> stk;

        while(curr){
            stk.push(curr);
            curr = curr->next;
        }

        for(int i=0;i<n;i++){
            stk.pop();
        }

        ListNode* prev = stk.top();
        prev->next = prev->next->next;

        return dummy->next;
    }
};
```

### 代码细节
1. `ListNode* dummy = new ListNode(0, head);`创建哑节点的原因是链表的**头节点会被更改**
2. `ListNode* prev = stk.top();`关键点是找到删除节点的前一个节点

### 时空复杂度分析
* 入栈时进行了整体扫描，出栈时扫描到目标，总时间复杂度为 $O(n)$
* 用栈对整个链表存储，空间复杂度为 $O(n)$ 

## 方法二、双指针|追赶指针

### 思路
1. 我们要求的是目标节点到链表尾的距离，那么可以用**双指针**来表示这个距离
2. 让`fast`比`slow`先出发n，当`fast`到达尾部时，`slow`到达应删除节点

### 整体代码
```
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode *dummy = new ListNode(-1);
        dummy->next = head;
        ListNode *first = dummy, *second = dummy;

        for(int i=0;i<n;i++) first = first->next;

        while(first->next){
            first = first->next;
            second = second->next;
        }

        second->next = second->next->next;
        return dummy->next;
    }
};
```

### 代码细节
1. ` while(first->next)` 此处找`next`指针的原因是，慢指针的目的是删除节点的前一个而非删除节点

### 时空复杂度分析
* 两个指针加起来跑满了整个链表，时间复杂度为 $O(n)$
* 本题除了两个指针外没有别的空间，空间复杂度为 $O(1)$
