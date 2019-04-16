---
layout: post
title: Prims (MST) : Special Subtree
categories: HackerRank
tags: 
- HackerRank
- Prim's MST
- Kruskal MST
- Mininal Spanning Tree
---

## **HackerRank link**
[Prim's (MST) : Special Subtree](https://www.hackerrank.com/challenges/primsmstsub/problem)


## **풀이 핵심**
Prim's 알고리즘에 대해 잘 이해하고 있다.  
해당 문제는 그래프에서 subtree 중 최소 가중치를 원하고 있다.  
Prim's 알고리즘을 잘 구현하면 된다.

※ MST(Minimal Spanning Tree : 최소 신장 트리) 알고리즘은 대표적으로 Kruskal MST와 Prim's MST가 있다.

둘다 Greedy 알고리즘의 일종이지만,   
Prim's 알고리즘은 트리를 greedy하게 만들고(계속 이어 붙임),  
Kruskal은 엣지를 greedy하게 Tree를 만든다(만들다가 이어 붙임).


## **Algorithm**
노드 추가 시 최솟값을 쉽게 구하기 위한 우선순위 큐를 이용  
1. 서치를 해야 하는 지점에서 이미 이동한 곳을 제외하고 우선순위 큐에 Node를 추가한다.
2. 이미 이동한 노드가 아니라면, 우선순위 큐에서 최솟값의 노드를 가져오고 더한다.
3. 1과2를 더 이상 서치 할 수 없을때까지 반복한다.

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
    public static class Node implements Comparable<Node>{
        int start;
        int end;
        int weight;
        
        Node(int start, int end, int weight)
        {
            this.start = start;
            this.end = end;
            this.weight = weight;
        }

        @Override
        public int compareTo(Node arg0) {
            // TODO Auto-generated method stub
            return arg0.weight >= this.weight ? -1 : 1; 
        }
    }
    
static int find(int[] parents, int node)
    {
        int root;
        
        if(parents[node] == node) return node;
        
        parents[node] = find(parents, parents[node]); // 찾을 때 마다 성능 개선
            
        return parents[node];
    }
    
    static void union(int[] parents, int nodeA, int nodeB)
    {
        int rootA = find(parents, nodeA);
        int rootB = find(parents, nodeB);
        
        parents[rootB] = rootA;
    }
    
    
    static int kruskal(int n, int[][] edges, int start) {
        int minWeight = 0;
        
        PriorityQueue<Node> minWeightQue = new PriorityQueue<>();
        int[] parents = new int[n+1];
        
        // 우선 순위 큐에 모든 데이터를 넣어둠.
        for(int i =0 ; i < edges.length; i++) 
        {
            int from = edges[i][0];
            int to = edges[i][1];
            int weight = edges[i][2];
            
            minWeightQue.add(new Node(from,to,weight));
        }
        
        // 각 노드의 부모는 각자로 초기화
        for(int i = 0; i < parents.length; i++)
        {
            parents[i] = i;
        }
        
        // 큐가 빌때까지 탐색
        while(minWeightQue.isEmpty() == false)
        {
            // 1. 큐에서 빼옴.
            Node tempNode = minWeightQue.poll();
            
            // 2. 이제 이어야 되는데 두 노드가 이미 이어져 있는 경우는 안되니까 패스
            int rootA = find(parents, tempNode.start);
            int rootB = find(parents, tempNode.end);
            
            if(rootA == rootB) continue;
            
            // 두개 이어 주고 (root를 맞춤)
            union(parents,tempNode.start, tempNode.end);
            
            minWeight += tempNode.weight;
        }
        
        return minWeight;
    }

    // Complete the prims function below.
     static int prims(int n, int[][] edges, int start) {
        int minWeight = 0;

        ArrayList<Node> nodeLists[] = new ArrayList[n + 1];
        for(int i = 0; i < nodeLists.length; i++)
        {
            nodeLists[i] = new ArrayList<>();
        }
        
        // 탐색을 위한 리스트 생성
        for(int i =0 ; i < edges.length; i++) 
        {
            int from = edges[i][0];
            int to = edges[i][1];
            int weight = edges[i][2];
            
            nodeLists[from].add(new Node(from,to,weight));
            nodeLists[to].add(new Node(to,from,weight));
        }
        
        // 탐색 및 최솟값
        
        PriorityQueue<Node> minWeightQue = new PriorityQueue<>();    // 현재 탐색한 곳에서의 최소 값을 알아내기 위한 우선순위 큐 생성
        Queue<Integer> haveToSearch = new LinkedList<>();             // 탐색을 해야하는 곳을 위한 큐 생성. (사실.. 큐가 아니여도 괘낞아 보임)
        boolean[] isMoved = new boolean[n+1];                        // 이동완료를 알기 위한 flag
        
        // 처음 위치는 start로 지정되어 있으므로, start로
        haveToSearch.add(start);
                
        // 더이상 서치 할 곳이 없으면 종료
        while(haveToSearch.isEmpty() == false)
        {
            // 서치 해야 하는 곳과 리스트를 받아 오고
            int curNode = haveToSearch.poll();            
            ArrayList tempList = nodeLists[curNode];
            
            // 현재 노드에서 탐색을 시작, 여긴 이동 했으니 이동했다고 표시
            isMoved[curNode] = true;
            
            for(int i = 0; i < tempList.size(); i++)
            {
                Node tempNode = (Node)tempList.get(i);
                // 이미 탐색을 하지 않는 곳이라면 추가를 한다. 
                if(isMoved[tempNode.end] == false)
                {
                    minWeightQue.add(tempNode);
                }                                
            }
            
            // 탐색을 다 했다면, 현재에서의 최솟값을 받아 온다.
            while(minWeightQue.isEmpty() == false)
            {
                Node tempNode = minWeightQue.poll();
                int next = tempNode.end;
                // 이미 갔던 곳이 아니라면 ( 중복 방지) 최솟값을 추가 한다.
                if(isMoved[next] == false)
                {
                    minWeight += tempNode.weight;
                    
                    // 그리고 다음 탐색지에 여기를 추가한다.
                    haveToSearch.add(next);
                    
                    break;
                }
            }
            
        }
        
        return minWeight;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] nm = scanner.nextLine().split(" ");

        int n = Integer.parseInt(nm[0]);

        int m = Integer.parseInt(nm[1]);

        int[][] edges = new int[m][3];

        for (int i = 0; i < m; i++) {
            String[] edgesRowItems = scanner.nextLine().split(" ");
            scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

            for (int j = 0; j < 3; j++) {
                int edgesItem = Integer.parseInt(edgesRowItems[j]);
                edges[i][j] = edgesItem;
            }
        }

        int start = scanner.nextInt();
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        // int result = prims(n, edges, start);
        int result = kruskal(n, edges, start);        

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedWriter.close();

        scanner.close();
    }
}
```