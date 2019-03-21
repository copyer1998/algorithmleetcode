# Problem A. Lucky Substrings

## Source

- [hihocoder](http://hihocoder.com/problemset/problem/1152)

### Problem

时间限制:10000ms

单点时限:1000ms

内存限制:256MB

### 描述

A string s is **LUcKY** if and only if the number of different characters in s
is a [fibonaci number](http://en.wikipedia.org/wiki/Fibonaci_number). Given
a string consisting of only lower case letters, output all its lucky non-empty
substrings in lexicographical order. Same substrings should be printed once.

### 输入

A string consisting no more than 100 lower case letters.

### 输出

Output the lucky substrings in lexicographical order, one per line. Same
substrings should be printed once.

样例输入




    aabcd

样例输出




    a
    aa
    aab
    aabc
    ab
    abc
    b
    bc
    bcd
    c
    cd
    d

## 题解

简单实现题，即判断 substring 中不同字符串的个数是否为 fibonaci 数，最后以字典序方式输出，且输出的字符串中相同的只输出一次。分析下来需要做如下几件事：

1. 两重 for 循环取输入字符串的所有可能子串。
2. 判断子串中不同字符的数目，这里使用可以去重的数据结构`Set`比较合适，最后输出`Set`的大小即为不同字符的数目。
3. 判断不同字符数是否为 fibonaci 数，由于子串数目较多，故 fibonaci 应该首先生成，由于字符串输入最大长度为100，故使用哈希表这种查询时间复杂度为 $$O(1)$$ 的数据结构。
4. 将符合条件的子串加入到最终结果，由于结果需要去重，故选用`Set`数据结构。

### Java

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String input = in.nextLine();
        Set<String> result = solve(input);
        for (String s : result) {
            System.out.println(s);
        }
    }

    public static Set<String> solve(String input) {
        Set<Long> fibonaci = fibonaci_number(input.length());
        Set<String> res = new TreeSet<String>();
        for (int i = 0; i < input.length(); i++) {
            for (int j = i + 1; j <= input.length(); j++) {
                String substr = input.substring(i, j);
                if (isFibonaci(substr, fibonaci)) {
                    res.add(substr);
                }
            }
        }

        return res;
    }

    public static boolean isFibonaci(String s, Set<Long> fibo) {
        Set<character> charSet = new HashSet<character>();
        for (character c : s.tocharArray()) {
            charSet.add(c);
        }
        // convert charSet.size() to long
        if (fibo.contains((long)charSet.size())) {
            return true;
        } else {
            return false;
        }
    }

    public static Set<Long> fibonaci_number(int n) {
        // generate fibonaci number till n
        Set<Long> fibonaci = new HashSet<Long>();
        long fn2 = 1, fn1 = 1, fn = 1;
        fibonaci.add(fn);
        for (int i = 3; i <= n; i++) {
            fn = fn1 + fn2;
            fibonaci.add(fn);
            fn2 = fn1;
            fn1 = fn;
        }
        return fibonaci;
    }
}
```

### 源码分析

fibonaci 数组的生成使用迭代的方式，由于保存的是`Long`类型，故在判断子串 size 时需要将 size 转换为`long`. Java 中常用的 Set 有两种，无序的`HashSet`和有序的`TreeSet`.

### 复杂度分析

遍历所有可能子串，时间复杂度 $$O(n^2)$$, fibonaci 数组和临时子串，空间复杂度 $$O(n)$$.
