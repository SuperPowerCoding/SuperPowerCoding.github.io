---
layout: post
title: The Power Sum
categories: HackerRank
tags:
- HackerRank
- Recursion
---

## **HackerRank link**
[The Power Sum](https://www.hackerrank.com/challenges/the-power-sum/problem)

## **풀이 핵심**
- 모든 수를 더해보면서 **X가 어떤 수들의 N제곱의 합**인지 확인한다.

ex) X = 10, N = 2 </br>
(X) X > 1^2  </br>
(X) X > 1^2 + 2^2 </br>
(X) X < 1^2 + 2^2 + 3^2 </br>
(<span style="color:red">O</span>) X = 1^2 + 3^2 </br>
... </br>

## **Algorithm**
1. 처음 입력을 1로 하여 1^N 부터 시작한다.
2. N승한 값을 구한다.
3. 현재까지의 합을 구한다.
4. 맞으면 count 후 리턴
5. 크면 현재까지의 count 값 리턴
6. 작으면 다음 수를 설정하여 과정 2부터 다시 구한다.

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

    static double[] array;
	static int pSum(int X,int N, double cur, int depth)
	{
		int count = 0;		
		
		for(int i = (int)cur ; i < X ; i++)
		{
			double sum = 0;
			array[depth] = Math.pow(i, N);
			for(int j = 0; j < array.length ; j++)
			{
				sum += array[j];
			}
			
			if(sum == X) 
			{				
				array[depth] = 0;
				System.out.println(depth+":"+i+"true");
				return count+1;
			}
			else if(sum > X)
			{
				array[depth] = 0;
				System.out.println(depth+":"+i+"=>"+sum+"false");
				return count;
			}
			
			System.out.print(depth+":"+i+"=>"+sum+" -> ");
			
			count += pSum(X,N,i+1,depth+1);
		}	
		
		return count;
	}
	
    // Complete the powerSum function below.
    static int powerSum(int X, int N) {
    	
    	int count = 0;
    	array = new double[X];
    	
    	count = pSum(X,N,1,0);
    			
    	return count;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int X = scanner.nextInt();
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        int N = scanner.nextInt();
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        int result = powerSum(X, N);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedWriter.close();

        scanner.close();
    }
}
```

