---
layout: post
title: Jack goes to Rapture
categories: HackerRank
tags: 
- HackerRank
- Prim's MST
- Kruskal MST
- Mininal Spanning Tree
---

## **HackerRank link**
[Jack goes to Rapture](https://www.hackerrank.com/challenges/jack-goes-to-rapture/problem)


## **풀이 핵심**
문제에서는 처음과 마지막까지의 모든 SubTree 중  
SubTree의 최댓값 중 최솟값을 요구하고 있다.  

먼저, 최솟값을 요구 하고 있기 때문에, SubTree의 최솟값인 MST을   
Prim's 또는 Kruskal 알고리즘을 통해 구할 수 있고,  

이 SubTree에서의 최댓값을 구하면 된다.

두 알고리즘은 모두 Greedy 알고리즘으로,   
Kruskal은 최솟값의 간선을 계속 이어 붙이는 방법이고,  
Prim's는 아무데나 시작해서 현재에서의 최솟값의 간선을 이어가는 방법이다.

## **Algorithm**
[Kruskal MST]
1. 일단 노드를 모두 우선순위 큐에 넣는다.
2. 순서데로 큐에서 값을 뺀다.
3. 해당 노드에서 시작점과 끝점을 연결을 시도한다.
4. 시작점과 끝점의 root가 같으면, 이미 연결되어 있으므로 패스한다.
5. root가 다르다면, 서로 연결한다. (이때 최댓값을 갱신한다.)
6. 끝점에 도달했다면, 종료한다.
7. 2~6과정을 큐에서 데이터가 없을 때까지 반복한다.

[Prim's MST]
1. 아무데다 시작한다. (여기서는 1로 지정)
2. 우선순위큐에 이미 간곳을 제외하고, 현재로부터 갈 수 있는 모든 노드를 추가한다.
3. 우선순위 큐에서 최솟값의 간선값을 갖는 노드를 가져온다.
4. 이전값과 비교하여 더 큰값으로 갱신한다. (최댓값 갱신)
5. 끝점에 도달했다면, 종료한다.
6. 그렇지 않다면, 다음 노드를 검색하며 과정 2~5를 반복한다.

## **Source Code**
```java
[Kruskal MST]
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.function.*;
import java.util.regex.*;
import java.util.stream.*;
import static java.util.stream.Collectors.joining;
import static java.util.stream.Collectors.toList;

class Result {

    /*
     * Complete the 'getCost' function below.
     *
     * The function accepts WEIGHTED_INTEGER_GRAPH g as parameter.
     */

    /*
     * For the weighted graph, <name>:
     *
     * 1. The number of nodes is <name>Nodes.
     * 2. The number of edges is <name>Edges.
     * 3. An edge exists between <name>From[i] and <name>To[i]. The weight of the edge is <name>Weight[i].
     *
     */
    public static class Node implements Comparable<Node>{
        int from;
        int to;
        int weight;
        Node(int from, int to, int weight){
            this.from = from;
            this.to = to; 
            this.weight = weight;
        }
        @Override
        public int compareTo(Node arg0) {
            // TODO Auto-generated method stub
            return this.weight <= arg0.weight ? -1 : 1;
        }
    }
    
    public static int find(int[] parents, int a) {
        
        if(a == parents[a]) return a;        
        
        parents[a] = find(parents, parents[a]);
        
        return parents[a];
        
    }
    
    public static void union(int[] parents, int a, int b) {
        if(a == b )return;
        
        int rootA = find(parents, a);
        int rootB = find(parents, b);
        
        parents[rootB] = rootA;
    }
    

    public static void getCost(int gNodes, List<Integer> gFrom, List<Integer> gTo, List<Integer> gWeight) {
    // Print your answer within the function and return nothing
        Iterator[] it = new Iterator[3];
        it[0] = gFrom.iterator();
        it[1] = gTo.iterator();
        it[2] = gWeight.iterator();
        
        PriorityQueue<Node> minWeightQue = new PriorityQueue<>();
        
        while(it[0].hasNext())
        {
            int from = (int)it[0].next();
            int to= (int)it[1].next();
            int weight= (int)it[2].next();
            
            minWeightQue.add(new Node(from, to , weight));            
        }
        
        int[] parents = new int[gNodes + 1];
        int[] minDistance = new int[gNodes + 1];
        for(int i = 0; i < parents.length; i++) {
            parents[i] = i;            
        }
                 
        // int index = 0;
        int max = 0;
        
        while(minWeightQue.isEmpty() == false) {
            Node temp = minWeightQue.poll();
            
            int to = temp.to;
            int from = temp.from;
            
            int rootTo = find(parents, to);
            int rootFrom = find(parents, from);
            
            if(rootTo == rootFrom) continue;
            
            union(parents, rootTo, rootFrom);
            
            // minDistance[index++] = temp.weight;
            if(temp.weight > max) max = temp.weight;
            
            if(find(parents, 1) == find(parents, gNodes)) break; 
        }
        
        
        
        if(find(parents, 1) != find(parents, gNodes)) {
            System.out.println("NO PATH EXISTS");
            return;
        }
        
        /*
        for(int i = 0; i < index ; i++)
        {
            if(minDistance[i] > max) max = minDistance[i];
        }
        */
        System.out.println(max);
        
        
        
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));

        String[] gNodesEdges = bufferedReader.readLine().replaceAll("\\s+$", "").split(" ");

        int gNodes = Integer.parseInt(gNodesEdges[0]);
        int gEdges = Integer.parseInt(gNodesEdges[1]);

        List<Integer> gFrom = new ArrayList<>();
        List<Integer> gTo = new ArrayList<>();
        List<Integer> gWeight = new ArrayList<>();

        IntStream.range(0, gEdges).forEach(i -> {
            try {
                String[] gFromToWeight = bufferedReader.readLine().replaceAll("\\s+$", "").split(" ");

                gFrom.add(Integer.parseInt(gFromToWeight[0]));
                gTo.add(Integer.parseInt(gFromToWeight[1]));
                gWeight.add(Integer.parseInt(gFromToWeight[2]));
            } catch (IOException ex) {
                throw new RuntimeException(ex);
            }
        });

        Result.getCost(gNodes, gFrom, gTo, gWeight);

        bufferedReader.close();
    }
}

[prims]
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.function.*;
import java.util.regex.*;
import java.util.stream.*;
import static java.util.stream.Collectors.joining;
import static java.util.stream.Collectors.toList;

class Result {

    /*
     * Complete the 'getCost' function below.
     *
     * The function accepts WEIGHTED_INTEGER_GRAPH g as parameter.
     */

    /*
     * For the weighted graph, <name>:
     *
     * 1. The number of nodes is <name>Nodes.
     * 2. The number of edges is <name>Edges.
     * 3. An edge exists between <name>From[i] and <name>To[i]. The weight of the edge is <name>Weight[i].
     *
     */
    public static class Node implements Comparable<Node>{
        int to;
        int weight;
        Node(int to, int weight){
            this.to = to; this.weight = weight;
        }
        @Override
        public int compareTo(Node arg0) {
            // TODO Auto-generated method stub
            return this.weight <= arg0.weight ? -1 : 1;
        }
    }
    
    
    public static void getCost(int gNodes, List<Integer> gFrom, List<Integer> gTo, List<Integer> gWeight) {
    // Print your answer within the function and return nothing
        ArrayList<Node> nodes[] = new ArrayList[gNodes+1];
        for(int i = 0; i < nodes.length; i++) {
            nodes[i] = new ArrayList<>();
        }
        
        Iterator[] it = new Iterator[3];
        it[0] = gFrom.iterator();
        it[1] = gTo.iterator();
        it[2] = gWeight.iterator();
        
        while(it[0].hasNext())
        {
            int from = (int)it[0].next();
            int to= (int)it[1].next();
            int weight= (int)it[2].next();
            
            nodes[to].add(new Node(from, weight));
            nodes[from].add(new Node(to, weight));
        }
        
        Queue<Integer> haveToGo = new LinkedList<>();
        PriorityQueue<Node> minWeightQue = new PriorityQueue<>();
        haveToGo.add(1);
        int[] isMoved = new int[gNodes+1];
        isMoved[1] = -1;
        int maxWeight = 0;
        
        while(haveToGo.isEmpty() == false) {
            int cur = haveToGo.poll();
            
            for(int i = 0; i < nodes[cur].size(); i++) {
                
                Node next = nodes[cur].get(i);
                int to = next.to;
                int weight = next.weight;
                
                // 1. 안가본 곳이라면,
                if(isMoved[to] == 0) {
                    minWeightQue.add(new Node(to, weight));                    
                }
            }
            
            // 2. 최소치를 본다.            
            while(minWeightQue.isEmpty() == false) {
                Node minNode = minWeightQue.poll();
                int to = minNode.to;
                int weight = minNode.weight;
                
                // 안가본 곳이라면,                 
                if(isMoved[to] == 0) {
                    
                    int newWeight;
                    // 간선 가중치를 갱신한다 :              
                    if(isMoved[cur] == 0) {    // 최초 갱신
                        newWeight = weight;
                    } else {    // 지난것과 비교 후 최대치
                        newWeight = weight > isMoved[cur] ? weight : isMoved[cur];
                    }
                    
                    isMoved[to] = newWeight;
                    
                    if(maxWeight < newWeight) maxWeight = newWeight;
                    
                    //다음 갈 곳을 지정하고,
                    if(to == gNodes) {
                        break;
                    } else {
                        haveToGo.add(to);
                    }
                    
                    break;
                }
            }            
        }
        
        if(isMoved[gNodes] == 0) {
            System.out.println("NO PATH EXISTS");
        } else {
            System.out.println(isMoved[gNodes]);
        }
        
    }
}
public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));

        String[] gNodesEdges = bufferedReader.readLine().replaceAll("\\s+$", "").split(" ");

        int gNodes = Integer.parseInt(gNodesEdges[0]);
        int gEdges = Integer.parseInt(gNodesEdges[1]);

        List<Integer> gFrom = new ArrayList<>();
        List<Integer> gTo = new ArrayList<>();
        List<Integer> gWeight = new ArrayList<>();

        IntStream.range(0, gEdges).forEach(i -> {
            try {
                String[] gFromToWeight = bufferedReader.readLine().replaceAll("\\s+$", "").split(" ");

                gFrom.add(Integer.parseInt(gFromToWeight[0]));
                gTo.add(Integer.parseInt(gFromToWeight[1]));
                gWeight.add(Integer.parseInt(gFromToWeight[2]));
            } catch (IOException ex) {
                throw new RuntimeException(ex);
            }
        });

        Result.getCost(gNodes, gFrom, gTo, gWeight);

        bufferedReader.close();
    }
}
```