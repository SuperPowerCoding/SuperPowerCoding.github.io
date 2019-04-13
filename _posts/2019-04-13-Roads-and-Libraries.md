---
layout: post
title: Roads and Libraries
categories: HackerRank
tags: 
- HackerRank
- Graph
---

## **HackerRank link**
[Roads and Libraries](https://www.hackerrank.com/challenges/torque-and-development/problem)


## **풀이 핵심**
문제에서 요구하는 모든 도시에서 도서관을 가는 방법에는

방법 1 : 도서관 하나와 도로를 수리한다.  
방법 2 : 모든 도시에 도서관을 짓는다.

각 방법의 
  - 방법 1: 총 도로의 수 * 도로 수리비용 + 도서관 1개 짓는 비용  
  - 방법 2: 도서관 1개를 짓는 비용 * 도시 수

둘중 더 코스트가 적은 쪽을 선택한다.

## **Algorithm**
1. 입력을 사용하기 편하게 가공한다.
2. 이미 이동했던 도시가 아니라면, bfs를 통해 간선(edge)의 수를 측정한다.
3. 방법 1 과 방법 2 중 코스트가 더 적은 쪽을 선택한다.
4. 모든 도시의 코스트를 측정할때까지 2-3을 반복한다.

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

    static long bfs(ArrayList<Integer> nodes[], int start, boolean[] isMoved) {
        long totalWeight = 0;
        Queue<Integer> haveToMove = new LinkedList<>();
        
        haveToMove.add(start);
        isMoved[start] = true;
        
        while(haveToMove.isEmpty() == false) {
            int cur = haveToMove.poll();
            
            
            for(int i = 0; i < nodes[cur].size(); i++) {
                int next = nodes[cur].get(i);
                if(isMoved[next] == false) {                    
                    haveToMove.add(next);
                    isMoved[next] = true;
                    totalWeight++;
                }
            }
        }
        
        return totalWeight;
    }
    
    
    // Complete the roadsAndLibraries function below.
    static long roadsAndLibraries(int n, int c_lib, int c_road, int[][] cities) {
        ArrayList<Integer> nodes[] = new ArrayList[n+1];
        for(int i = 0 ; i <= n ;  i++) {
            nodes[i] = new ArrayList<>();
        }
        
        for(int i = 0; i < cities.length; i++)
        {
            int from = cities[i][0];
            int to = cities[i][1];
            
            nodes[from].add(to);
            nodes[to].add(from);
        }
        
        boolean[] isMoved = new boolean[n+1];
        
        long totalWeight = 0;
        
        for(int i = 1 ; i <= n ; i++)
        {
            if(isMoved[i] == false)
            {
                long weight = bfs(nodes, i, isMoved);
                
                long roadCost = weight * (long)c_road + (long) c_lib;
                long cityCost = (long) c_lib * (weight + 1);
                
                totalWeight += roadCost > cityCost ? cityCost : roadCost;
            }
        }
        
        
        return totalWeight;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int q = scanner.nextInt();
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        for (int qItr = 0; qItr < q; qItr++) {
            
            int n = scanner.nextInt();
            int m = scanner.nextInt();
            int c_lib = scanner.nextInt();
            int c_road = scanner.nextInt();

            int[][] cities = new int[m][2];

            for (int i = 0; i < m; i++) {
                scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

                for (int j = 0; j < 2; j++) {
                    int citiesItem = scanner.nextInt();
                    cities[i][j] = citiesItem;
                }
            }

            long result = roadsAndLibraries(n, c_lib, c_road, cities);

            bufferedWriter.write(String.valueOf(result));
            bufferedWriter.newLine();
        }

        bufferedWriter.close();

        scanner.close();
    }
}
```