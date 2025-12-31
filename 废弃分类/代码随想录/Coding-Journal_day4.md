+++
date = '2025-09-05T09:54:16+08:00'
draft = false
title = 'Coding Journal_day4'
image = "img/code.jpg"
license = false
math = true
categories = [
  "代码随想录",
  "c++"
]
+++
代码仓库: git@github.com:Sophomoresty/Coding-Journal.git

## 5 两两交换节点

**1.判断是否能用递归解决**
1) 问题分解同构 
2) 原问题和子问题的解存在关联


**2.函数签名与规约**
- 输入: 当前链表的头节点
- 输出: 两两交换后的链表头节点

**3.归纳基础**
空节点或则1个节点, 2个节点是不是基本情况?

不是, 不能直接得到平反解.


**4.归纳证明**
```c++
// leetcode_24_两两交换节点

struct ListNode {
    int val;
    ListNode *next;
    ListNode() : val(0), next(nullptr) {}
    ListNode(int x) : val(x), next(nullptr) {}
    ListNode(int x, ListNode *next) : val(x), next(next) {}
};
/*
1. 判断是否能用递归解决
    1) 问题分解同构 
    2) 原问题和子问题的解存在关联

2. 函数签名与规约
输入: 当前链表的头节点
输出: 两两交换后的链表头节点

3. 归纳基础
空节点或则1个节点, 2个节点是不是基本情况?
不是, 不能直接得到平反解.

4. 归纳证明


*/

class Solution {
public:
    ListNode *swapPairs(ListNode *head) {
        if (head == nullptr || head->next == nullptr) {
            return head;
        }
        ListNode *new_head = swapPairs(head->next->next);
        ListNode *temp = head->next;
        head->next = new_head;
        temp->next = head;
        return temp;
    }
};
```

## 6 删除链表倒数第n个节点
这是1个数学问题
这里全部按序号叙述, 设链表有$n_总$个节点

倒数第1个节点序号即为$n_总$
倒数第n个节点序号即为$n_总-n+1$

考虑需要删除首元节点, 所以遍历节点需要从虚拟节点, 即序号0出发, 找到倒数n+1个节点的序号.

即从$0\rightarrow n_总-n$, 步长为$n_总-n$

这个步长$n_总$对于链表而言是未知, 因为链表是不具备长度和随机访问的特性
我们需要用1个节点走到链表空节点, 即$n_总+1$, 使其步数刚好为$n_总-n$
这样我们的遍历节点和它同步走, 就能走$n_总-n$
$n_总-n=(n_总+1)-(n+1)$, 即这个节点从序号$n+1$走到$n_总+1$
需要想下边界情况是否成立, 比如倒数第$n_总$个, 没问题

```c++
// leetcode_19_删除链表倒数第n个节点

struct ListNode {
    int val;
    ListNode *next;
    ListNode() : val(0), next(nullptr) {}
};



class Solution {
public:
    ListNode *removeNthFromEnd(ListNode *head, int n) {
        ListNode *dummy_head = new ListNode();
        dummy_head->next = head;
        ListNode *fast, *slow;
        fast = slow = dummy_head;
        // fast从0到n+1, 步数n+1
        for (int i = 1; i < n + 2; i++) {
            fast = fast->next;
        }
        // fast和slow同步走
        // fast: n+1->n_总+1, 即fast==nullpter停下来
        // slow: 0->n_总-n
        while (fast != nullptr) {
            fast = fast->next;
            slow = slow->next;
        }
        ListNode *temp = slow->next;
        slow->next = slow->next->next;
        delete temp;
        ListNode *new_head = dummy_head->next;
        delete dummy_head;
        return new_head;
    }
};
```

## 8 环形链表II
这个题我重新做一遍发现最难思考的逻辑是
慢指针从入口点到和快指针相遇, 慢指针有没有可能转过一圈?
设环的节点个数为r

快指针可能出现在环形链表的任意一点, 与慢指针相对距离最大只能是r-1,即慢指针在快指针前面.

设运动n次, 两指针相遇, 得到n=r-1, 即慢指针与快指针相遇, 最多走r-1步, 而慢指针走过一圈必须大于等于r.
所以慢指针和慢指针相遇时, 慢指针就在第一圈被追到了.
s_slow = x+y
s_fast = x+y+nr
s_fast = 2 s_slow
x+y = nr
x= (n-1)r+r-y, 记r-y=z
x=(n-1)r+z
z是相遇点到入口点的距离, 假设指针从相遇点出发, 它的入口点走过的路程就是z+nr
所以只要从起点出发的指针, 到入口点, 走过x步, 一定会和从相遇点出发的指针相遇

```c++
// leetcode_142_环形链表II
struct ListNode {
    int val;
    ListNode *next;
    ListNode() : val(0), next(nullptr) {}
};



class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode *slow, *fast;
        slow = fast = head;
        while (fast != nullptr && fast->next != nullptr) {
            slow = slow->next;
            fast = fast->next->next;
            if (slow == fast) {
                // 让slow从头开始走, fast从相遇点开始走
                slow = head;
                while (slow != fast) {
                    slow = slow->next;
                    fast = fast->next;
                }
                return slow;
            }
        }

        return nullptr;
    }
};
```