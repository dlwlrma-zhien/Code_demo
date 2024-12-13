## 反转字符串

### 题目

地址：https://leetcode.cn/problems/reverse-string/description/

编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 char[] 的形式给出。

不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。

你可以假设数组中的所有字符都是 ASCII 码表中的可打印字符。

示例 1：
输入：["h","e","l","l","o"]
输出：["o","l","l","e","h"]

示例 2：
输入：["H","a","n","n","a","h"]
输出：["h","a","n","n","a","H"]

### 代码

~~~java
class Solution {
    public void reverseString(char[] s) {
        //双指针
        int left = 0;
        int right = s.length - 1;
        while(left < right){
            char temp = s[left];
            s[left] = s[right];
            s[right] = temp;
            left++;
            right--;
        }
    }
}
~~~

## 反转字符串II

### 题目

地址：https://leetcode.cn/problems/reverse-string-ii/description/

给定一个字符串 s 和一个整数 k，从字符串开头算起, 每计数至 2k 个字符，就反转这 2k 个字符中的前 k 个字符。

如果剩余字符少于 k 个，则将剩余字符全部反转。

如果剩余字符小于 2k 但大于或等于 k 个，则反转前 k 个字符，其余字符保持原样。

示例:

输入: s = "abcdefg", k = 2
输出: "bacdfeg"

### 代码

~~~java
class Solution {
    public String reverseStr(String s, int k) {
        //先将字符串转换为字符数组
        char[] arr = s.toCharArray();
        for(int i=0;i<arr.length;i+=2*k){
            int start = i;
            //判断尾部情况
            int end = Math.min(arr.length-1,i+k-1);
            while(start < end){
                char temp = arr[start];
                arr[start] = arr[end];
                arr[end] = temp;
                start++;
                end--;
            }
        }
        return new String(arr);
    }
}
~~~

