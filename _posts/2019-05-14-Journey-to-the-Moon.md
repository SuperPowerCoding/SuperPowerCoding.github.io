---
layout: post
title: Journey to the Moon
categories: HackerRank
tags: 
- HackerRank
- Graph Theory
---

## **HackerRank link**
[Journey to the Moon](https://www.hackerrank.com/challenges/journey-to-the-moon/problem)

## **풀이 과정**
사실 1년전쯤 푼 문제인데, 언제푼지 기억이 안나서 14일 일자로 등록.

문제를 푸는데 두가지가 핵심이 될 것 같다.
1. 우주 비행사를 나라별로 어떻게 분류 할 수 있는가?
2. 분류를 하고 나서 어떻게 답을 구할 수 있는가?

나라 별로 분류 하고 나서 일단 답을 구하는 방법을 생각하면,  

예제 입력1을 봤을 때,

**Sample Input1**
```
4 1
0 2
```
입력에 의해 [0,2], [1], [3]으로 나라가 나뉘고,  

[0,1], [0,3], [2,1], [2,3], [1,3] 총 5가지가 된다.

즉, sum((전체 인원수 - 해당 나라 인원수)*(해당 나라 인원수))/2 = 나머지 페어 수 가된다.

곱을 하는 이유는 해당 나라 인원 수 만큼 경우의 수가 생기고,

나누기를 하는 이유는 중복을 제외한 것이다.

[0,2]의 경우 4 - 2 = 2 * 2 = 4
[1]의 경우 4 - 1 = 3  
[3]의 경우 4 - 1 = 3  

10/2 = 5 가 된다.

우주 비행사를 나라별로 어떻게 분류하는 것인가는

우주 비행사 별 나라 정보를 기억하는 배열, 나라별 수를 기억하는 배열이 두가지로 해결가능하다.

#### **ArrayList vs 배열**

사실 2번째 풀땐, 보기 좋으라고 ArrayList 형태로 풀어 보았다. 나라별로 보기에는 편하지만, ArrayList의 메모리 할당과정이 많이 걸려서 결과적으로는 ArrayList 버전이 더 느렸다. 

**ref input**
```
100000 2
1 2
3 4
```
|         | ArrayList | 배열 | 
|-------|--------|---------|
|메모리 할당   | 0.011  | 0.002      | 
|알고리즘 수행 | 0.0     | 0.002      | 
|합계    | 0.013  | 0.004      | 


## **풀이 핵심**
1. 배열 2개로 우주 비행사 별 나라 정보를 기록하고, 나라별 우주 비행사의 수를 기록한다.
2. 답은 sum((전체 인원수 - 해당 나라 인원수)*(해당 나라 인원수))/2 = 나머지 페어 수 로 구할 수 있다.

## **Algorithm**
1. 나라별 우주 비행사 별 배열의 용량을 할당하고 초기화 한다. 처음에는 각 나라의 우주 비행사 수는 1, 그리고 서로 다른나라라고 가정한다.
2. 입력이 들어왔을 때, 서로 다른 나라인 경우 해당 나라로 전부 바꿔준다. 바꿔야되는 나라를 전체 검색하여 바꾸고, 바꿀때마다 나라별 우주 비행사수를 갱신한다.

## **Source Code**
**배열 버전**
```java
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.regex.*;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;


public class Solution1 {

    static long journeyToMoon(int n, int[][] astronaut) {
    	boolean debugPrint = false;
    	long numPairs = 0;
    	long pre = System.currentTimeMillis();
    	int[] country = new int[n];
    	int[] count = new int[n];
  	
    	// assume all of astronauts are from different country.
    	for(int i = 0 ; i < n; i++)
    	{
    		country[i] = i;
    		count[i] = 1;
    	}
    	System.out.println((float)(System.currentTimeMillis() - pre)/1000.0);  
    	pre = System.currentTimeMillis();
    	for(int i = 0 ; i < astronaut.length ; i++)
    	{
    		int c0 = astronaut[i][0];
    		int c1 = astronaut[i][1];
    		
    		if(country[c0] != country[c1])
    		{
				int c = country[c0] < country[c1] ? country[c0] : country[c1];
				int change = country[c0] > country[c1] ? country[c0] : country[c1];
				for(int j = 0; j<n ; j++)
				{
					if(country[j] == change)
					{
						count[change]--;
						country[j] = c;
						count[c]++;
					}
				}        		
    		}
    	}
    	System.out.println((float)(System.currentTimeMillis() - pre)/1000.0);  
    	if(debugPrint)
    	{
    		for(int i = 0 ; i< n ; i++)
        	{
        		System.out.println(i+":"+country[i]);
        	}
        	
        	System.out.println("-------------");
        	for(int i = 0 ; i< n ; i++)
        	{
        		int countyrCode = i;
        		System.out.println(countyrCode+":"+count[i]);
        	}
        	System.out.println("-------------");
    	}
    	
    	numPairs = 0;

    	for(int i = 0 ; i< n; i++)
    	{
   			int cnt = n-count[country[i]];   			
    		numPairs += (cnt);
    		
    		if(debugPrint) System.out.println(i+":"+cnt);
    	}
    	
    	numPairs /= 2;
    	return numPairs;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        // BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] np = scanner.nextLine().split(" ");

        int n = Integer.parseInt(np[0]);

        int p = Integer.parseInt(np[1]);

        int[][] astronaut = new int[p][2];

        for (int i = 0; i < p; i++) {
            String[] astronautRowItems = scanner.nextLine().split(" ");
            scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

            for (int j = 0; j < 2; j++) {
                int astronautItem = Integer.parseInt(astronautRowItems[j]);
                astronaut[i][j] = astronautItem;
            }
        }

        long pre = System.currentTimeMillis();
        long result = journeyToMoon(n, astronaut);

//        System.out.println(result);
        System.out.println(result+":"+(float)(System.currentTimeMillis() - pre)/1000.0);
        // bufferedWriter.write(String.valueOf(result));
        // bufferedWriter.newLine();

        // bufferedWriter.close();

        scanner.close();
    }
    
}
```
**ArrayList 버전**
```java
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.regex.*;

public class Solution2 {

    // Complete the journeyToMoon function below.
    static long journeyToMoon(int n, int[][] astronaut) {
    	long numPairs = 0;
    	long pre = System.currentTimeMillis();
    	List<Integer> astronautList[] = new ArrayList[n];		// 나라 가지고 있는 우주 비행사 정보
    	int[] country = new int[n];								// 우주비행사의 나라 정보
    	for(int i = 0; i< n; i++) {
    		astronautList[i] = new ArrayList<Integer>();
    		astronautList[i].add(i);
    		country[i] = i;
    	}
    	System.out.println((float)(System.currentTimeMillis() - pre)/1000.0);
    	pre = System.currentTimeMillis();
    	for(int i = 0; i < astronaut.length; i++) {
    		int a1 = astronaut[i][0];
    		int a2 = astronaut[i][1];
    		
    		// 서로 다른 나라면
    		if(country[a1] != country[a2]) { 
    			// 해당 나라의 모든 우주 비행사들을 a1쪽으로 나라를 바꾼다.
    			int haveToDelete = country[a2];
    			for(int temp : astronautList[country[a2]]) {
    				astronautList[country[a1]].add(temp);
    				country[temp] = country[a1];
    			}
    			astronautList[haveToDelete].clear();
    		}
    	}
    	System.out.println((float)(System.currentTimeMillis() - pre)/1000.0);
    	for(int i = 0; i < astronautList.length; i++) {
    		int size = astronautList[i].size();
    		if(size != 0) {
    			int cnt = (n-size)*size;  
    			numPairs += (cnt); 
    		}	
    	}

    	return numPairs/2;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
//        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] np = scanner.nextLine().split(" ");

        int n = Integer.parseInt(np[0]);

        int p = Integer.parseInt(np[1]);

        int[][] astronaut = new int[p][2];

        for (int i = 0; i < p; i++) {
//            String[] astronautRowItems = scanner.nextLine().split(" ");
//            scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

            for (int j = 0; j < 2; j++) {
//                int astronautItem = Integer.parseInt(astronautRowItems[j]);
            	int astronautItem = scanner.nextInt();
                astronaut[i][j] = astronautItem;
            }
        }
        
        long pre = System.currentTimeMillis();
        
        long result = journeyToMoon(n, astronaut);
        System.out.println(result+":"+(float)(System.currentTimeMillis() - pre)/1000.0);
//        bufferedWriter.write(String.valueOf(result));
//        bufferedWriter.newLine();

//        bufferedWriter.close();

        scanner.close();
    }
    
}

```
