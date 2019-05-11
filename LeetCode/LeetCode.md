### [LeetCode](<https://leetcode-cn.com/problemset/all/>)

### 题目列表

| 题解连接                                                 | 原题连接                                                     |
| -------------------------------------------------------- | ------------------------------------------------------------ |
| [1.两数之和](#1两数之和)                                 | [1.两数之和](<https://leetcode-cn.com/problems/two-sum/>)    |
| [2.两数相加](#2两数相加)                                 | [2.两数相加](<https://leetcode-cn.com/problems/add-two-numbers/>) |
| [3.无重复字符的最长子串](#3无重复字符的最长子串)         | [3.无重复字符的最长子串](<https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/>) |
| [4.寻找两个有序数组的中位数](#4寻找两个有序数组的中位数) | [4.寻找两个有序数组的中位数](<https://leetcode-cn.com/problems/median-of-two-sorted-arrays/>) |
| [5.最长回文子串](#5最长回文子串)                         | [5.最长回文子串](<https://leetcode-cn.com/problems/longest-palindromic-substring/>) |

### 1.两数之和

#### 题目描述

给定一个整数数组 `nums` 和一个目标值 `target`，请你在该数组中找出和为目标值的那 **两个** 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

#### 示例

```
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

#### 题解

暴力法使用两层for循坏可以解决，时间复杂度O(n²)，空间复杂度O(1)。

本题解法使用一个HashMap，时间复杂度O(n)，空间复杂度O(n)。

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        // key 为值  value 为对应的数组下标
        HashMap<Integer,Integer> map = new HashMap();
        int key = 0;
        for(int i=0;i<nums.length;i++){
            key = target - nums[i];  // 需要找的值
            if (map.containsKey(key)){ // map中存在 返回
                return new int[]{map.get(key),i};
            } else {    // map中不存在 放入
                map.put(nums[i],i);
            }
        }
        return new int[2];
    }
}
```

### 2.两数相加

#### 题目描述

给出两个 **非空** 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 **逆序** 的方式存储的，并且它们的每个节点只能存储 **一位** 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

#### 示例

```
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```

#### 题解

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        if (l1==null && l2==null) return null;
        ListNode node = new ListNode(0);  // 最后结果要返回的链表
        ListNode cur = node;              // 用来遍历的节点
        int sum = 0,carry = 0;
        while (l1!=null || l2!=null){
            sum = 0;   // sum为每个位置上两个数的和 每次要清0
            if (l1!=null) {
                sum += l1.val;
                l1 = l1.next;
            } 
            if (l2!=null) {
                sum += l2.val;
                l2 = l2.next;
            } 
            sum += carry;    // 加上上一次进位的数
            carry = sum/10;  // 下一个新的进位
            sum %= 10;       // 求余得到个位数
            cur.next = new ListNode(sum); // 构造新节点
            cur = cur.next;  // 继续向后遍历
        }
        // 处理进位 两个个位数相加 进位不超过10 使用if即可
        if (carry!=0) cur.next = new ListNode(carry);
        return node.next;
    }
}
```

### 3.无重复字符的最长子串

#### 题目描述

给定一个字符串，请你找出其中不含有重复字符的 **最长子串** 的长度。

#### 示例1

```
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

#### 示例2

```
输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

#### 示例3

```
输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

#### 题解

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if (s==null || s.length()==0) return 0;
        // key 字符 value 字符对应的位置
        HashMap<Character,Integer> map = new HashMap();
        int res = 0,i = 0;  //  结果为 [i,j] 
        for(int j=0;j<s.length();j++){
            if (map.containsKey(s.charAt(j))){ // 当前字符已出现过
                // 原字符的位置 和 左边界比较 不能超过左边界
                i = Math.max(map.get(s.charAt(j))+1,i);
            }
            res = Math.max(res,j-i+1);
            // 不管当前字符有没有出现过 都要更新最新位置
            map.put(s.charAt(j),j);
        }
        return res;
    }
}
```

### 4.寻找两个有序数组的中位数

#### 题目描述

给定两个大小为 m 和 n 的有序数组 `nums1` 和 `nums2`。

请你找出这两个有序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。

你可以假设 `nums1` 和 `nums2` 不会同时为空。

#### 示例1

```
nums1 = [1, 3]
nums2 = [2]

则中位数是 2.0
```

#### 示例2

```
nums1 = [1, 2]
nums2 = [3, 4]

则中位数是 (2 + 3)/2 = 2.5
```

#### 题解

题目要求时间复杂度O(log(m+n))，能力有限，只给出O(n+m)解法，使用归并排序思想，将两个数组合并为一个。

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int n1 = nums1.length,n2 = nums2.length;
        int[] temp = new int[n1+n2];
        int i=0,j=0,k=0;
        while(i<n1 && j<n2){
            temp[k++] = nums1[i]<=nums2[j]?nums1[i++]:nums2[j++];
        }
        while(i<n1) temp[k++] = nums1[i++];
        while(j<n2) temp[k++] = nums2[j++];
        // k==n1+n2 如果是奇数个
        if((k&1)==1) return temp[k/2]+0.0;
        else return (temp[k/2]+temp[k/2-1])/2.0;
    }
}
```

### 5.最长回文子串

#### 题目描述

给定一个字符串 `s`，找到 `s` 中最长的回文子串。你可以假设 `s` 的最大长度为 1000。

#### 示例1

```
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
```

#### 示例2

```
输入: "cbbd"
输出: "bb"
```

#### 题解

```java
class Solution {
    public String longestPalindrome(String s) {
        if(s==null || s.length()==0) return "";
        int left=0,right=0;
        int len1 =0,len2=0,len=0;
        for(int i=0;i<s.length();i++){
            len1 = getLen(s,i,i);   // 匹配 aba 这种
            len2 = getLen(s,i,i+1); // 匹配 abba 这种
            len = Math.max(len1,len2);
            if (len>=right-left+1){  // 更新最大长度的左右边界
                left = i - (len-1)/2;
                right = i + len/2;
            }
        }
        return s.substring(left,right+1);
    }
    // 以 i j 为中心 向两边扩散匹配 得到长度
    public int getLen(String s,int i,int j){
        while(i>=0 && j<s.length() && s.charAt(i)==s.charAt(j)){
            i--;
            j++;
        }
        //  (j-i+1)-2
        return j-i-1;
    }
}
```

