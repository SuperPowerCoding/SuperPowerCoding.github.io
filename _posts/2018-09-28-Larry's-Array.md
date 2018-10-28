---
layout: post
title: Larry's Array
categories: HackerRank
tags:
- HackerRank
- Implementation
---


## **HackerRank link**
[Larry's Array](https://www.hackerrank.com/challenges/larrys-array/problem)


## **풀이 핵심**
* 임의의 3개의 수를 rotate 하더라도 변하지 않는 수열의 특성을 이용하여 풀수가 있다. [inversion](https://en.wikipedia.org/wiki/Inversion_(discrete_mathematics))    

ex) 가능한 경우와 가능하지 않는 경우를 보면  
 - 가능한경우   
 arr = {1, 2, 3} 이고 rotate를 했을때 나오는 모든 경우와 inversion을 구하면,  
 {1, 2, 3} => 0  
 {2, 3, 1} => 2  
 {3, 1, 2} => 2  

 - 가능하지 않는 경우   
 arr = {1, 3, 2} 이고 rotate를 했을때 나오는 모든 경우와 inversion을 구하면,  
 {1, 3, 2} => 1  
 {3, 2, 1} => 3  
 {2, 1, 3} => 1  

한번 rotate하는 경우 inversion은 짝수개가 변한다. 
- 좌측 rotate : 가장 작은 수가 맨 우측으로 이동 inversion 2증가  
- 우측 rotate : 가장 큰수가 맨 좌측으로 이동    inversion 2증가  

따라서, 가능한 경우는 inversion이 항상 짝수개  
가능하지 않는 경우는 inversion이 항상 홀수개 이다.

## **Algorithm**
1. inversion을 구한다.
2. inversion이 짝수인 경우 YES, 아닌경우 NO 리턴


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

    // Complete the larrysArray function below.
    static String larrysArray(int[] A) {
        int inversions = 0;
        for(int i = 0 ; i < A.length - 1; i++)
        {
            for(int j = i+1 ; j< A.length ; j++)
            {
                if(A[i] > A[j]) inversions++;
            }
        }
        
        if(inversions % 2 == 0) return "YES";
            
        return "NO";
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int t = scanner.nextInt();
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        for (int tItr = 0; tItr < t; tItr++) {
            int n = scanner.nextInt();
            scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

            int[] A = new int[n];

            String[] AItems = scanner.nextLine().split(" ");
            scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

            for (int i = 0; i < n; i++) {
                int AItem = Integer.parseInt(AItems[i]);
                A[i] = AItem;
            }

            String result = larrysArray(A);

            bufferedWriter.write(result);
            bufferedWriter.newLine();
        }

        bufferedWriter.close();

        scanner.close();
    }
}
{% endhighlight %}
