# Digit counts

## Question

- leetcode: [Number of Digit One | Leetcode OJ](https://leetcode.com/problems/number-of-digit-one/)
- lintcode: [(3) Digit counts](http://www.lintcode.com/en/problem/digit-counts/)

```
count the number of k's between 0 and n. k can be 0 - 9.

Example
if n=12, k=1 in [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12],
we have FIVE 1's (1, 10, 11, 12)
```

## 题解

leetcode 上的有点简单，这里以 Lintcode 上的为例进行说明。找出从0至整数 n 中出现数位k的个数，与整数有关的题大家可能比较容易想到求模求余等方法，但其实很多与整数有关的题使用字符串的解法更为便利。将整数 i 分解为字符串，然后遍历之，自增 k 出现的次数即可。

### c
```c
class Solution {
public:
    /*
     * param k : As description.
     * param n : As description.
     * return: How many k's between 0 and n.
     */
    int digitcounts(int k, int n) {
        char c = k + '0';
        int count = 0;
        for (int i = k; i <= n; i++) {
            for (auto s : to_string(i)) {
                if (s == c) count++;
            }
        }
        return count;
    }
};
```

### Java

```java
class Solution {
    /*
     * param k : As description.
     * param n : As description.
     * return: An integer denote the count of digit k in 1..n
     */
    public int digitcounts(int k, int n) {
        int count = 0;
        char kchar = (char)(k + '0');
        for (int i = k; i <= n; i++) {
            char[] ichars = Integer.toString(i).tocharArray();
            for (char ichar : ichars) {
                if (kchar == ichar) count++;
            }
        }

        return count;
    }
}
```

### 源码分析

太简单了，略

### 复杂度分析

时间复杂度 $$O(n \times L)$$, L 为n 的最大长度，拆成字符数组，空间复杂度 $$O(L)$$.
