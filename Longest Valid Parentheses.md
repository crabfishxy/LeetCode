# Longest Valid Parentheses

## Description

Given a string containing just the characters '(' and ')', find the length of the longest valid (well-formed) parentheses substring.

For "(()", the longest valid parentheses substring is "()", which has length = 2.

Another example is ")()())", where the longest valid parentheses substring is "()()", which has length = 4.

## My Solution

我的方法比讨论区的要复杂一些，并且时间效率也不高，主要还是因为动态规划不熟练。

用了一个stack，一个和字符串长度相同的数组来解决。大体分为两步：

1. index和类型组成一个Map入栈，没当一个'('和一个')'匹配掉，将这对括号的index对应的数组置为1
2. 遍历完成后对数组进行遍历，找到最长的连续'1'即可

## My Code

```Java
import java.util.*;
class Solution {
    public int longestValidParentheses(String s) {
        Stack<Map.Entry<Integer, String>> stack = new Stack<>();
        List<Integer> list = new LinkedList<>();
        int f[] = new int[s.length()];
        int max = 0;
        for(int i = 0; i < s.length(); i ++){
            String c = "" + s.charAt(i);
            if(stack.size() == 0){
                stack.push(new AbstractMap.SimpleEntry<>(i, c));
                continue;
            }
            if(stack.peek().getValue().equals(")")){
                stack.clear();
                stack.push(new AbstractMap.SimpleEntry<>(i, c));
                continue;
            }
            if(stack.peek().getValue().equals("(") && c.equals(")")){
                f[i] = 1;
                f[stack.peek().getKey()] = 1;
                stack.pop();

                continue;
            }
            if(stack.peek().getValue().equals("(") && c.equals("(")){
                stack.push(new AbstractMap.SimpleEntry<>(i, c));
            }else{
                stack.push(new AbstractMap.SimpleEntry<>(i, c));
            }


        }
        for(int i =1 ; i < s.length(); i ++){
            if(f[i] != 0 ){
                f[i] = f[i-1] + 1;
                if(f[i] > max)max = f[i];
            }
        }
        return max/2*2;
    }
}
```

## Standard Solution

DP：

1. s[i] = ')' and s[i-1] = '('. dp[i] = dp[i-2] + 2

2. s[i] = ')' and s[i-1] = ')'

if s[i-dp[i-1]-1] = '(' then


dp[i]=dp[i−1]+dp[i−dp[i−1]−2]+2