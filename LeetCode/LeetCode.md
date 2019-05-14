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
| [11.盛最多水的容器](#11盛最多水的容器)                   | [11.盛最多水的容器](<https://leetcode-cn.com/problems/container-with-most-water/>) |
| [12.整数转罗马数字](#12整数转罗马数字)                   | [12.整数转罗马数字](https://leetcode-cn.com/problems/integer-to-roman/) |
| [13.罗马数字转整数](#13罗马数字转整数)                   | [13.罗马数字转整数](https://leetcode-cn.com/problems/roman-to-integer/) |
| [14.最长公共前缀](#14最长公共前缀)                       | [14.最长公共前缀](https://leetcode-cn.com/problems/longest-common-prefix/) |
| [15.三数之和](#15三数之和)                               | [15.三数之和](https://leetcode-cn.com/problems/3sum/)        |
| [16.最接近的三数之和](#16最接近的三数之和)               | [16.最接近的三数之和](https://leetcode-cn.com/problems/3sum-closest/) |
| [17.电话号码的字母组合](#17电话号码的字母组合)           | [17.电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/) |
| [18.四数之和](#18四数之和)                               | [18.四数之和](https://leetcode-cn.com/problems/4sum/)        |
| [19.删除链表的倒数第N个节点](#19删除链表的倒数第n个节点) | [19.删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/) |
| [20.有效的括号](#20有效的括号)                           | [20.有效的括号](https://leetcode-cn.com/problems/valid-parentheses/) |
### 1.两数之和

#### 题目描述

给定一个整数数组 `nums` 和一个目标值 `target`，请你在该数组中找出和为目标值的那 **两个** 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

**示例**

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

**示例**

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

**示例1**

```
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

**示例2**

```
输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

**示例3**

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

**示例1**

```
nums1 = [1, 3]
nums2 = [2]

则中位数是 2.0
```

**示例2**

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

**示例1**

```
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
```

**示例2**

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

**示例1**

```
输入: s = "LEETCODEISHIRING", numRows = 3
输出: "LCIRETOESIIGEDHN"
```

**示例2**

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

**示例1**

```
输入: 123
输出: 321
```

**示例2**

```
输入: -123
输出: -321
```

**示例3**

```
输入: 120
输出: 21
```

**注意**

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

**示例1**

```
输入: "42"
输出: 42
```

**示例2**

```
输入: "   -42"
输出: -42
解释: 第一个非空白字符为 '-', 它是一个负号。
     我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。
```

**示例3**

```
输入: "4193 with words"
输出: 4193
解释: 转换截止于数字 '3' ，因为它的下一个字符不为数字。
```

**示例4**

```
输入: "words and 987"
输出: 0
解释: 第一个非空字符是 'w', 但它不是数字或正、负号。
     因此无法执行有效的转换。
```

**示例5**

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

**示例1**

```
输入: 121
输出: true
```

**示例2**

```
输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
```

**示例3**

```
输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。
```

**进阶**

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

**示例1**

```
输入:
s = "aa"
p = "a"
输出: false
解释: "a" 无法匹配 "aa" 整个字符串。
```

**示例2**

```
输入:
s = "aa"
p = "a*"
输出: true
解释: '*' 代表可匹配零个或多个前面的元素, 即可以匹配 'a' 。因此, 重复 'a' 一次, 字符串可变为 "aa"。
```

**示例3**

```
输入:
s = "ab"
p = ".*"
输出: true
解释: ".*" 表示可匹配零个或多个('*')任意字符('.')。
```

**示例4**

```
输入:
s = "aab"
p = "c*a*b"
输出: true
解释: 'c' 可以不被重复, 'a' 可以被重复一次。因此可以匹配字符串 "aab"。
```

**示例5**

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

### 11.盛最多水的容器

#### 题目描述

给定 *n* 个非负整数 *a*1，*a*2，...，*a*n，每个数代表坐标中的一个点 (*i*, *ai*) 。在坐标内画 *n* 条垂直线，垂直线 *i* 的两个端点分别为 (*i*, *ai*) 和 (*i*, 0)。找出其中的两条线，使得它们与 *x* 轴共同构成的容器可以容纳最多的水。

**说明:** 你不能倾斜容器，且 *n* 的值至少为 2。

![](images/question_11.jpg)

**示例:**

```
输入: [1,8,6,2,5,4,8,3,7]
输出: 49
```

#### 题解

```java
class Solution {
    public int maxArea(int[] height) {
        int i=0,j=height.length-1;
        int res = 0,cur=0;
        while(i<j){  // 从两边向中间移动
            // 当前面积
            cur = Math.min(height[i],height[j])*(j-i);
            // 当前面积和最大面积比较
            res = Math.max(cur,res);
            // 两边高度较低的向中间移动
            if(height[i]<=height[j]) i++;
            else j--;
        }
        return res;
    }
}
```

### 12.整数转罗马数字

#### 题目描述

罗马数字包含以下七种字符： `I`， `V`， `X`， `L`，`C`，`D` 和 `M`。

```
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

例如， 罗马数字 2 写做 `II` ，即为两个并列的 1。12 写做 `XII` ，即为 `X` + `II` 。 27 写做  `XXVII`, 即为 `XX` + `V` + `II` 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 `IIII`，而是 `IV`。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 `IX`。这个特殊的规则只适用于以下六种情况：

- `I` 可以放在 `V` (5) 和 `X` (10) 的左边，来表示 4 和 9。
- `X` 可以放在 `L` (50) 和 `C` (100) 的左边，来表示 40 和 90。 
- `C` 可以放在 `D` (500) 和 `M` (1000) 的左边，来表示 400 和 900。

给定一个整数，将其转为罗马数字。输入确保在 1 到 3999 的范围内。

**示例 1:**

```
输入: 3
输出: "III"
```

**示例 2:**

```
输入: 4
输出: "IV"
```

**示例 3:**

```
输入: 9
输出: "IX"
```

**示例 4:**

```
输入: 58
输出: "LVIII"
解释: L = 50, V = 5, III = 3.
```

**示例 5:**

```
输入: 1994
输出: "MCMXCIV"
解释: M = 1000, CM = 900, XC = 90, IV = 4.
```

#### 题解

```java
class Solution {
    public String intToRoman(int num) {
        // 定义字典
        int values[]={1000,900,500,400,100,90,50,40,10,9,5,4,1};
        String[] strs={"M","CM","D","CD","C","XC","L","XL","X","IX","V","IV","I"};
        StringBuilder sb = new StringBuilder();
        // 从大数开始寻找
        for(int i=0; i< strs.length; i++){
            // 当前数 大于 字典数
            while(num >= values[i]){
                num -= values[i];
                sb.append(strs[i]);
            }
        }
        return sb.toString();
    }
}
```

### 13.罗马数字转整数

#### 题目描述

罗马数字包含以下七种字符: `I`， `V`， `X`， `L`，`C`，`D` 和 `M`。

```
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

例如， 罗马数字 2 写做 `II` ，即为两个并列的 1。12 写做 `XII` ，即为 `X` + `II` 。 27 写做  `XXVII`, 即为 `XX` + `V` + `II` 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 `IIII`，而是 `IV`。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 `IX`。这个特殊的规则只适用于以下六种情况：

- `I` 可以放在 `V` (5) 和 `X` (10) 的左边，来表示 4 和 9。
- `X` 可以放在 `L` (50) 和 `C` (100) 的左边，来表示 40 和 90。 
- `C` 可以放在 `D` (500) 和 `M` (1000) 的左边，来表示 400 和 900。

给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。

**示例 1:**

```
输入: "III"
输出: 3
```

**示例 2:**

```
输入: "IV"
输出: 4
```

**示例 3:**

```
输入: "IX"
输出: 9
```

**示例 4:**

```
输入: "LVIII"
输出: 58
解释: L = 50, V= 5, III = 3.
```

**示例 5:**

```
输入: "MCMXCIV"
输出: 1994
解释: M = 1000, CM = 900, XC = 90, IV = 4.
```

#### 题解

```java
class Solution {
    public int romanToInt(String s) {
        // 定义字典
        int values[]={1000,900,500,400,100,90,50,40,10,9,5,4,1};
        String[] strs={"M","CM","D","CD","C","XC","L","XL","X","IX","V","IV","I"};
        // res:结果 i:字典位置 j:字符串的位置
        int res = 0,i=0,j=0;
        // 遍历整个字符串
        while(j<s.length()){
            // 当前字符和字典匹配 从j开始以strs[i]开头
            if(s.startsWith(strs[i],j)) {
                // 结果加上对应值
                res += values[i];
                // 字符串移动 字典值的长度
                j += strs[i].length();
            }
            // 不匹配 移动字典位置
            else i++;
        }
        return res;
    }
}
```

### 14.最长公共前缀

#### 题目描述

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 `""`。

**示例 1:**

```
输入: ["flower","flow","flight"]
输出: "fl"
```

**示例 2:**

```
输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。
```

**说明:**

所有输入只包含小写字母 `a-z` 。

#### 题解

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {  
        if(strs==null || strs.length==0) return "";
        if(strs.length==1) return strs[0];
        // 以第一个字符串作为比较对象
        char[] model = strs[0].toCharArray();
        StringBuilder sb = new StringBuilder("");
        // 比较所有字符串的每一位
        for(int i=0;i<model.length;i++){
            // 比较每个字符串的第i位
            for(int j=1;j<strs.length;j++){
                // 超出字符串长度 或者 不匹配 直接返回
                if(i>=strs[j].length() || strs[j].charAt(i)!=model[i]) 
                    return sb.toString();
            }
            // 每个字符串的第i位都相等
            sb.append(model[i]);
        }
        return sb.toString();
    }
}
```

### 15.三数之和

#### 题目描述

给定一个包含 *n* 个整数的数组 `nums`，判断 `nums` 中是否存在三个元素 *a，b，c* ，使得 *a + b + c =* 0 ？找出所有满足条件且不重复的三元组。

**注意:** 答案中不可以包含重复的三元组。

```
例如, 给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

#### 题解

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> list = new ArrayList();
        Arrays.sort(nums);  // 排序从两边找
        int i=0,j=0;
        // 每次k不动 找其他两个数
        for(int k=0;k<nums.length-2;k++){
            // 去重
            if(k>0 && nums[k]==nums[k-1]) continue;
            i = k+1;
            j = nums.length-1;
            while(i<j){
                // 值不等 做相应的处理
                if (nums[i]+nums[j]+nums[k]>0) j--;
                else if(nums[i]+nums[j]+nums[k]<0) i++;
                else {
                    list.add(Arrays.asList(nums[k],nums[i],nums[j]));
                    // 去除重复值
                    while(i<j&&nums[i]==nums[i+1]) i++;
                    while(i<j&&nums[j]==nums[j-1]) j--;
                    // 继续寻找
                    i++;
                    j--;
                }
            }
        }
        return list;
    }
}
```

### 16.最接近的三数之和

#### 题目描述

给定一个包括 *n* 个整数的数组 `nums` 和 一个目标值 `target`。找出 `nums` 中的三个整数，使得它们的和与 `target` 最接近。返回这三个数的和。假定每组输入只存在唯一答案。

```
例如，给定数组 nums = [-1，2，1，-4], 和 target = 1.

与 target 最接近的三个数的和为 2. (-1 + 2 + 1 = 2).
```

#### 题解

```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        int i=0,j=0,cur=0;
        int res = nums[0]+nums[1]+nums[2];
        Arrays.sort(nums); // 排序
        for(int k=0;k<nums.length-2;k++){
            i = k+1;
            j = nums.length-1;
            while(i<j){
                // 当前三数之和
                cur = nums[k]+nums[i]+nums[j];
                // 当前三数之和如果比最接近的还接近 更新
                if(Math.abs(cur-target)<Math.abs(res-target)) res = cur;
                if(cur>target) j--;
                else if(cur<target) i++;
                else return target;
            }
        }
        return res;
    }
}
```

### 17.电话号码的字母组合

#### 题目描述

给定一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

![](images/question_17.png)

**示例:**

```
输入："23"
输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

**说明:**
尽管上面的答案是按字典序排列的，但是你可以任意选择答案输出的顺序。

#### 题解

```java
class Solution {
    Map<Character,String> map = new HashMap();
    {
        map.put('2',"abc");
        map.put('3',"def");
        map.put('4',"ghi");
        map.put('5',"jkl");
        map.put('6',"mno");
        map.put('7',"pqrs");
        map.put('8',"tuv");
        map.put('9',"wxyz");
    }
    public List<String> letterCombinations(String digits) {
        List<String> list = new ArrayList();
        if(digits==null || digits.length()==0) return list;
        dfs(list,digits,"",0);
        return list;
    }
    
    public void dfs(List<String> list,String digits,String str,int index){
        // 遍历结束
        if(index==digits.length()){
            list.add(new String(str));
            return;
        }
        // 给定字符串 第index位上的字符
        char key = digits.charAt(index);
        // 遍历这个字符对应的字符串
        for(int i=0;i<map.get(key).length();i++){
            dfs(list,digits,str+map.get(key).charAt(i),index+1);
        }
    }
}
```

### 18.四数之和

#### 题目描述

给定一个包含 *n* 个整数的数组 `nums` 和一个目标值 `target`，判断 `nums` 中是否存在四个元素 *a，**b，c* 和 *d* ，使得 *a* + *b* + *c* + *d* 的值与 `target` 相等？找出所有满足条件且不重复的四元组。

**注意：**

答案中不可以包含重复的四元组。

**示例：**

```
给定数组 nums = [1, 0, -1, 0, -2, 2]，和 target = 0。

满足要求的四元组集合为：
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```

#### 题解

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> list = new ArrayList();
        if(nums==null || nums.length<4) return list;
        int k=0,t=0,cur=0;
        Arrays.sort(nums);
        // nums[i] 不变 其他三个值变
        for(int i=0;i<nums.length-3;i++){
            // 去重
            if(i>0 && nums[i]==nums[i-1]) continue;
            // nums[j]不变 其他两个值变
            for(int j=i+1;j<nums.length-2;j++){
                // 去重
                if(j>i+1 && nums[j]==nums[j-1]) continue;
                k = j+1;
                t = nums.length-1;
                while(k<t){
                    // 当前四数之和
                    cur = nums[i]+nums[j]+nums[k]+nums[t];
                    if(cur>target) t--;
                    else if(cur<target) k++;
                    else {
                        list.add(Arrays.asList(nums[i],nums[j],nums[k],nums[t]));
                        // 去重
                        while(k<t && nums[k]==nums[k+1]) k++;
                        while(k<t && nums[t]==nums[t-1]) t--;
                        k++;
                        t--;
                    }
                }
            }
        }
        return list;
    }
}
```

### 19.删除链表的倒数第N个节点

#### 题目描述

给定一个链表，删除链表的倒数第 *n* 个节点，并且返回链表的头结点。

**示例：**

```
给定一个链表: 1->2->3->4->5, 和 n = 2.

当删除了倒数第二个节点后，链表变为 1->2->3->5.
```

**说明：**

给定的 *n* 保证是有效的。

**进阶：**

你能尝试使用一趟扫描实现吗？

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
    public ListNode removeNthFromEnd(ListNode head, int n) {
        if(head==null) return null;
        ListNode slow = head,fast=head.next,pre=null;
        // 快指针先走n-1步
        for(int i=0;i<n-1;i++){
            fast = fast.next;
        }
        // 两指针同时走 并记录慢指针的上一个
        while(fast!=null){
            pre = slow;    // 保存前一个
            slow = slow.next;
            fast = fast.next;
        }
        // 删除的节点为头结点
        if(pre==null) return slow==null?null:slow.next;
        pre.next = slow.next;
        return head;
    }
}
```

### 20.有效的括号

#### 题目描述

给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。

注意空字符串可被认为是有效字符串。

**示例 1:**

```
输入: "()"
输出: true
```

**示例 2:**

```
输入: "()[]{}"
输出: true
```

**示例 3:**

```
输入: "(]"
输出: false
```

**示例 4:**

```
输入: "([)]"
输出: false
```

**示例 5:**

```
输入: "{[]}"
输出: true
```

#### 题解

```java
class Solution {
    Map<Character,Character> map = new HashMap();
    {
        map.put(')','(');
        map.put('}','{');
        map.put(']','[');
    }
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack();
        // 遍历字符串
        for(char c : s.toCharArray()){
            // 三种左括号直接进栈
            if(c=='(' || c=='{' || c=='[') stack.push(c);
            // 三种右括号 匹配栈顶元素
            else {
                // 空栈遇到右括号 直接返回false
                if(stack.isEmpty()) return false;
                // 匹配成功 弹出栈顶元素
                else if(map.get(c)==stack.peek()) stack.pop();
                // 匹配失败 加入队列
                else stack.push(c);
            }
        }
        return stack.isEmpty();
    }
}
```

