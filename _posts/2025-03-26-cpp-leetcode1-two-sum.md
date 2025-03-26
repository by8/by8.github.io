---
layout: post
redirect_from:
  - /2025/03/26/cpp-leetcode1-two-sum/
title: template page
categories: [leetcode, 编程训练]
description: C++版 - Leetcode No.1 - Two sum题解
keywords: leetcode, 编程训练
---

# C++版 - Leetcode No.1 - Two sum题解
​
**1. Two Sum (2 sum)**

提交网址： <https://leetcode.com/problems/two-sum/>

Total Accepted: 216928  Total Submissions: 953417   **Difficulty:** Easy   **ACrate: 22.8%**

Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.

You may assume that each input would have ***exactly*** one solution.

**Example:**

Given nums = **[2, 7, 11, 15]**, target = **9**,
Because nums[0] + nums[1] = 2 + 7 = 9,
return **[0, 1]**.

**UPDATE (2016/2/13):**
The return format had been changed to **zero-based** indices. Please read the above updated description carefully. 

(**温馨提示：**此题以前的输出要求是：下标从1开始算，**2016/2/13**后改为下标从0开始算！)

**分析**
方法1: 暴力法，复杂度O(n^2)，会TLE(超时);
方法2: hashmap。用一个哈希表(C++中用unordered_map，Java中可以直接用HashMap)，存储每个数对应的下标，复杂度O(n);
方法3：先**变相**用C++ STL算法库中的sort函数，然后左右分别使用一个计数变量去比较和的值与目标值，前面的复杂度为O(n*log(n))，后面为n，所以总的复杂度为O(n*log(n))...

**温馨提示：**
此题对没找到的结果时的输出没有进行要求，有人输出了[-1, -1]，本人输出的是空的vector.

![](https://img-blog.csdn.net/20160510100517645)![](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw== "点击并拖拽以移动")​

## 方法3

AC代码：

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

typedef struct node{
    int index;
    int value;
    node(){};
    node(int i, int v) : index(i), value(v){}
}Node;

bool compare(const Node& a, const Node& b){
    return a.value < b.value;
}

class Solution {
public:
    vector<int> twoSum(vector<int> &nums, int target) {

        int len = nums.size();
        assert(len >= 2);

        vector<int> ret(2, 0);  // 初始化：ret包含2个值为0的元素

        vector<Node> nums2(len);
        for(int i = 0; i < len; i++){
            nums2[i] = Node(i+1, nums[i]);
        }

        sort(nums2.begin(), nums2.end(), compare);    // 调用快排，T(n)=O(n*log(n))

        int l = 0;
        int r = len - 1;
        while(l < r){
            int sum = nums2[l].value + nums2[r].value;
            if(sum == target){
                ret[0] = min(nums2[l].index, nums2[r].index)-1;     // 注意，这里需要减去1
                ret[1] = max(nums2[l].index, nums2[r].index)-1;
                break;
            } else if(sum < target){
                l++;
            } else {
                r--;
            }
        }
        return ret;  // 用两个指针来扫
    }
};

// 下面是测试代码

/*
int main()
{
    Solution sol;
    vector<int> arr;

    arr.push_back(3);
    arr.push_back(2);
    arr.push_back(4);

    vector<int> ret = sol.twoSum(arr, 6);
    cout<<ret[0]<<" "<<ret[1]<<endl;
    return 0;
}*/
```

![image.png](https://b.bdstatic.com/comment/btVm9ENZ4h2RcfZN9yiixw220e8408996b15f97edb15b6c48b76a5.png)


## 方法2 hashMap法
hashMap的一种写法：(顺序的，可以解决有相等的值的情况，推荐此写法！)

AC代码：

```cpp
#include<iostream>
#include<unordered_map>
#include<vector>
using namespace std;
class Solution {
	public:
		vector<int> twoSum(vector<int> &nums, int target)
		{
			unordered_map<int, int> dict;
			vector<int> result;
			for(int i = 0; i < nums.size(); i++) {
				dict[nums[i]] = i; // 顺序的map映射: value->index
			}
			for(int i = 0; i < nums.size(); i++)
			{
				int query = target - nums[i];
				if(dict.find(query) != dict.end() && dict[query] > i)  // dict[query] > i是为了防止重复计算
				{
					result.push_back(i);
					result.push_back(dict[query]);
					break;
				}
			}
			return result;
		}
};
// 下面是测试
int main()
{
    Solution sol;
    vector<int> arr1={3,2,2,2,2,2,4};
    vector<int> arr2={3,2,4};
    vector<int> res1= sol.twoSum(arr1, 6);
    vector<int> res2= sol.twoSum(arr2, 6);	
	for(int i:res1)
		printf("%d ",i);
	printf("n");
	for(int i:res2)
		printf("%d ",i);		
    return 0;
}
```

![](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw== "点击并拖拽以移动")

### haspMap的另一种写法

AC代码：

```cpp
#include<iostream>
#include<unordered_map>
#include<vector>
// #include<algorithm>
using namespace std;
class Solution {
public:
	vector<int> twoSum(vector<int> &nums, int target)
	{
		unordered_map<int, int> dict;
		vector<int> res(2,-1), emptyVect;
		for(int i=0;i<nums.size();i++)
		{
			int query=target-nums[i];		
			if(dict.find(query)==dict.end()) dict[nums[i]]=i;     // 逆序的map映射: value->index
			else {
				res[1]=i;
				res[0]=dict[query];
				return res;	
			}
		}
		return emptyVect;
	}
};
// 下面是测试
int main()
{
    Solution sol;
    vector<int> arr={3,2,4};
    vector<int> res = sol.twoSum(arr, 8);
    for(int i:res)
		printf("%d ",i);
    return 0;
}
```

![](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw== "点击并拖拽以移动")

提交后系统提示：

You are here!
Your runtime beats `55.48%` of cppsubmissions.

