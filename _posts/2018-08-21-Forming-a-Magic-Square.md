---
layout: post
title: Forming a Magic Square
categories: HackerRank
tags:
- HackerRank
- Implementation
---


## **HackerRank link**
[Forming a Magic Square](https://www.hackerrank.com/challenges/magic-square-forming/problem?h_r=internal-search)


## **풀이 핵심**
- 경우에 따라선 복잡한 알고리즘으로 구하는 것보다 모든 경우를 직접 구해서 비교하는 방법이 더 빠르다.
- 3x3 마방진의 경우 모든 경우의 수는 9가지가 된다. (기본 형태 + 좌/우/상/하 대칭 + 90도씩 회전 = 9가지)

## **Algorithm**
1. 9가지의 마방진을 미리 입력해 둔다.
2. 입력으로 들어온 3x3행렬을 미리 입력 된 마방진을 통하여 cost의 최솟값을 구한다.

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

    static int[][][] magicSquare =            
        {    
            {
                {8, 1, 6},
                {3, 5, 7},
                {4, 9, 2}
            },
            {
                {4, 3, 8},
                {9, 5, 1},
                {2, 7, 6}
            },
            {
                {2, 9, 4},
                {7, 5, 3},
                {6, 1, 8}
            },        
            {
                {6, 7, 2},
                {1, 5, 9},
                {8, 3, 4}
            },        
            {
                {6, 1, 8},
                {7, 5, 3},
                {2, 9, 4}
            },       
            {
                {8, 3, 4},
                {1, 5, 9},
                {6, 7, 2}
            },               
            {
                {4, 9, 2},
                {3, 5, 7},
                {8, 1, 6}
            },
            {
                {2, 7, 6},
                {9, 5, 1},
                {4, 3, 8}
            }        
        };
    
    
    // Complete the formingMagicSquare function below.
    static int formingMagicSquare(int[][] s) {
        int min = Integer.MAX_VALUE;
        
        for(int i = 0 ; i < magicSquare.length ; i++)
        {
            int cost = 0;
            for(int j = 0; j < s.length ; j++)
            {
                for(int k = 0 ; k < s[j].length ; k++)
                {
                    cost += Math.abs(magicSquare[i][j][k] - s[j][k]);
                }
            }
            
            if(cost < min) min = cost;
        }
        
        return min;

    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int[][] s = new int[3][3];

        for (int i = 0; i < 3; i++) {
            String[] sRowItems = scanner.nextLine().split(" ");
            scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

            for (int j = 0; j < 3; j++) {
                int sItem = Integer.parseInt(sRowItems[j]);
                s[i][j] = sItem;
            }
        }

        int result = formingMagicSquare(s);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedWriter.close();

        scanner.close();
    }
}
{% endhighlight %}
