## 有效的字母异位词

### 题目

地址：https://leetcode.cn/problems/valid-anagram/

给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。

示例 1: 输入: s = "anagram", t = "nagaram" 输出: true

示例 2: 输入: s = "rat", t = "car" 输出: false

**说明:** 你可以假设字符串只包含小写字母。

思路

需要定义一个多大的数组呢，定一个数组叫做record，大小为26 就可以了，初始化为0，因为字符a到字符z的ASCII也是26个连续的数值。

为了方便举例，判断一下字符串s= "aee", t = "eae"。

操作动画如下：

![242.有效的字母异位词](https://code-thinking.cdn.bcebos.com/gifs/242.%E6%9C%89%E6%95%88%E7%9A%84%E5%AD%97%E6%AF%8D%E5%BC%82%E4%BD%8D%E8%AF%8D.gif)

定义一个数组叫做record用来上记录字符串s里字符出现的次数。

需要把字符映射到数组也就是哈希表的索引下标上，**因为字符a到字符z的ASCII是26个连续的数值，所以字符a映射为下标0，相应的字符z映射为下标25。**

再遍历 字符串s的时候，**只需要将 s[i] - ‘a’ 所在的元素做+1 操作即可，并不需要记住字符a的ASCII，只要求出一个相对数值就可以了。** 这样就将字符串s中字符出现的次数，统计出来了。

那看一下如何检查字符串t中是否出现了这些字符，同样在遍历字符串t的时候，对t中出现的字符映射哈希表索引上的数值再做-1的操作。

那么最后检查一下，**record数组如果有的元素不为零0，说明字符串s和t一定是谁多了字符或者谁少了字符，return false。**

最后如果record数组所有元素都为零0，说明字符串s和t是字母异位词，return true。

时间复杂度为O(n)，空间上因为定义是的一个常量大小的辅助数组，所以空间复杂度为O(1)。

### 代码

~~~java
class Solution {
    public boolean isAnagram(String s, String t) {
        //使用数组来解决
        int[] record = new int[26];

        for(int i=0;i<s.length();i++){
            record[s.charAt(i) - 'a']++;
        }

        for(int i=0;i<t.length();i++){
            record[t.charAt(i) - 'a']--;
        }

        //遍历数组
        for(int count : record){
            if(count != 0){
                return false;
            }
        }
        return true;
    }
}
~~~

## 两个数组的交集

### 题目

地址：https://leetcode.cn/problems/intersection-of-two-arrays/

题意：给定两个数组，编写一个函数来计算它们的交集。

![349. 两个数组的交集](https://code-thinking-1253855093.file.myqcloud.com/pics/20200818193523911.png)

**说明：** 输出结果中的每个元素一定是唯一的。 我们可以不考虑输出结果的顺序。

题目说到唯一就应该想到用Set来去重。

### 代码

~~~java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        if(nums1 == null || nums1.length == 0 ||nums2 == null || nums2.length == 0){
            return new int[0];
        }

        Set<Integer> set1 = new HashSet<>();
        Set<Integer> set2 = new HashSet<>();

        //遍历nums1
        for(int i : nums1){
            set1.add(i);
        }

        //遍历nums2
        for(int i : nums2){
            if(set1.contains(i)){
                set2.add(i);
            }
        }

        //重新建立一个数组，存放元素
        int[] nums3 = new int[set2.size()];
        int j = 0;
        for(int i : set2){
            nums3[j++] = i;
        }
        return nums3;
    }
}
~~~

## 快乐数

### 题目

地址：https://leetcode.cn/problems/happy-number/description/

编写一个算法来判断一个数 n 是不是快乐数。

「快乐数」定义为：对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和，然后重复这个过程直到这个数变为 1，也可能是 无限循环 但始终变不到 1。如果 可以变为  1，那么这个数就是快乐数。

如果 n 是快乐数就返回 True ；不是，则返回 False 。



### 代码

~~~java
class Solution {
    public boolean isHappy(int n) {
        Set<Integer> set = new HashSet<>();
        while(n !=1 && !set.contains(n)){
            set.add(n);
            n = getN(n);
        }
        return n==1;
    }

    private int getN(int n){
        int res = 0;
        while(n > 0){
            int temp = n % 10;
            res += temp * temp;
            n = n / 10;
        }
        return res;
    }
}
~~~

## 四数相加II

### 题目

地址：https://leetcode.cn/problems/4sum-ii/description/

给定四个包含整数的数组列表 A , B , C , D ,计算有多少个元组 (i, j, k, l) ，使得 A[i] + B[j] + C[k] + D[l] = 0。

为了使问题简单化，所有的 A, B, C, D 具有相同的长度 N，且 0 ≤ N ≤ 500 。所有整数的范围在 -2^28 到 2^28 - 1 之间，最终结果不会超过 2^31 - 1 。

**例如:**

输入:

- A = [ 1, 2]
- B = [-2,-1]
- C = [-1, 2]
- D = [ 0, 2]

输出:

2

**解释:**

两个元组如下:

1. (0, 0, 0, 1) -> A[0] + B[0] + C[0] + D[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> A[1] + B[1] + C[0] + D[0] = 2 + (-1) + (-1) + 0 = 0



### 代码

~~~java
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
         int res = 0;
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        //统计两个数组中的元素之和，同时统计出现的次数，放入map
        for (int i : nums1) {
            for (int j : nums2) {
                int sum = i + j;
                map.put(sum, map.getOrDefault(sum, 0) + 1);
            }
        }
        //统计剩余的两个元素的和，在map中找是否存在相加为0的情况，同时记录次数
        for (int i : nums3) {
            for (int j : nums4) {
                res += map.getOrDefault(0 - i - j, 0);
            }
        }
        return res;
    }
}
~~~

## 赎金信

### 题目

地址：https://leetcode.cn/problems/ransom-note/description/

给定一个赎金信 (ransom) 字符串和一个杂志(magazine)字符串，判断第一个字符串 ransom 能不能由第二个字符串 magazines 里面的字符构成。如果可以构成，返回 true ；否则返回 false。

(题目说明：为了不暴露赎金信字迹，要从杂志上搜索各个需要的字母，组成单词来表达意思。杂志字符串中的每个字符只能在赎金信字符串中使用一次。)

因为题目说只有小写字母，那可以采用空间换取时间的哈希策略，用一个长度为26的数组来记录magazine里字母出现的次数。

然后再用ransomNote去验证这个数组是否包含了ransomNote所需要的所有字母。

依然是数组在哈希法中的应用。

### 代码

~~~java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        if(ransomNote.length() > magazine.length()){
            return false;
        }

        int[] arr = new int[26];

        for(char i : magazine.toCharArray()){
            arr[i - 'a'] += 1;
        }

        for(char i : ransomNote.toCharArray()){
            arr[i - 'a'] -= 1;
        }

        for(int i : arr){
            if(i<0){
                return false;
            }
        }
        return true;
    }
}
~~~

## 三数之和

### 题目

地址：https://leetcode.cn/problems/3sum/description/

给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有满足条件且不重复的三元组。

**注意：** 答案中不可以包含重复的三元组。

示例：

给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为： [ [-1, 0, 1], [-1, -1, 2] ]

### 代码

~~~java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums);

        for(int i=0;i<nums.length;i++){
            if(nums[i] > 0){
                return result;
            }

            //对a去重
            if(i>0 && nums[i] == nums[i-1]){
                continue;
            }

            int left = i+1;
            int right = nums.length - 1;
            while(left < right){
                int sum = nums[i] + nums[left] + nums[right];
                if(sum > 0){
                    right--;
                }else if(sum < 0){
                    left++;
                }else{
                    result.add(Arrays.asList(nums[i],nums[left],nums[right]));
                    //对b,c去重
                    while(left < right && nums[right] == nums[right-1]){
                        right--;
                    }
                    while(left < right && nums[left] == nums[left+1]){
                        left++;
                    }

                    left++;
                    right--;
                }
            }
        }
        return result;
    }
}
~~~

