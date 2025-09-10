+++
date = '2025-09-10T23:01:50+08:00'
draft = false
title = 'Coding Journal_day6'
image = "img/code.jpg"
license = false
math = true
categories = [
  "代码随想录",
  "c++"
]
+++
代码仓库: git@github.com:Sophomoresty/Coding-Journal.git

## 2_有效的字母异位词
```c++
#include <string>
#include <vector>
using namespace std;

class Solution {
public:
    bool isAnagram(string s, string t) {
        if (s.length() != t.length()) {
            return false;
        }
        // 初始化数组
        vector<int> letter(26, 0);
        for (int i = 0; i < s.size(); i++) {
            letter[s[i] - 'a']++;
        }
        for (int i = 0; i < t.size(); i++) {
            letter[t[i] - 'a']--;
        }
        // 如果两数组相同, 则数组全0
        for (int i = 0; i < letter.size(); i++) {
            if (letter[i] != 0) {
                return false;
            }
        }
        return true;
    }
};
```

## 3_两个数组的交集
```c++
// leetcode_349_两个数组的交集

#include <unordered_set>
#include <vector>
using namespace std;

/*
之前是直接用vector存结果, 会存在一个问题, 就是nums2存在和nums1相同的, 多个元素, 会反复加入,所以最佳办法是用unordered_set存储结果, 最后返回vector
*/

class Solution {
public:
    vector<int> intersection(vector<int> &nums1, vector<int> &nums2) {
        unordered_set<int> res;
        unordered_set<int> hash_table(nums1.begin(), nums1.end());
        for (int i = 0; i < nums2.size(); i++) {
            if (hash_table.find(nums2[i]) != hash_table.end()) {
                res.insert(nums2[i]);
            }
        }
        return vector<int>(res.begin(), res.end());
    }
};
```

## 4_快乐数
```c++
// leetcode_202_快乐数

/*
计算快乐数, 每计算1次, 将原数放入哈希表中, 如果后续的数在计算前在哈希表中找到了, 则代表进入循环

即, 结束函数两种情况, 计算值为1, 退出循环, return true
在哈希表中找到值, return false

*/
#include <unordered_set>
using namespace std;

class Solution {
public:
    void get_happy(int &n) {
        // 现在需要一个sum保存每位的平方和
        int sum = 0;
        // 什么时候退出循环, n变得只有0位了, 即n==0
        // 最后要修改n, sum保存和, 最后赋值给n
        while (n) {
            // 每次对10取余都会获得最低位, 同时要求n要继承变化
            sum += (n % 10) * (n % 10);
            n = n / 10;
        }
        n = sum;
    }

    bool isHappy(int n) {
        unordered_set<int> hash_tab;
        while (n != 1) {
            // 加入每轮循环初始的n
            hash_tab.insert(n);
            // 将n进行快乐数计算
            get_happy(n);
            // 如果得到的快乐数,在哈希表中找到了,代表已经发生了循环
            // 最后得到1的快乐数序列, 中间不存在重复的数
            if (hash_tab.find(n) != hash_tab.end()) {
                return false;
            }
        }
        return true;
    }
};
```

## 5_两数之和
```c++
// leetcode_1_两数之和

#include <unordered_map>
#include <vector>
using namespace std;

/*
前面的做法确实, 不太合理, 最合适的方式还是使用哈希map

*/
class Solution {
public:
    vector<int> twoSum(vector<int> &nums, int target) {
        // 数值:索引
        unordered_map<int, int> hash_map;
        for (int i = 0; i < nums.size(); i++) {
            // 遍历数组
            // 期望找到的值
            int complenmt = target - nums[i];
            // 如果找到了
            if (hash_map.find(complenmt) != hash_map.end()) {
                // 这里返回的索引值是如何写的
                return {hash_map[complenmt], i};
            }
            // 没找到则加入
            else {
                hash_map[nums[i]] = i;
            }
        }
        return {};
    }
};
```

