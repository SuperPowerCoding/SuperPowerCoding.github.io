---
layout: post
title: Bigger is Greater
categories: HackerRank
tags:
- HackerRank
- Implementation
---

## **HackerRank link**
[Bigger is Greater](https://www.hackerrank.com/challenges/bigger-is-greater/problem)


## **풀이 핵심** 
입력의 바로 다음 큰 문자열을 잘 만들어 내는 것이 핵심.

## **Algorithm**
1. String 입력을 Integer로 변환 한다. (편의성을 위해)
2. 바로 다음 수를 구한다.

### 다음수를 구하는 방법
ex) dabbcca
1. 바뀌어야 되는 지점을 찾는다. (내림차순 순의 처음지점): dab<span style="color:red">b</span>cca
2. 바로 큰수와 교환 : dab<span style="color:blue">b</span>c<span style="color:blue">c</span>a → da<span style="color:red">c</span>c<span style="color:red">b</span>a
3. 바뀌어야 되는 지점이후 우측은 거꾸로 돌려준다.  : dabc<span style="color:blue">cba</span> → dabc<span style="color:red">abc</span>

## **Source Code**
```java
import java.io.*;
import java.util.*;
import java.text.*;
import java.math.*;
import java.util.regex.*;

public class Solution {
 
    static boolean getNextNum(int[] n)
    {
        boolean debugPrint = false;
        
        if(n.length == 1)   return false;
        
     
        // find position that will be changed
        int i = n.length - 1;
        if(debugPrint) System.out.print(i+"->");
        while((i-1>=0) && (n[i-1] >= n[i]))
        {
            i--;
        }        
        i--;
        if(debugPrint) System.out.println(i);
        
        if(i < 0)  return false;
        
        // find bigger number of n[i]
        int j = n.length - 1;
        while(n[i] >= n[j])
        {
            j--;
        }
        
        if(debugPrint) System.out.println(j);
        
        int temp = n[i];
        n[i] = n[j];
        n[j] = temp;
        
        j = n.length - 1;
        i++;
        while (i < j) {
            temp = n[i];
            n[i] = n[j];
            n[j] = temp;
            i++;
            j--;
        }
        
        return true;
    }
    
    
    static String biggerIsGreater(String w) {
        // Complete this function
        int[] wi = new int[w.length()];
        
        for(int i = 0 ; i < w.length() ; i++)
        {
            wi[i] = w.charAt(i) - 'a';
        }
        
        // next number
        if(getNextNum(wi) == false) return "no answer";
        else
        {
            char[] str = new char[wi.length];
            
            for(int i = 0 ; i < wi.length ; i++)
            {
               str[i] = (char)((char)wi[i] + 'a');
            }
            
            return new String(str,0,str.length);
        }
    }

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int T = in.nextInt();
        for(int a0 = 0; a0 < T; a0++){
            String w = in.next();
            String result = biggerIsGreater(w);
            System.out.println(result);
        }
        in.close();
    }
}
```

