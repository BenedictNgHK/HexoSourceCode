---
title: Two Sum
author: Benedict Ng
categories:
    - LeetCode
---

**[Link to Problem](https://leetcode.com/problems/two-sum/)**

This is the very beginning of LeetCode, where the dream starts (or ends 🤣). Enjoy your journey!

## Problem Description

```txt
Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

 

Example 1:

Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
Example 2:

Input: nums = [3,2,4], target = 6
Output: [1,2]
Example 3:

Input: nums = [3,3], target = 6
Output: [0,1]
 

Constraints:

2 <= nums.length <= 104
-109 <= nums[i] <= 109
-109 <= target <= 109
Only one valid answer exists.
 

Follow-up: Can you come up with an algorithm that is less than O(n2) time complexity?
```

## Solution

The problem is quite simple if you just try to crack it down. The first thought is to use 2 nested `for` loops, the outer loop traverses the whole array and the inner loop starts from next position of the outer pointer. When the sum of 2 values matches the `target`, return the 2 pointers as an array.

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> ret_val;
        for(int i = 0; i < nums.size(); i++)
        {
            /*
                note: j must be different from i; otherwise, the same
                index may appear twice in the ret_val
            */
            for(int j = i+1; j < nums.size(); j++)
            {
                if(nums[i] + nums[j] == target)
                {
                    ret_val.push_back(i);
                    ret_val.push_back(j);
                    return ret_val;
                }
                else
                    continue;
            }
        }
        return ret_val;
    }
};
```

However, this will results in O(N^2) time complexity, that is very undesirable; and the it doesn't match the follow-up requirement as well. Let's dive deeper to see how to solve the follow-up.

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) 
    {
        map<int,int> a;
        vector<int> ret_val(2,-1);
        for(int i = 0; i < nums.size(); i++)
        {
            

            a.insert(map<int,int>::value_type(nums[i],i));              // Insert an element and its index
            if(a.count(target-nums[i]) >0 && a[target-nums[i]] != i)    // Check if the difference between target and nums[i] exists
            {
                ret_val[1] = i;
                ret_val[0] = a[target-nums[i]];
                break;
            }
        }
        
        return ret_val;
    }
};
```

The `std::map` is an stl container and it is implemented with red black tree. So its insertion and search are both O(log(N)). With the outer for loop, the time complexity of the code is reduced to O(N*log(N)), which meets the requirement of follow-up.

This snippet of code insert a key-value pair into `std::map` in each `for` loop, and check if the difference between `target` and `nums[i]` exists in the `std::map a`. The assertion `a[target-nums[i]] != i` is to avoid duplicates in the return value, since `a.insert()` is before `a.count()`.

And this is the end of my note. Enjoy your day! 