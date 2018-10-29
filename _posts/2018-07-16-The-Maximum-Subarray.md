---
layout: post
title: The Maximum Subarray
categories: HackerRank
tags:
- HackerRank
- Dynamic Programming
---

## **HackerRank link**
[The Maximum Subarray](https://www.hackerrank.com/challenges/maxsubarray/submissions/code/78369109)

## **풀이 핵심**
두가지를 풀어야 됨.  
[max subsequence]  
1. 단순히 양수를 더하면 최댓값을 알 수 있음.  

[max subArray]  
1. 동적 프로그래밍으로 풀이
2. 아래의 표와 같이 최대 subArray의 sum은 **sum의 최대값 - sum의 최소값(단, 최대값 이전 index)** 이다.

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-292d{font-weight:bold;background-color:#ffccc9;color:#fe0000;border-color:#000000;text-align:center;vertical-align:top}
.tg .tg-yes0{font-weight:bold;border-color:#000000;text-align:center}
.tg .tg-mqa1{font-weight:bold;border-color:#000000;text-align:center;vertical-align:top}
.tg .tg-40tn{background-color:#ffccc9;color:#333333;border-color:#000000;text-align:center;vertical-align:top}
.tg .tg-cijy{background-color:#ffccc9;color:#333333;border-color:#000000;text-align:center}
.tg .tg-z118{font-weight:bold;background-color:#ffccc9;color:#3166ff;border-color:#000000;text-align:center;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-yes0">index</th>
    <th class="tg-mqa1">0</th>
    <th class="tg-yes0">1</th>
    <th class="tg-yes0">2</th>
    <th class="tg-yes0">3</th>
    <th class="tg-mqa1">4</th>
    <th class="tg-mqa1">5</th>
    <th class="tg-mqa1">6</th>
  </tr>
  <tr>
    <td class="tg-yes0">arr</td>
    <td class="tg-40tn">-2</td>
    <td class="tg-cijy">2</td>
    <td class="tg-cijy">-1</td>
    <td class="tg-cijy">2</td>
    <td class="tg-40tn">3</td>
    <td class="tg-40tn">4</td>
    <td class="tg-40tn">-5</td>
  </tr>
  <tr>
    <td class="tg-yes0">sum</td>
    <td class="tg-z118">-2</td>
    <td class="tg-cijy">0</td>
    <td class="tg-cijy">-1</td>
    <td class="tg-cijy">1</td>
    <td class="tg-40tn">4</td>
    <td class="tg-292d">8</td>
    <td class="tg-40tn">-3</td>
  </tr>
</table>

답은 10

## **Algorithm**
1. 현재까지의 sum 값을 구한다.
2. 최소 값과 최대값을 업데이트 하며, subArrMax를 업데이트한다.
3. 양수를 더하며 subsequence의 최대값을 구한다.
4. 과정 1~4를 반복한다.

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

    // Complete the maxSubarray function below.
    static long[] maxSubarray(long[] arr) {
    	long[] sum = new long[arr.length];
    	
    	long subArrMax = arr[0];
    	long sumMin = arr[0];
    	long subSeqMax = Long.MIN_VALUE;
    	
    	for(int i = 0 ; i < arr.length ; i++)
    	{
    		if(i == 0)
    		{
    			sum[i] = arr[0];
    		}
    		else
    		{
    			sum[i] += sum[i-1] + arr[i];
    			if(sum[i-1] < sumMin) sumMin = sum[i-1];
    			
    			long subMax = sum[i] - sumMin;
    			subMax = sum[i] < subMax ? subMax : sum[i];
    			
    			if(subArrMax < subMax) subArrMax = subMax;
    		}
    		
    		if(arr[i] > 0)
			{
				if(subSeqMax > 0)
				{
					subSeqMax += arr[i];
				}
				else
				{
					subSeqMax = arr[i];
				}
			}
			else
			{
				if(subSeqMax < 0 )
				{
					if(subSeqMax < arr[i])
					{
    					subSeqMax = arr[i];
					}
				}    				
			}
    	}
    	
    	long[] result = new long[2];
    	result[0] = subArrMax;	// 2, -1, 2, 3, 4
    	result[1] = subSeqMax;	// 2,  2, 3, 4
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

            long[] arr = new long[n];

            String[] arrItems = scanner.nextLine().split(" ");
            scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

            for (int i = 0; i < n; i++) {
                long arrItem = Long.parseLong(arrItems[i]);
                arr[i] = arrItem;
            }

            long[] result = maxSubarray(arr);

            for (int i = 0; i < result.length; i++) {
                // bufferedWriter.write(String.valueOf(result[i]));
                bufferedWriter.write(Long.toString(result[i]));

                if (i != result.length - 1) {
                    bufferedWriter.write(" ");
                }
            }

            bufferedWriter.newLine();
        }

        bufferedWriter.close();

        scanner.close();
    }
}
```