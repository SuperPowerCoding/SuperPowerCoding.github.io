---
layout: post
title: Breadth First Search: Shortest Reach
categories: HackerRank
tags: 
- HackerRank
- Breadth First Search
---

## **HackerRank link**
[Breadth First Search: Shortest Reach](https://www.hackerrank.com/challenges/bfsshortreach/problem)


## **풀이 핵심**
BFS 알고리즘에 대해 잘 이해하고 있다.  
단순 BFS 구현 문제


## **Algorithm**
BFS를 구현한다.


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

    // Complete the bfs function below.
	/*
	 * n: the integer number of nodes
	 * m: the integer number of edges
	 * edges: a 2D array of start and end nodes for edges
	 * s: the node to start traversals from
	 */
    static int[] bfs(int n, int m, int[][] edges, int s) {
    	boolean debugPrint = true;
    	// edge data    	
    	ArrayList<Integer>[] edge = new ArrayList[n+1];
    	for(int i = 0 ; i < edge.length ; i++)
    	{
    		edge[i] = new ArrayList<>();
    	}
    	
    	for(int i = 0; i < edges.length; i++)
    	{
    		edge[edges[i][0]].add(edges[i][1]);
    		edge[edges[i][1]].add(edges[i][0]);
    	}
    	
    	int[] moved = new int[n+1];
    	Queue<Integer> que = new LinkedList<Integer>();
    	
    	que.add(s);
    	moved[s] = 0;
    	
    	while(que.isEmpty() == false)
    	{
    		int cur = que.poll();
    		int curStep = moved[cur] + 1;
    		
    		if(debugPrint) System.out.print("("+cur+") ");
    		
    		for(int i = 0 ; i < edge[cur].size() ; i++)
    		{
    			int temp = edge[cur].get(i);
    			if(moved[temp] == 0)
    			{
    				que.add(temp);
    				moved[temp] = curStep;
    				if(debugPrint) System.out.print(" "+temp+" -> ");
    			}
    		}
    		
    		if(debugPrint) System.out.println();
    		
    	}
    	
    	int[] result = new int[n-1];
    	int idx = 0;
    	for(int i = 1 ; i < moved.length; i++)
    	{
    		if((i) != s )
    		{
    			result[idx] = moved[i] == 0 ? -1 : (moved[i]) * 6;
    			idx++;
    		}
    	}
    	
    	return result;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
//        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int q = scanner.nextInt();
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        for (int qItr = 0; qItr < q; qItr++) {
            String[] nm = scanner.nextLine().split(" ");

            int n = Integer.parseInt(nm[0]);

            int m = Integer.parseInt(nm[1]);

            int[][] edges = new int[m][2];

            for (int i = 0; i < m; i++) {
                String[] edgesRowItems = scanner.nextLine().split(" ");
                scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

                for (int j = 0; j < 2; j++) {
                    int edgesItem = Integer.parseInt(edgesRowItems[j]);
                    edges[i][j] = edgesItem;
                }
            }

            int s = scanner.nextInt();
            scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

            int[] result = bfs(n, m, edges, s);

            for (int i = 0; i < result.length; i++) {
//                bufferedWriter.write(String.valueOf(result[i]));
            	System.out.print(String.valueOf(result[i]));

                if (i != result.length - 1) {
//                    bufferedWriter.write(" ");
                	System.out.print(" ");
                }
            }

//            bufferedWriter.newLine();
            System.out.println();
        }

//        bufferedWriter.close();

        scanner.close();
    }
}

```