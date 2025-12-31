+++
date = '2025-09-05T09:41:13+08:00'
draft = false
title = 'Coding Journal_day3'
image = "img/code.jpg"
license = false
math = true
categories = [
  "代码随想录",
  "c++"
]
+++
代码仓库: <git@github.com>:Sophomoresty/Coding-Journal.git

## 2 移除链表的元素
```c++
// leetcode_203_移除链表的元素

struct ListNode {
    int val;
    ListNode *next;
    ListNode() : val(0), next(nullptr) {}
    ListNode(int x) : val(x), next(nullptr) {}
    ListNode(int x, ListNode *next) : val(x), next(next) {}
};
// 采用虚拟头结点

class Solution {
public:
    ListNode *removeElements(ListNode *head, int val) {
        // C创建结构体
        // struct ListNode *dummmy_head;
        // dummmy_head = (ListNode *)malloc(sizeof(ListNode));
        // c++创建结构体
        ListNode *dummy_head = new ListNode();
        dummy_head->next = head;
        ListNode *cur = dummy_head;
        // 找到目标节点, 并删除, 可能有多个, 故是循环
        while (cur->next != nullptr) {
            //找到目标节点
            if (cur->next->val == val) {
                // 1.保存被删除节点
                ListNode *temp;

                temp = cur->next;
                // 2.改指针
                cur->next = cur->next->next;
                // 释放该节点
                delete temp;
            }
            // 未找到
            else {
                cur = cur->next;
            }
        }
        ListNode *result = dummy_head->next;

        delete dummy_head;
        return result;
    }
};
```



## 3 设计链表

```c++
// leetcode_707_设计链表


struct ListNode {
    int val;
    ListNode *next;
    ListNode() : val(0), next(nullptr) {}
    ListNode(int x) : val(x), next(nullptr) {}
    ListNode(int x, ListNode *next) : val(x), next(next) {}
};

// 采用虚拟头节点

class MyLinkedList {
private:
    ListNode *dummy_head;
    int _size;

public:
    // 初始化
    MyLinkedList() {
        dummy_head = new ListNode();
        _size = 0;
    }

    // 题目给的index>0, 所以后面<0的判断可以取消

    // 循环推进遍历指针找到index对应的节点
    int get(int index) {
        // index合法值: 0-_size-1,
        if (index < 0 || index > _size - 1) {
            return -1;
        }
        // i表示步长计数, 0到index, 步长为index
        // 记住对应关系, 本次循环中的循环变量i要和循环内部变量执行后对应
        // 故cur必须是从头节点开始, 前面已经排除了零节点
        ListNode *cur = dummy_head->next;
        for (int i = 1; i <= index; i++) {
            cur = cur->next;
        }
        return cur->val;
    }
    // 插入到头部
    void addAtHead(int val) {
        ListNode *node = new ListNode(val);
        node->next = dummy_head->next;
        dummy_head->next = node;
        _size++;
    }

    void addAtTail(int val) {

        ListNode *node = new ListNode(val);

        ListNode *cur = dummy_head;
        // 用cur找尾节点
        while (cur->next != nullptr) {
            cur = cur->next;
        }
        cur->next = node;
        _size++;
    }

    void addAtIndex(int index, int val) {
        // 合法性判断
        // 合法范围index: 0-_size-1
        // index为_size, 插入尾部, 调用用  addAtTail
        // index>_size, 不会插入
        if (index > _size) {
            return;
        }
        if (index == _size) {
            addAtTail(val);
            return;
        }
        // 需要找到index-1索引处的节点, 进行插入
        // 这里处理要注意, index可以取0, 取0即进行头插, 这里最好调用函数
        if (index == 0) {
            addAtHead(val);
            return;
        }
        ListNode *node = new ListNode(val);
        ListNode *cur = dummy_head->next;
        // i作为步数计数器, 0-index-1, 步数需要index-1
        for (int i = 1; i < index; i++) {
            cur = cur->next;
        }
        // 出来的cur即为索引index-1的节点
        node->next = cur->next;
        cur->next = node;
        _size++;
    }

    void deleteAtIndex(int index) {
        // 有效索引index:0-_size-1;
        // 题目给的index>0, 所以无效是>_size-1
        if (index > _size - 1) {
            return;
        }
        ListNode *cur = dummy_head;
        // i依然作为步数计数器, 这次从-1出发, 因为index可以取0
        // 需要找到索引index-1的节点
        // 从-1到index-1, 步长为index
        for (int i = 1; i <= index; i++) {
            cur = cur->next;
        }
        // cur现在是index-1索引的节点的指针
        ListNode *temp = cur->next;
        cur->next = temp->next;
        delete temp;
        _size--;
    }

    // 类结束调用后, 会自动与性能的函数, 需要释放链表节点和虚拟头节点
    ~MyLinkedList() {
        ListNode *cur = dummy_head->next;
        while (cur != nullptr) {
            ListNode *temp = cur;
            cur = temp->next;
            delete temp;
        }
        delete dummy_head;
    }
};
```

## 4 反转链表
**1.判断是否可以用递归**
1) 原问题是否可以划分结构相同的子问题, 可以, 翻转n个节点与翻转n-1个节点, 本质是一类问题
2) 假设子问题解决后, 是否能解决原问题

**2.函数签名与规约**
- 输入: 当前链表的头节点
- 输出: 反转链表后的头节点 `归纳假设`

**3.处理基本情况, 归纳基础**
处理最基本情况, 2个节点反转, 0和1个节点,直接返回

**4.归纳证明**
我们需要反转1-n个节点
假设1-n个节点已经反转, 所以只需要让2-n个尾节点指向1, 1指向空
head->next->next =head;
head->next =nullptr

```c++
// leetcode_206_反转链表
struct ListNode {
    int val;
    ListNode *next;
    ListNode() : val(0), next(nullptr) {}
    ListNode(int x) : val(x), next(nullptr) {}
    ListNode(int x, ListNode *next) : val(x), next(next) {}
};



class Solution {
public:
    ListNode *reverseList(ListNode *head) {
        if (head == nullptr || head->next == nullptr) {
            return head;
        }
        // 归纳证明
        ListNode *new_head = reverseList(head->next);
        head->next->next = head;
        head->next = nullptr;
        return new_head;
    }
};
```

