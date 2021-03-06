---
layout: post
title: Project Euler #89: Roman numerals
categories: HackerRank
tags:
- HackerRank
- Project Euler
---

## **HackerRank link**
[Project Euler #89: Roman numerals](https://www.hackerrank.com/challenges/cut-the-tree/problem)

## **풀이 핵심**
1. 주어진 사양에 맞게 코딩
2. 문자열→숫자, 숫자→문자열 변환

## **Algorithm**
1. 로마 숫자를 숫자로 변환한다.
2. 변환된 숫자를 로마 숫자로 변환한다.

## **Source Code**
```java
import java.io.*;
import java.util.*;
import java.text.*;
import java.math.*;
import java.util.regex.*;

public class Solution {

    static int RomanNumStr2Num(String str)
    {
        int[] number         = new int[]     {  1,  5, 10, 50,100,500,1000};
        char[] romanStr     = new char[]    {'I','V','X','L','C','D','M'};
        
        int pre = 0;
        int num = 0;
        
        int reVal = 0;
        
        for(int i = 0 ; i < str.length(); i++)
        {
            char ch = str.charAt(i);
            for(int j = romanStr.length - 1 ; j >= 0 ; j--)
            {
                if(ch == romanStr[j])
                {
                    num = number[j];
                    reVal += num;
                    if(pre < num)
                    {
                        reVal -= 2 * pre; 
                    }
                    pre = num;
                    break;
                }
            }
        }
        
        return reVal;
        
    }
    
    static String Num2RomanStr(int num)
    {
        int[] number         = new int[]     {  1,   4,  5,   9, 10,  40, 50,  90,100, 400,500, 900, 1000};
        String[] romanStr     = new String[]    {"I","IV","V","IX","X","XL","L","XC","C","CD","D","CM","M"};
        
        StringBuffer roman = new StringBuffer();
        
        while(num > 0 )
        {
            for(int j = romanStr.length - 1 ; j >= 0 ; j--)
            {
                int sub = num - number[j];
                
                if(sub < 0 ) {continue;}
                
                roman.append(romanStr[j]); 
                num -= number[j];
                break;
            }
        }
        
        
        return roman.toString();        
    }
    
    static String RomanNumerals(String str)
    {
        int num = RomanNumStr2Num(str);
        
        return Num2RomanStr(num);
    }    
    
    private static final Scanner scanner = new Scanner(System.in);
    
    public static void main(String[] args) {
        /* Enter your code here. Read input from STDIN. Print output to STDOUT. Your class should be named Solution. */
        
        // Unit test for RomanNumStr2Num 
        // System.out.println(RomanNumStr2Num("CCXX"));
        
        // Unit test for Num2RomanStr 
        // System.out.println(Num2RomanStr(999));

        
        
        int nTimes = scanner.nextInt();
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");
                
        for(int i = 0 ; i < nTimes ; i++)
        {
            String str = scanner.nextLine();
            
            String answer = RomanNumerals(str);
            
            System.out.println(answer);            
        }
        
        
        
    }
}
```

