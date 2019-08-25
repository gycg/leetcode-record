

# LeetCode题目记录

## 目录

* [Two Sum](#twosum)
* [Add Two Numbers](#AddTwoNumbers)
* [Longest Substring Without Repeating Characters](#LongestSubstringWithoutRepeatingCharacters)
* [Longest Palindromic Substring](#LongestPalindromicSubstring)

#### TwoSum

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        map<int, int> m;
        int l = nums.size();
        vector<int> ret;
        for(int i=0; i<l; i++){
            if(m.count(nums[i])){
                ret.push_back(m[nums[i]]);
                ret.push_back(i);
                return ret;
            }
            else{
                m[target-nums[i]] = i;
            }
        }
        return ret;
    }
};
```



#### AddTwoNumbers

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode *l = new ListNode(0);
        ListNode *cur = l;
        int sum, carry=0;
        int v1, v2;
        while(l1 || l2){
            v1 = l1 ? l1->val : 0;
            v2 = l2 ? l2->val : 0;
            int sum = v1 + v2 + carry;
            cur->next = new ListNode(sum % 10);
            carry = sum / 10;
            if(l1) l1 = l1->next;
            if(l2) l2 = l2->next;
            cur = cur->next;
        }
        if(carry){
            cur->next=new ListNode(carry);
        }
        return l->next;
    }
};
```

#### LongestSubstringWithoutRepeatingCharacters
* 没有重复的最长子串

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        map<int, int> m;
        int j=0;
        int ret=0;
        for(int i=0; i<s.size(); i++){
            if(m.count(s[i])){
                j = max(j, m[s[i]]);
            }
            m[s[i]] = i+1;  
            ret = max(ret, i-j+1);
        }
        return ret;
    }
};
```

#### LongestPalindromicSubstring
* 最长回文子串

```c++
// 从第1位到第n位，以每一位做为中点向左向右扩展，判断最长的回文子串；每两位之间的间隙也可以作为中点，共判断2*n-1次，取最长那个的下标，返回答案。
```

#### ZigZag Conversion

```c++
// 定义一个大小为numRows的string vector，利用一个goingDown的flag判断向上还是向下。遍历一次字符串，将字符添加到对应的行，最后将几个string连接返回。
```

####快排

```c++
# 最好的时间复杂度O(nlgn)，最差O(n^2)。平均O(nlgn)
#include <iostream>
#include <vector>
using namespace std;

class quick_sort(vector<int> &arr, int left, int right){
  if(left >= right)
    return;
  int i=left, j=right;
  int pivot = arr[i];
  while(i<j){
    while(i<j && arr[j]>=pivot)
      j--;
    arr[i] = arr[j];
    while(i<j && arr[i]<=pivot)
      i++;
    arr[j] = arr[i];
  }
  arr[i] = pivot;
  quick_sort(arr, left, i-1);
  quick_sort(arr, i+1, right);
}

int main(){
    vector<int> a = {1, 2, 3, 7, 6, 5, 9, 8};
    quick_sort(a, 0, a.size()-1);
    for(int i=0; i<a.size(); i++)
        cout << a[i] << endl;
}
```

#### 匹配IP

```python
import re
pattern = re.compile(r'((2(5[0-5]|[0-4]\d))|[0-1]?\d{1,2})(\.((2(5[0-5]|[0-4]\d))|[0-1]?\d{1,2})){3}')
str = ''
print(pattern.search(str))

re.findall('[0-9]+(?:\.[0-9]+){3}', s)
```

#### Reverse Integer(反转整数)

```c++
#include <iostream>
using namespace std;

void reverse_int(int x){
  int ret = 0;
  while(x){
    int pop = x % 10;
    x = x / 10;
    if(ret > INT_MAX || (ret == INT_MAX && pop > 7)) return 0;
    if(ret < INT_MIN || (ret == INT_MIN && pop < -8)) return 0;
    ret = ret * 10 + pop;
  }
  return ret;
}
```

#### Reverse String

```python
' '.join(a.split()[::-1])
```

####两个有序数组的交集

```c++
vector<int> intersect(vector<int> arrA, vector<int> arrB){
  vector<int> ret;
  int la = arrA.size(), lb = arrB.size();
  int i=0, j=0;
  while(i<la && j<lb){
    if(arrA[i] == arrB[j]){
      ret.push_back(arrA[i]);
      i++;
      j++;
    }
    else if(arrA[i] > arrB[j])
      j++;
    else
      i++;
  }
  return ret;
}

// 如果arrB很长
vector<int> intersect(vector<int> arrA, vector<int> arrB){
  vector<int> ret;
  int la = arrA.size(), lb = arrB.size();
  for(int i=0; i<la; i++){
    int low = 0, high = lb-1;
    while(low < high){
      int median = (low + high) / 2;
      if(arrA[i] == arrB[median]){
        ret.push_back(arrA[i]);
        break;
      }
      else if(arrA[i] > arrB[median]){
        low = median + 1;
      }
      else{
        high = median - 1;
      }
    }
  }
  return ret;
}
```

#### Intersection of Two Arrays(两个数组求交集，hash)

```c++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        int l1 = nums1.size();
        int l2 = nums2.size();
        map<int, int> m;
        vector<int> ret;
        for(int i=0; i<l1; i++){
            m[nums1[i]] = 1;
        }
        for(int i=0; i<l2; i++){
            if(m.count(nums2[i])){
                if(m[nums2[i]] == 1){
                    ret.push_back(nums2[i]);
                }
                m[nums2[i]] -= 1;
            }
        }
        return ret;
    }
};
```

#### Combination Sum(和为target的所有组合)

```c++
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<int> tmp;
        vector<vector<int>> ret;
        backtrace(ret, candidates, target, 0, 0, tmp);
        return ret;
    }
    void backtrace(vector<vector<int>>& ret, vector<int>& candidates, int target, int prev, int sum, vector<int>& tmp){
        if(sum == target){
            ret.push_back(tmp);
            return;
        }
        if(sum > target)
            return;
        for(int i=prev; i<candidates.size(); i++){
            tmp.push_back(candidates[i]);
            backtrace(ret, candidates, target, i, sum + candidates[i], tmp);
            tmp.pop_back();
        }
    }
};
```

#### 找出和为target的三数组合

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& arr) {
        vector<vector<int>> ret;
	    	vector<int> tmp;
        int target = 0;
        sort(arr.begin(), arr.end());
        for(int i=0; i<arr.size(); i++){
            if(i>0 && arr[i]==arr[i-1])
                continue;
            int j = i+1, k = arr.size()-1;
            while(j < k){
                int sum = arr[i] + arr[j] + arr[k];
                if(sum > target){
                    k--;
                }
                else if(sum == target){
                    ret.push_back({arr[i], arr[j], arr[k]});
                    while(j+1<k && arr[j+1]==arr[j]) j++;
                    while(k+1>j && arr[k-1]==arr[k]) k--;
                    j++;
                    k--;
                }
                else{
                    j++;
                }
            }
        }
        return ret;
    }
};
```

####Binary Tree Level Order Traversal(层次遍历)

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ret;
        if(!root) return ret;
        queue<TreeNode*> q;
        q.push(root);
        vector<int> level;
        while(!q.empty()){
            level.clear();
            int s = q.size();
            for(int i=0; i<s; i++){
                TreeNode* p = q.front();
                q.pop();
                level.push_back(p->val);
                if(p->left){
                    q.push(p->left);
                }
                if(p->right){
                    q.push(p->right);
                }
            }
            ret.push_back(level);
        }
        return ret;
    }
};
```

#### 二叉树深度

```c++
int depth(TreeNode* root){
  if(!root) return 0;
  int left = depth(root->left);
  int right = depth(root->right);
  return left > right ? left + 1 : right +1;
}
```

#### Palindrome Number(回文数字)

```
class Solution {
public:
    bool isPalindrome(int x) {
        if(x < 0) return false;
        long long y = x, ret = 0;
        while(y){
            int pop = y % 10;
            y /= 10;
            if(ret * 10 > INT_MAX || (ret * 10 == INT_MAX && pop > 7)) return false;
            ret = ret * 10 + pop; 
        }
        return ret==x;
    }
};
```

#### 反转字符串

```
class Solution {
public:
    void reverseString(vector<char>& s) {
        int l = s.size();
        for(int i=0; i<l / 2; i++){
            char tmp = s[i];
            s[i] = s[l-i-1];
            s[l-i-1] = tmp;
        }
    }
};
```

####Knight Probability in Chessboard(走棋概率)

```c++
class Solution {
public:
    double knightProbability(int N, int K, int r, int c) {
        vector<vector<int>> dire = {{-1, -2}, {1, -2}, {2, -1}, {2, 1}, {1, 2}, {-1, 2}, {-2, 1}, {-2, -1}};
        vector<vector<double>> dp;
        dp[r][c] = 1;
        for(int step=0; step<K; step++){
            vector<vector<double>> dpTmp;
            for(int i=0; i<N; i++){
                for(int j=0; j<N; j++){
                    for(auto d: dire){
                        int lastR = i + d[0];
                        int lastC = j + d[1];
                        if(lastR>=0 && lastR<N && lastC>=0 && lastC<N)
                            dpTmp[i][j] += dp[lastR][lastC] * 0.125;
                    }
                }
            }
            dp = dpTmp;
        }
        double ret;
        for(int i=0; i<N; i++){
            for(int j=0; j<N; j++){
                ret += dp[i][j];
            }
        }
        return ret;
    }
};
```

####Coin Change

```c++
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        int max = amount + 1;
        vector<int> dp(max, max);
        dp[0] = 0;
        for(int i=1; i<=amount; i++){
            for(int j=0; j<coins.size(); j++){
                if(coins[j] <= i){
                    dp[i] = min(dp[i], dp[i-coins[j]] + 1);
                }
            }
        }
        return dp[amount] > amount ? -1 : dp[amount];
    }
};
```

#### Intersection of Two Arrays

```

```

