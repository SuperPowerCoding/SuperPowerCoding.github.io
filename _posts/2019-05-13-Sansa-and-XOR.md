---
layout: post
title: Sansa and XOR
categories: HackerRank
tags: 
- HackerRank
- Bit Manipulation
---

## **HackerRank link**
[Sansa and XOR](https://www.hackerrank.com/challenges/sansa-and-xor/problem)

## **풀이 과정**
예제에 나온데로 순차적으로 계산하면 답이 될 수 있다.

하지만, 제약조건을 봤을때, 2 <= n <= 10^5 이다. 게다가 한번만 계산하는게 아니라

예제를 보면 
```
입력 배열이 arr = [3,4,5] 이면,  

3^4^5^(3^4)^(4^5)^(4^5)^(3^4^5) 
```
순으로 계산한다.

굉장히 많은 연산량이 필요하게 된다.

다른 방법을 생각해 보자.

Xor은 해당 비트가 같으면 0 아니면 1이다.

그리고, 몇번 xor 연산을 해보면 이러한 특징을 발견 할 수 있다.

1. 자기자신을 xor 하면 0이다. 왜냐면 똑같으니까.
2. xor은 분배법칙, 교환법칙 모두가 통한다. 즉 3^4^5^(3^4^5) = 3^4^5^3^4^5
3. 배열의 사이즈가 짝수인경우 각 배열의 원소가 짝수번 xor 연산을 하게 되어 무조건 0이다.
4. 배열의 사이즈가 홀수인경우 원소의 짝수번째 index는 짝수번 나오게되어 0이다. (시작이 1부터라고 한 경우)

이 특징을 이용하면 O(n)으로 풀 수 있다.

## **풀이 핵심**
1. 배열의 사이즈가 짝수면 무조건 답은 0이다.
2. 배열의 사이즈가 홀수인경우 짝수번째는 0이다.

## **Algorithm**
1. 배열의 사이즈를 체크하면 짝수면 0을 리턴한다.
2. 그게 아니라면, 배열의 짝수번째 index를 모두 xor 연산을 한다.

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

    // Complete the sansaXor function below.
    static int sansaXor(int[] arr) {
        
        if(arr.length % 2 == 0) return 0;
        
        int result = 0;
        
        for(int i = 0 ; i < arr.length; i+=2) {           
            result = result ^ arr[i];            
        }
        
        return result;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int t = scanner.nextInt();
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        for (int tItr = 0; tItr < t; tItr++) {
            int n = scanner.nextInt();
            scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

            int[] arr = new int[n];

            String[] arrItems = scanner.nextLine().split(" ");
            scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

            for (int i = 0; i < n; i++) {
                int arrItem = Integer.parseInt(arrItems[i]);
                arr[i] = arrItem;
            }

            int result = sansaXor(arr);

            bufferedWriter.write(String.valueOf(result));
            bufferedWriter.newLine();
        }

        bufferedWriter.close();

        scanner.close();
    }
}

```
