---
layout: post
title: Stock Maximize
categories: HackerRank
tags: 
- HackerRank
- Dynamic Programming
---

## **HackerRank link**
[Stock Maximize](https://www.hackerrank.com/challenges/stockmax/problem)


## **풀이 핵심**
1. 최댓값이 바뀌는 시점을 파악한다.
2. 거꾸로 시작해서 최댓값이 바뀌는 시점까지 이익을 계산한다.

## **Algorithm**
1. 거꾸로 시작해서 최댓값이 바뀌는 시점까지 이익을 계산한다.

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
     
    // Complete the stockmax function below.
    static long stockmax(long[] prices) {
        long max = 0;
        long profit = 0;
        
        for(int i = prices.length - 1; i >= 0; i--) {
            if(prices[i] > max) {
                max = prices[i];
            }
            
            profit += (max - prices[i]);
        }
        
        return profit;

    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int t = scanner.nextInt();
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        for (int tItr = 0; tItr < t; tItr++) {
            int n = scanner.nextInt();
            scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");
          
            String[] pricesItems = scanner.nextLine().split(" ");
            
            n = pricesItems.length;
                    
            long[] prices = new long[n];
            
            scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

            for (int i = 0; i < n; i++) {
                long pricesItem = Long.parseLong(pricesItems[i]);
                prices[i] = pricesItem;
            }

            long result = stockmax(prices);
            System.out.println(result);

            bufferedWriter.write(String.valueOf(result));
            bufferedWriter.newLine();
        }

        bufferedWriter.close();

        scanner.close();
    }
}
```
