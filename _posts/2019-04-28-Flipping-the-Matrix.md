---
layout: post
title: Flipping the Matrix
categories: HackerRank
tags: 
- HackerRank
- Constructive Algorithms
---

## **HackerRank link**
[Flipping the Matrix](https://www.hackerrank.com/challenges/flipping-the-matrix/problem)


## **풀이 핵심**
1. reverse만 하기 때문에 이동 가능한 좌표가 따로 있다.
2. 하나씩 reverse를 이용해서 채우면 어떠한 경우에도 최대값을 위치 시킬 수 있다.  

ex)  문제 예제에서   
112 42 83 119  
56 125 56 49  
15 78 101 43  
62 98 114 108  

- (0,0)에 올 자리 : (0,0), (0, 3), (3, 0), (3, 3) 중 제일 큰값 : => 119
- (0,1)에 올 자리 : (0,1), (0, 2), (3, 1), (3, 2) 중 제일 큰값 : => 114
- (1,0)에 올 자리 : (1,0), (1, 3), (2, 0), (2, 3) 중 제일 큰값 : => 56
- (1,1)에 올 자리 : (1,1), (1, 2), (2, 1), (2, 2) 중 제일 큰값 : => 125 

모두를 더하면 414

## **Algorithm**
위 예제에서 필요한 값을 보면,
1. matrix의 최대 길이 : 좌표
2. matrix의 절반 길이 : 반복할 횟수  

아래 두 값을 이용하여 간단하게 이중 for문을 이용하여 돌려서,   
각 네 좌표의 최대값을 순서데로 더한다. 

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

    static int max(int a, int b, int c, int d) {
        int max = 0;
        int[] arr = {a,b,c,d};
        
        for(int i : arr) {
            if(i > max) max = i;
        }
        
        return max;
    }
    
    // Complete the flippingMatrix function below.
    static int flippingMatrix(int[][] matrix) {
        int sum = 0;
        /*
         1. 0,0에 올 자리 : (0,0), (0, 3), (3, 0), (3, 3) 중 제일 큰값 : => 119
                          
         2. 0,1에 올 자리 : (0,1), (0, 2), (3, 1), (3, 2) 중 제일 큰값 : => 114
         
         3. 1,0에 올 자리 : (1,0), (1, 3), (2, 0), (2, 3) 중 제일 큰값 : => 56
         
         4. 1,1에 올 자리 : (1,1), (1, 2), (2, 1), (2, 2) 중 제일 큰값 : => 125 
         
             112 42 83 119
            56 125 56 49
            15 78 101 43
            62 98 114 108
         */
        
        int l = matrix.length / 2;
        int m = matrix.length;
        
        for(int y = 0 ; y < l ; y++) {
            for(int x = 0 ; x < l ; x++) {
                sum += max(matrix[y][x], matrix[y][m-1-x], matrix[m-1-y][x], matrix[m-1-y][m-1-x]);
            }
        }
        
        
        return sum;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int q = scanner.nextInt();
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        for (int qItr = 0; qItr < q; qItr++) {
            int n = scanner.nextInt();
            scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

            int[][] matrix = new int[2*n][2*n];

            for (int i = 0; i < 2*n; i++) {
                String[] matrixRowItems = scanner.nextLine().split(" ");
                scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

                for (int j = 0; j < 2*n; j++) {
                    int matrixItem = Integer.parseInt(matrixRowItems[j]);
                    matrix[i][j] = matrixItem;
                }
            }

            int result = flippingMatrix(matrix);

            bufferedWriter.write(String.valueOf(result));
            bufferedWriter.newLine();
        }

        bufferedWriter.close();

        scanner.close();
    }
}
```