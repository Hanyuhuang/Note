### [LeetCode](<https://leetcode-cn.com/problemset/all/>)

### 题目列表

| 题解连接                                                 | 原题连接                                                     |
| -------------------------------------------------------- | ------------------------------------------------------------ |
| [1.两数之和](#1两数之和)                                 | [1.两数之和](<https://leetcode-cn.com/problems/two-sum/>)    |
| [2.两数相加](#2两数相加)                                 | [2.两数相加](<https://leetcode-cn.com/problems/add-two-numbers/>) |
| [3.无重复字符的最长子串](#3无重复字符的最长子串)         | [3.无重复字符的最长子串](<https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/>) |
| [4.寻找两个有序数组的中位数](#4寻找两个有序数组的中位数) | [4.寻找两个有序数组的中位数](<https://leetcode-cn.com/problems/median-of-two-sorted-arrays/>) |
| [5.最长回文子串](#5最长回文子串)                         | [5.最长回文子串](<https://leetcode-cn.com/problems/longest-palindromic-substring/>) |
| [6.Z字形变换](#6z字形变换)                               | [6.Z字形变换](<https://leetcode-cn.com/problems/zigzag-conversion/>) |
| [7.整数反转](#7整数反转)                                 | [7.整数反转](<https://leetcode-cn.com/problems/reverse-integer/>) |
| [8.字符串转换整数(atoi)](#8字符串转换整数atoi)           | [8.字符串转换整数(atoi)](<https://leetcode-cn.com/problems/string-to-integer-atoi/>) |
| [9.回文数](#9回文数)                                     | [9.回文数](<https://leetcode-cn.com/problems/palindrome-number/>) |
| [10.正则表达式匹配](#10正则表达式匹配)                   | [10.正则表达式匹配](<https://leetcode-cn.com/problems/regular-expression-matching/>) |

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

### 6.Z字形变换

#### 题目描述

将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 `"LEETCODEISHIRING"` 行数为 3 时，排列如下：

```
L   C   I   R
E T O E S I I G
E   D   H   N
```

之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如：`"LCIRETOESIIGEDHN"`。

请你实现这个将字符串进行指定行数变换的函数：

```
string convert(string s, int numRows);
```

#### 示例1

```
输入: s = "LEETCODEISHIRING", numRows = 3
输出: "LCIRETOESIIGEDHN"
```

#### 示例2

```
输入: s = "LEETCODEISHIRING", numRows = 4
输出: "LDREOEIIECIHNTSG"
解释:

L     D     R
E   O E   I I
E C   I H   N
T     S     G
```

#### 题解

```java
class Solution {
    public String convert(String s, int numRows) {
        if(numRows == 1) return s;
        // list中每一个StringBuilder 代表每一行的值
        List<StringBuilder> list = new ArrayList();
        for(int i=0;i<numRows;i++) list.add(new StringBuilder());
        int row = 0;         // 当前行
        boolean isUp = false;// 控制移动方向
        for(int i=0;i<s.length();i++){
            // 将当前字符追加到当前行的值上
            list.get(row).append(s.charAt(i));
            // 当到第一行和最后一行时 要改变方向
            if(row==0 || row==numRows-1) isUp = !isUp;
            // 控制 向上还是向下
            row += isUp? 1 : -1;
        }
        // 将每一行的结果拼接起来
        StringBuilder sb = new StringBuilder();
        for(StringBuilder string : list) sb.append(string);
        return sb.toString();
    }
}
```

### 7.整数反转

#### 题目描述

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

#### 示例1

```
输入: 123
输出: 321
```

#### 示例2

```
输入: -123
输出: -321
```

#### 示例3

```
输入: 120
输出: 21
```

#### 注意

假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−2^31,  2^31 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。

#### 题解

```java
class Solution {
    public int reverse(int x) {
        // 使用字符串将数字反转
        StringBuilder sb = new StringBuilder();
        String s = x+"";
        boolean isNav = false; // 是否是负数
        if(s.charAt(0)=='-') isNav = true;
        for(int i=isNav?1:0;i<s.length();i++){
            sb.append(s.charAt(i));
        }
        if(isNav) sb.append('-');
        sb.reverse();
        // 转为long类型 比较越界
        long res = new Long(sb.toString());
        if(res>Integer.MAX_VALUE || res<Integer.MIN_VALUE) return 0;
        else return (int)res;
    }
}
```

### 8.字符串转换整数(atoi)

#### 题目描述

请你来实现一个 `atoi` 函数，使其能将字符串转换成整数。

首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。

当我们寻找到的第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字组合起来，作为该整数的正负号；假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。

该字符串除了有效的整数部分之后也可能会存在多余的字符，这些字符可以被忽略，它们对于函数不应该造成影响。

注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换。

在任何情况下，若函数不能进行有效的转换时，请返回 0。

**说明：**

假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−231,  231 − 1]。如果数值超过这个范围，qing返回  INT_MAX (231 − 1) 或 INT_MIN (−231) 。

#### 示例1

```
输入: "42"
输出: 42
```

####示例2

```
输入: "   -42"
输出: -42
解释: 第一个非空白字符为 '-', 它是一个负号。
     我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。
```

#### 示例3

```
输入: "4193 with words"
输出: 4193
解释: 转换截止于数字 '3' ，因为它的下一个字符不为数字。
```

####示例4

```
输入: "words and 987"
输出: 0
解释: 第一个非空字符是 'w', 但它不是数字或正、负号。
     因此无法执行有效的转换。
```

####示例5

```
输入: "-91283472332"
输出: -2147483648
解释: 数字 "-91283472332" 超过 32 位有符号整数范围。 
     因此返回 INT_MIN (−231) 。
```

#### 题解

```java
class Solution {
    public int myAtoi(String str) {
        str = str.trim();
        if(str.equals("") || str.length()==0) return 0;
        StringBuilder sb = new StringBuilder();
        // 第一个字符合法
        if((str.charAt(0)>='0'&&str.charAt(0)<='9')
           || str.charAt(0)=='+'||str.charAt(0)=='-') sb.append(str.charAt(0));
        // 第一个字符不合法
        else return 0;
        // 处理剩余
        for(int i=1;i<str.length();i++){
            if(str.charAt(i)>='0'&&str.charAt(i)<='9') sb.append(str.charAt(i));
            else break;
        }
        //  单个正负号的情况
        if(sb.length()==1&&(sb.charAt(0)=='-' || sb.charAt(0)=='+')) return 0;
        try{
            return new Integer(sb.toString());
        }catch(Exception e){
            if(sb.charAt(0)=='-') return Integer.MIN_VALUE;
            else return Integer.MAX_VALUE;
        }
    }
}
```

### 9.回文数

#### 题目描述

判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

#### 示例1

```
输入: 121
输出: true
```

#### 示例2

```
输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
```

#### 示例3

```
输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。
```

#### 进阶

你能不将整数转为字符串来解决这个问题吗？

#### 题解

```java
class Solution {
    public boolean isPalindrome(int x) {
        if(x<0) return false;
        StringBuilder sb = new StringBuilder(x+"");
        return (sb.toString().equals(sb.reverse().toString()));
    }
}
```

### 10.正则表达式匹配

#### 题目描述

给定一个字符串 (`s`) 和一个字符模式 (`p`)。实现支持 `'.'` 和 `'*'` 的正则表达式匹配。

```
'.' 匹配任意单个字符。
'*' 匹配零个或多个前面的元素。
```

匹配应该覆盖**整个**字符串 (`s`) ，而不是部分字符串。

**说明:**

- `s` 可能为空，且只包含从 `a-z` 的小写字母。
- `p` 可能为空，且只包含从 `a-z` 的小写字母，以及字符 `.` 和 `*`。

#### 示例1

```
输入:
s = "aa"
p = "a"
输出: false
解释: "a" 无法匹配 "aa" 整个字符串。
```

#### 示例2

```
输入:
s = "aa"
p = "a*"
输出: true
解释: '*' 代表可匹配零个或多个前面的元素, 即可以匹配 'a' 。因此, 重复 'a' 一次, 字符串可变为 "aa"。
```

#### 示例3

```
输入:
s = "ab"
p = ".*"
输出: true
解释: ".*" 表示可匹配零个或多个('*')任意字符('.')。
```

#### 示例4

```
输入:
s = "aab"
p = "c*a*b"
输出: true
解释: 'c' 可以不被重复, 'a' 可以被重复一次。因此可以匹配字符串 "aab"。
```

#### 示例5

```
输入:
s = "mississippi"
p = "mis*is*p*."
输出: false
```

#### 题解

```java
class Solution {
    public boolean isMatch(String s, String p) {
        if(s==null || p==null) return false;
        return dfs(s.toCharArray(),p.toCharArray(),0,0);
    }
    public boolean dfs(char[] s,char[] p,int i,int j){
        // 两个同时匹配结束 才结束
        if(j==p.length) return i==s.length;
        // 当前字符 是否匹配
        boolean match = i<s.length && (s[i]==p[j] || p[j]=='.');
        // 后一个字符为*
        if(j+1<p.length && p[j+1]=='*'){
           //'||'前面的判断表示为：后面的*表示当前字符出现0次的情况 继续从*后面的字符匹配
           //'||'后面的判断表示为：后面的*表示当前字符出现的情况 并且需要匹配当前字符
            return dfs(s,p,i,j+2) || (match && dfs(s,p,i+1,j)) ;
        }else{
            return match && dfs(s,p,i+1,j+1);
        }
    }
}
```

