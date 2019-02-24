---
layout: post
title: KnightL on a Chessboard
categories: HackerRank
tags: 
- HackerRank
- Breadth First Search
---

## **HackerRank link**
[KnightL on a Chessboard](https://www.hackerrank.com/challenges/knightl-on-chessboard/problem)


## **풀이 핵심**
BFS 알고리즘에 대해 잘 이해하고 있다.  
해당 문제에서 체크 말을 목적지까지 이동 시 소요되는 최단 거리를 원하고 있다.  


## **Algorithm**
함수 두개를 이용하여 구현한다.
1. BFS를 구현한 함수.
2. ab를 각각 1 ~ n-1 까지 증가 시키면서 BFS의 결과를 받는 함수.


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

	static public class Point 
	{
		int x,y;
		Point()
		{
			x = 0;
			y = 0;
		}
		
		Point(int _x, int _y)
		{
			x = _x;
			y = _y;
		}
		
	}
	
	static void printSteps(int[][] steps)
	{
		System.out.println();
		for(int i = 0 ; i < steps.length ; i++)
		{
			for(int j = 0 ; j < steps.length ; j++)
			{
				System.out.print(steps[i][j] + " ");
			}
			System.out.println();
		}
	}
	
	
    // Complete the knightlOnAChessboard function below.
	static int BreathFirstSearch(int n, int a, int b)
	{
		boolean debugPrint = false;
		
		Point p;
		
		Queue<Point> que = new LinkedList<Point>();
		int[][] steps = new int[n][n];
		
		
		int[][] dir = {
				{+a,+b}, {+a,-b}, {-a,+b}, {-a,-b},
				{+b,+a}, {+b,-a}, {-b,+a}, {-b,-a}
		};
		
		if((a == 1) && (b == 1)) return n - 1;
		
		if(a == (n - 1) && b == (n - 1)) return 1;
		
		que.add(new Point());
		steps[0][0] = 1;
		
		while(que.isEmpty() == false)
		{
			p = que.poll();
			
			if(debugPrint) System.out.print("(poll:"+p.x+","+p.y+") >> ");
			
			int curStep = steps[p.x][p.y] + 1;
			
			if(debugPrint) System.out.print("steps:" + curStep + " ");
			
			for(int i = 0 ; i < dir.length ; i++)
			{
				// new point
				int x = p.x + dir[i][0];
				int y = p.y + dir[i][1];
				
				// check limits
				if( x < 0 || y < 0) continue;
				if( x >= n || y >= n) continue;
				
				// check duplicated
				if(steps[x][y] == 0)
				{
					// check goal
					if( x == n - 1 && y == n - 1) 
					{
						steps[x][y] = curStep;
						if(debugPrint) printSteps(steps);
						return curStep - 1;						
					}
						
					steps[x][y] = curStep;
					que.add(new Point(x,y));
					
					if(debugPrint) System.out.print("("+x+","+y+") -> ");
					
				}			
			}
			
			if(debugPrint) System.out.println();
		}
		
		if(debugPrint) System.out.println("\n==================================");
		
		return -1;
	}
	
	
    static int[][] knightlOnAChessboard(int n) {
    	int[][] result = new int[n - 1][n - 1];
    	
    	
    	for(int a = 1 ; a < n ; a++)
    	{
    		for(int b = 1 ; b < n ; b++)
    		{
    			result[a-1][b-1] = BreathFirstSearch(n,a,b);
    		}
    	}
    	
    	return result;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        // BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int n = scanner.nextInt();
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        int[][] result = knightlOnAChessboard(n);

        for (int i = 0; i < result.length; i++) {
            for (int j = 0; j < result[i].length; j++) {
                // bufferedWriter.write(String.valueOf(result[i][j]));
            	System.out.print(String.valueOf(result[i][j]));

                if (j != result[i].length - 1) {
                    // bufferedWriter.write(" ");
                	System.out.print(" ");
                }
            }

            if (i != result.length - 1) {
                // bufferedWriter.write("\n");
            	System.out.print("\n");
            }
        }

        // bufferedWriter.newLine();

        // bufferedWriter.close();

        scanner.close();
    }
}
```