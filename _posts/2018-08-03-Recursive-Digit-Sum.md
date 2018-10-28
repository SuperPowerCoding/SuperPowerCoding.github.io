---
layout: post
title: Recursive Digit Sum
categories: HackerRank
tags:
- HackerRank
- Recursion
---

## **HackerRank link**
[Recursive Digit Sum](https://www.hackerrank.com/challenges/recursive-digit-sum/problem)


## **풀이 핵심**  
- 이름 그대로 재귀 함수를 이용하여 풀이
- 입력에 k 번 반복의 경우 k번 반복 할 필요 없이 k를 곱하면 간단하게 해결  

## **Algorithm**
1. 문자열의 길이가 1이면 구할 필요가 없음. 바로 리턴.
2. 각 문자열의 숫자를 합하고 k를 곱한다.
3. 문자열의 길이가 1이 될때까지 각 문자열의 숫자를 합하여 super digit을 구한다. (재귀함수)

## **Source Code**
{% highlight js %}
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.regex.*;

public class Solution {

    static long super_digit(String n)
    {
        if(n.length() == 1)
            return Integer.parseInt(n);
        long sum = 0;
        System.out.print(n+"->");
        for(int i = 0 ; i < n.length() ; i++)
        {
            long temp = Long.parseLong(n.substring(i,i+1)); 
            sum += temp;
            // System.out.print("("+temp+")");
        }
        System.out.println(sum);
        return super_digit(Long.toString(sum));
    }
    
    // Complete the superDigit function below.
    static long superDigit(String n, int k) {
        if(n.length() == 1)
            return Integer.parseInt(n);
        long sum = 0;
        System.out.print(n+"->");
        for(int i = 0 ; i < n.length() ; i++)
        {
             sum += Long.parseLong(n.substring(i,i+1));
        }
        sum = sum * k;
        System.out.println(sum);
        
        return super_digit(Long.toString(sum));     

    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] nk = scanner.nextLine().split(" ");

        String n = nk[0];

        int k = Integer.parseInt(nk[1]);

        long result = superDigit(n, k);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedWriter.close();

        scanner.close();
    }
}

{% endhighlight %}

