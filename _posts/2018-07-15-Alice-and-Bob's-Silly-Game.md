---
layout: post
title: Alice and Bob's Silly Game
categories: HackerRank
tags:
- HackerRank
- Game Theory
---

## **HackerRank link**
[Alice and Bob's Silly Game](https://www.hackerrank.com/challenges/alice-and-bobs-silly-game/problem)

## **풀이 핵심**
- Alice와 Bob이 서로 반복하기 때문에 계속 반복할 필요 없이 소수의 총수를 구하면 간단하게 구할 수 있다.

## **Algorithm**
1. 소수를 구하면서 전체 소수의 갯수를 센다.
2. 소수의 수가 짝수이면 Bob, 아니면 Alice를 리턴한다.

## **Source Code**
```java
import java.io.*;
import java.math.*;
import java.text.*;
import java.util.*;
import java.util.regex.*;

public class Solution {

    /*
     * Complete the sillyGame function below.
     */
    static String sillyGame(int n) {
        /*
         * Write your code here.
         */
		boolean[] primeNum = new boolean[n+1];
		int count = 0;
		
		primeNum[0] = true;
		primeNum[1] = true;
		
		for(int i = 2; i <= n ; i++)
		{
			if(primeNum[i] == false)
			{
				count++;
				for(int j = 2 ; j*i < primeNum.length; j++)
				{
					primeNum[i*j] = true;
				}
			}
		}
		
		if(count % 2 == 0)	return "Bob";
		
		return "Alice";
	}

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int g = Integer.parseInt(scanner.nextLine().trim());

        for (int gItr = 0; gItr < g; gItr++) {
            int n = Integer.parseInt(scanner.nextLine().trim());

            String result = sillyGame(n);

            bufferedWriter.write(result);
            bufferedWriter.newLine();
        }

        bufferedWriter.close();
    }
}
```

