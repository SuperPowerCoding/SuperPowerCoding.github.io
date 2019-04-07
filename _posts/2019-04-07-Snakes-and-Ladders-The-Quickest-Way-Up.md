---
layout: post
title: Snakes and Ladders: The Quickest Way Up
categories: HackerRank
tags: 
- HackerRank
- Breadth First Search
---

## **HackerRank link**
[Snakes and Ladders: The Quickest Way Up](https://www.hackerrank.com/challenges/the-quickest-way-up/problem)


## **풀이 핵심**
1. "최단거리의 탐색 = BFS(Breadth First Search)" 임을 이용한다.
2. 최단거리를 기록하는 것으로는 board에 기록하여 최단 거리를 기록한다.

## **Algorithm**
기본적으로 BFS알고리즘을 이용한다. Queue를 이용하여 서치.  

사다리와 뱀의 정보를 어떻게 이용할지는 
 - 사다리와 뱀정보를 보드에 기록하여 사용 (quickestWayUp)
 - 사다리와 뱀정보를 움직일때마다 계속 검색 (quickestWayUp2)  

일 수 있지만, 미리 보드에 기록하는 편이 더 좋음. 검색 횟수 ↓

1. 사다리와 뱀 정보를 맵에 업데이트 시킨다.   
   1-1. 사다리와 뱀은 -로 표기    
2. 목적지에 도착했는지 확인한다.
3. 주사위를 굴린다. (현재부터 최대 6부터 검색 - 무조건 빨리가야 좋은거니까)  
   2-1. 뱀 또는 사다리(-)라면 먼저 그곳으로 이동하고,  
   2-2. 아니라면 주사위의 최댓값으로 이동한다.    
4. 각각 이동시 이동정보를 board에 업데이트 한다.

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

	static boolean isDebug = false;
	
	static final int boardGoal = 100;
	
    // Complete the quickestWayUp function below.
    static int quickestWayUp(int[][] ladders, int[][] snakes) {

    	Queue<Integer> haveToMove = new LinkedList<>();
    	int[] board = new int[boardGoal+1];    	
    	
    	int[][][] searchList = new int[2][][];
    	
    	searchList[0] = ladders;
    	searchList[1] = snakes;
    	
    	haveToMove.add(1);
    	int cur = 1;
    	
    	// 사다리 뱀 정모를 맵에 업데이트 시킨다.
		for(int j = 0 ; j < searchList.length; j++)
		{
			int[][] tempList = searchList[j];
			
			for(int i = 0 ; i < tempList.length; i++)
    		{
    			int startP = tempList[i][0];
    			int endP = tempList[i][1];
    			
    			board[startP] = -endP;
    		}
		}
    	
    	while(haveToMove.isEmpty() == false)
    	{	
    		cur = haveToMove.poll();
    		int moves = board[cur];
    		
    		if(isDebug)System.out.println();    		
    		if(isDebug)System.out.print(cur+"("+moves+")"+"->");
    		
    		if(cur == boardGoal) return board[cur];
    		else if(cur + 6 >= boardGoal) return moves+1;
    		
    		moves += 1;
    		
    		// 주사위를 굴린다.
    		boolean diced = false;
    		for(int i = 6 ; i >= 1; i--)
    		{
    			// 만약 사다리 또는 뱀이라면.. 갑시다.
    			if(board[cur+i] < 0)
    			{	    				
    				board[cur+i] = -board[cur+i];
    				haveToMove.add(board[cur+i]);
    				board[board[cur+i]] = moves;
    			} // 주사의 최대값을 구한다.
    			else if(board[cur+i] == 0 && diced == false)
    			{
    				diced = true;
    				haveToMove.add(cur+i);
    				board[cur+i] = moves;
    			}
    		}
    	}
    	
    	if(board[boardGoal] == 0) return -1;
    	    	
    	return board[boardGoal];
    }
    
    static int quickestWayUp2(int[][] ladders, int[][] snakes) {

    	Queue<Integer> haveToMove = new LinkedList<>();
    	int[] board = new int[boardGoal+1];
    	
    	int[][][] searchList = new int[2][][];
    	
    	searchList[0] = ladders;
    	searchList[1] = snakes;
    	
    	haveToMove.add(1);
    	int cur = 1;
    	
    	while(haveToMove.isEmpty() == false)
    	{	
    		cur = haveToMove.poll();
    		int moves = board[cur];
    		
    		if(isDebug)System.out.println();    		
    		if(isDebug)System.out.print(cur+"("+moves+")"+"->");
  
    		boolean[] cantMove = new boolean[6];
    				
    		int minMoves = cur + 1;
    		int maxMoves = cur + 6;
    		
    		moves += 1;
    		
    		// check end
    		if(maxMoves >= boardGoal)
    		{    			
    			board[boardGoal] = cur == boardGoal ? moves - 1: moves;
    			break;
    		}
    		
    		// check ladder & snake
    		for(int j = 0 ; j < searchList.length; j++)
    		{
    			int[][] tempList = searchList[j];
    			
    			for(int i = 0 ; i < tempList.length; i++)
	    		{
	    			int startP = tempList[i][0];
	    			int endP = tempList[i][1];
	    			if(minMoves <= startP && startP <= maxMoves)
	    			{
	    				cantMove[5-(maxMoves-startP)] = true;
	    				
	    				if(board[startP] == 0 && board[endP] == 0)
	    				{
	    					if(isDebug)System.out.print("["+startP+"->"+endP+"]");
		    				
		    				haveToMove.add(endP);
		    				board[endP] = moves;
	    				}
	    			}
	    		}
    		}
    		 
    		// add max move
    		for(int i = cantMove.length - 1 ; i >= 0 ; i--)
    		{
    			if(cantMove[i] == false)
    			{
    				haveToMove.add(i+cur+1);
    				board[i+cur+1] = moves;
    				
    				if(isDebug)System.out.print("("+(i+cur+1)+")");
    				
    				break;
    			}
    		}
    	}
    	
    	if(board[boardGoal] == 0) return -1;
    	    	
    	return board[boardGoal];
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
//        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int t = scanner.nextInt();
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        for (int tItr = 0; tItr < t; tItr++) {
            int n = scanner.nextInt();
            scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

            int[][] ladders = new int[n][2];

            for (int i = 0; i < n; i++) {
                String[] laddersRowItems = scanner.nextLine().split(" ");
                scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

                for (int j = 0; j < 2; j++) {
                    int laddersItem = Integer.parseInt(laddersRowItems[j]);
                    ladders[i][j] = laddersItem;
                }
            }

            int m = scanner.nextInt();
            scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

            int[][] snakes = new int[m][2];

            for (int i = 0; i < m; i++) {
                String[] snakesRowItems = scanner.nextLine().split(" ");
                scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

                for (int j = 0; j < 2; j++) {
                    int snakesItem = Integer.parseInt(snakesRowItems[j]);
                    snakes[i][j] = snakesItem;
                }
            }

            int result = quickestWayUp(ladders, snakes);

//            bufferedWriter.write(String.valueOf(result));
//            bufferedWriter.newLine();
            System.out.println(result);
        }

//        bufferedWriter.close();

        scanner.close();
    }
}

```