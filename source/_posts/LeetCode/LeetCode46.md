---
title: Permutations
author: Benedict Ng
categories:
    - LeetCode
---
**[Link to Problem](https://leetcode.com/problems/permutations/)**

## Problem Description

```txt
Given an array nums of distinct integers, return all the possible permutations. You can return the answer in any order.

Example 1:

Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
Example 2:

Input: nums = [0,1]
Output: [[0,1],[1,0]]
Example 3:

Input: nums = [1]
Output: [[1]]
 
Constraints:

1 <= nums.length <= 6
-10 <= nums[i] <= 10
All the integers of nums are unique.
```

## Solution

### Base case

To begin with, let's consider the case that the size of the array is fixed. If the array size is 3, for instance: `arr = [1,2,3]` The permutations of the array are:

```c++
[[1,2,3],
 [1,3,2],
 [2,1,3],
 [2,3,1],
 [3,1,2],
 [3,2,1]
]
```

If the size of the array is fixed, we can use nested for loops to generate the output. How many `for` loops are required? It's easy to see, the answer equals the array size. The code is as follows:

```c++
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
  
    /* 
        Note: the following code noly suitable for the 
        case that the nums size is 3!
    */
        vector<vector<int>> ret_val;
        // store the one-dimensional array;
        vector<int> help; 
        help.resize(nums.size());
        for(int i = 0; i < nums.size(); i++)
        {
            help[0] = nums[i];
            for(int j = 0; j < nums.size();j++)
            {   
                // Avoid same element in array visited twice!
                if(i == j)
                    continue;
                help[1] = nums[j];
                for(int k = 0; k < nums.size(); k++)
                {
                /*
                    As above, to void repeatedly access the same element,
                    which is not allowed.
                */
                    if(k == i || k == j)
                        continue;
                    help[2] = nums[k];
                    // Add help to ret_val;
                    ret_val.push_back(help);
                }
            }
        }
        return ret_val;
    }
};
```

 However, if the array size is variable, how to generalize to all possible results? A recursive algorithm is needed.

### Generalization

We still take the previous example. The problem is, how to generate the help arrays recursively?

The first help array, obviously, is `[1,2,3]`. And the second is `[1,3,2]`. How to get `[1,3,2]` from `[1,2,3]`? It looks pretty tricky at first glance, but by using recursion, everything gets clearer! The subarrays `[2,3]` and `[3,2]`, are actually permutations of the array `[2,3]`, which is the help array discarding the first element 1! So to generate the output we just need to concatenate two arrays, by pseudo-code:

```txt
(elements already permuted) 
+
permute(the rest elements of the input array)
```

The pseudo-code has already shown us that we need to know if the currently visited element in nums is visited or not. If it is visited, it should be put into elements already permuted array, otherwise, it should be put into the rest elements array and wait for permuting. How to do this?

First, we need a bucket that stores an array of bool type to mark if each element in the array is visited.

And try to visit every element in the nums. This is simple, so we'll jump to the next step. If the current element has not visited yet,  mark the `Bucket[i] = true`, add it to the array already permuted, and permute the rest elements. After permuting the rest elements, and reset `Bucket[i]`  to false.

```c++
  if(Bucket[i] == false)
{  
    // Add current element to already permuted array
    help_arr[help_index] = nums[i];
    // Denote that current element is visited
    Bucket[i] = true;
    // Permute rest elements
    help(nums, Bucket, help_arr, help_index+1);
    // Reset the current elements as unvisited
    Bucket[i] = false;
}
```

If the current element is visited, then is nothing worthy to be mentioned, just continue.

Lastly, every recursion has a return condition. When do we return? Obviously, when the size of the array that stores the result equals to nums!

```c++
    // the help_arr has already reaches to the end
    if(help_index >= nums.size() )
    {
        // Add the help_arr to ret_val and return
        ret_val.push_back(help_arr);
        return;
    }
```

Now the whole algorithm completes. The full code is as follows:

```c++
class Solution{
public:
    /*
    Note:
    ret_val should be claimed outside the permute function!!!
    */
    vector<vector<int>> ret_val;
    vector<vector<int>> permute(vector<int>& nums) 
    {
        vector<int> help_arr(nums.size());
        bool * Bucket  = new bool[nums.size()];
  
        memset(Bucket,false,sizeof(bool)*nums.size());
        help(nums,Bucket, help_arr, 0);
  
        delete [] Bucket;
  
        return ret_val;
    }
    void help(vector<int>& nums,  bool * Bucket, vector<int>& help_arr, int help_index)
{
        for(int i = 0; i < nums.size(); i++)
        {
            // return condition
            // the help_arr has already reaches to the end
            if(help_index >= nums.size() )
            {
                // Add the help_arr to ret_val and return
                ret_val.push_back(help_arr);
                return;
            }
            else if(Bucket[i] == false)
            {
                // Add current element to already permuted array
                help_arr[help_index] = nums[i];
                //Denote that current element is visited
                Bucket[i] = true;
                // Permute rest elements
                help(nums, Bucket, help_arr, help_index+1);
                // Reset the current elements as unvisited
                Bucket[i] = false;
            }
            else
                continue;
        }
    }
};
```
