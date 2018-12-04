---
layout: post
title: Maximum Palindromes
categories: HackerRank(solving)
tags: 
- HackerRank
- Strings
- solving
---

## **HackerRank link**
* [Maximum Palindromes](https://www.hackerrank.com/challenges/maximum-palindromes/problem)


## **풀이 핵심**
1. 페르마의 소정리
2. 분할소거법
3. 퍼포먼스를 위한 초기 계산

## **Algorithm**


## **Source Code**
```java
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.regex.*;

public class Solution {

    static final int size = 100001;
    static long mod = (long)Math.pow(10,9)+7;
    static long[] factorial = new long[size];
    static long[] inverseFac = new long[size];
    static int a2z[][];
    
    static long powerWithMod(long a, long n)
    {
        long ret = 1;
        while(n > 0)
        {
            if(n % 2 == 1)    // 홀수 인경우 한번 먼저 곱
            {
                ret *= a;
                ret %= mod;
                
            }
            a *= a;
            a %= mod;
            n /= 2;
        }
        
        return ret;
    }
    
    
    // Complete the initialize function below.
    static void initialize(String s) {
        // This function is called once before all queries.
        // initialize factorial
        factorial[0] = 1;
        factorial[1] = 1;
        for(int i = 2 ; i < factorial.length ; i++)
        {
            factorial[i] = factorial[i-1] * i % mod;
        }
        
        // inverse factorial for modulation.
        // a^(P-1)
        // a x X = 1 (mod P-1)
        // X = a^(P-2)
        
        inverseFac[size-1] = powerWithMod(factorial[size-1], mod - 2);
        for (int i = size - 2; i >= 0; i--) 
        {
            inverseFac[i] = (inverseFac[i + 1] * (i + 1)) % mod;     
        }
        
        a2z = new int[s.length()+1]['z'-'a'+1];                
        
        for(int i = 0 ; i < s.length() ; i++)
        {
            System.arraycopy(a2z[i], 0, a2z[i + 1], 0, 'z' - 'a' + 1);
            a2z[i+1][s.charAt(i)-'a']++;
        }
    }

    // Complete the answerQuery function below.
    static int answerQuery(int l, int r) {
        // Return the answer for this query modulo 1000000007.
        int mid = 0;
        int total = 0;
        long inverse = 1;
        long result =1;
        int[] aTozCnt = new int['z'-'a' + 1];
        
        for(int i = 0 ; i < aTozCnt.length ; i++)
        {
            aTozCnt[i] = a2z[r][i] - a2z[l-1][i];
        }        
        
        for(int cnt : aTozCnt)
        {
            if(cnt % 2 == 0)    // 짝수
            {
                total += cnt/2;
                inverse = (inverse * inverseFac[cnt/2])%mod;
                
            }
            else // 홀수
            {
                if(cnt == 1)    // 한개
                {
                    mid++;
                }
                else
                {
                    total += (cnt - 1)/2;
                    mid++;
                }
                inverse = (inverse * inverseFac[(cnt-1)/2])%mod;
            }
        }
        if(mid == 0) mid =1;
        
        result = (factorial[total] * inverse) % mod;
        result = (result * mid) % mod;
   
        return (int)result;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        String s = scanner.nextLine();

        initialize(s);
        int q = scanner.nextInt();
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        for (int qItr = 0; qItr < q; qItr++) {
            // String[] lr = scanner.nextLine().split(" ");

            int l = scanner.nextInt();

            int r = scanner.nextInt();

            int result = answerQuery(l, r);

            System.out.println(result);
        }

        scanner.close();
    }
}
```