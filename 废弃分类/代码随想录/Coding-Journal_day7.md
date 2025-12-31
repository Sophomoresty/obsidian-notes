 +++
date = '2025-09-10T23:03:01+08:00'
draft = false
title = 'Coding Journal_day7'
image = "img/code.jpg"
license = false
math = true
categories = [
  "代码随想录",
  "c++"
]
+++
代码仓库: git@github.com:Sophomoresty/Coding-Journal.git


## 6_四数相加II
```c++
// leetcode_454_四数相加II
#include <unordered_map>
#include <vector>
using namespace std;

/*
只需要返回四数之和为0的四元组的数量,

两层for循环, 构建map, key: 两数之和, val:出现的次数
两层for循环遍历剩余两个数组, 查找map中是否存在0-(数组之和), 找到了count进行+=, 也就是存储了目标四元组的数量 

*/
class Solution {
public:
    int fourSumCount(vector<int> &nums1, vector<int> &nums2, vector<int> &nums3,
                     vector<int> &nums4) {
        unordered_map<int, int> hash_map;
        for (int i = 0; i < nums1.size(); i++) {
            for (int j = 0; j < nums2.size(); j++) {
                hash_map[nums1[i] + nums2[j]]++;
                // map容器会动态创建, 并且初始化值为0, 这个特性 太牛逼了
            }
        }
        int count = 0; // 符合条件的四元组计算
        for (int i = 0; i < nums3.size(); i++) {
            for (int j = 0; j < nums4.size(); j++) {
                int complement = -(nums3[i] + nums4[j]);
                if (hash_map.find(complement) != hash_map.end()) {
                    count += hash_map[complement];
                }
            }
        }
        return count;
    }
};
```

## 7_赎金信
```c++
// leetcode_383_赎金信
#include <string>
#include <unordered_map>
using namespace std;

/*
思路: 
两次哈希表, 先把magazine的数组的字符作为key, value为对应字符出现的次数, 然后再遍历ransomNote, 出现1次, 则value--.

每次--后做判断, 如果value<0, 说明不够, return false

*/
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        unordered_map<char, int> hash_map;
        for (int i = 0; i < magazine.size(); i++) {
            hash_map[magazine[i]]++;
        }
        for (int i = 0; i < ransomNote.size(); i++) {
            hash_map[ransomNote[i]]--;
            if (hash_map[ransomNote[i]] < 0) {
                return false;
            }
        }
        return true;
    }
};
```

## 8_三数之和
```c++
// leetcode_15_三数之和
#include <algorithm>
#include <vector>
using namespace std;

class Solution {
public:
    vector<vector<int>> threeSum(vector<int> &nums) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> res;
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] > 0) {
                break;
            }
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }

            int left = i + 1;
            int right = nums.size() - 1;
            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];
                if (sum > 0) {
                    right--;
                } else if (sum < 0) {
                    left++;
                } else {
                    res.push_back({nums[i], nums[left], nums[right]});
                    while (left < right && nums[left] == nums[left + 1]) {
                        left++;
                    }
                    while (left < right && nums[right] == nums[right - 1]) {
                        right--;
                    }
                    left++;
                    right--;
                }
            }
        }
        return res;
    }
};
```

## 9_四数之和
```c++
// leetcode_18_四数之和
#include <algorithm>
#include <vector>
using namespace std;

class Solution {
public:
    vector<vector<int>> fourSum(vector<int> &nums, int target) {
        if (nums.size() < 4) {
            return {};
        }

        sort(nums.begin(), nums.end());
        vector<vector<int>> res;
        // 循环条件有问题
        for (int i = 0; i < nums.size() - 3; i++) {
            // 这个剪枝简直是天才, 简单, 但是有效
            // 防止溢出
            long long sum_i_min =
                (long long)nums[i] + nums[i + 1] + nums[i + 2] + nums[i + 3];
            long long sum_i_max = (long long)nums[i] + nums[nums.size() - 3] +
                                  nums[nums.size() - 2] + nums[nums.size() - 1];
            // 如果初始的四个数之和都大于target, 肯定没戏, 但是可能前面有正确结果, 不能return {}
            // break掉即可
            if (sum_i_min > target) {
                break;
            }
            // 初始+最大的三个数都小于target, 本轮肯定没戏
            if (sum_i_max < target) {
                continue;
            }
            // i去重
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }

            // j肯定得在i之后
            // j最多走到nums.size()-3
            for (int j = i + 1; j < nums.size() - 2; j++) {
                long long sum_j_min =
                    (long long)nums[i] + nums[j] + nums[j + 1] + nums[j + 2];
                long long sum_j_max = (long long)nums[i] + nums[j] +
                                      nums[nums.size() - 2] +
                                      nums[nums.size() - 1];
                if (sum_j_min > target) {
                    break; // 这里原先也是return {}, 前面可能有正确结果
                }
                if (sum_j_max < target) {
                    continue;
                }
                // j去重, j的初始值是i+1, 必须>i+1, 才有去重的需要
                if (j > i + 1 && nums[j] == nums[j - 1]) {
                    continue;
                }
                int left = j + 1;
                int right = nums.size() - 1;
                while (left < right) {
                    long long sum =
                        (long long)nums[i] + nums[j] + nums[left] + nums[right];
                    if (sum < target) {
                        left++;
                    } else if (sum > target) {
                        right--;
                    } else {
                        res.push_back(
                            {nums[i], nums[j], nums[left], nums[right]});
                        while (left < right && nums[left] == nums[left + 1]) {
                            left++;
                        }
                        while (left < right && nums[right] == nums[right - 1]) {
                            right--;
                        }
                        left++;
                        right--;
                    }
                }
            }
        }
        return res;
    }
};
```