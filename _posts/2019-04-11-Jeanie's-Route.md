---
layout: post
title: Jeanie's Route
categories: HackerRank
tags: 
- HackerRank
- Graph
---

## **HackerRank link**
[Jeanie's Route](https://www.hackerrank.com/challenges/jeanies-route/problem)


## **풀이 핵심**
1. 갈 필요 없는 곳을 찾아 낸다.
   - letter를 배달해야 하는 지역은 당연히 가야 한다.
   - letter를 배달하지 않는 지역이라도 갈 필요가 없는 곳이 있다. (leaf node)   
2. 전체 트리의 순회 결과를 계산법을 알아내야 한다.
   - 가장 가중치가 큰 곳은 한 번만 가야 하고, 그 외에는 2번씩 갈 수밖에 없다.
   - 즉, graph의 diameter는 가장 가중치가 큰 노드의 합으로 한번만 이동하고
   - 나머지 노드는 2번씩 이동하므로, 
   - **total weight = edge의 합 * 2 - diameter**가 된다.
4. 어디서 시작을 해야 하는지 찾아내야 한다.   
   - 계산법에 의해 가장 큰 가중치에서 시작해야 하므로,
   - 일단 bfs로 돌려보면서 가장 큰 가중치에서 시작점을 찾을 수 있다.

## **Algorithm**
1. 입력 데이터를 사용하기에 알맞게 가공한다.
2. 갈 필요 없는 곳을 찾아낸다.
   - leaf node의 특징은 node의 차수가 1인 노드임을 이용한다.
   - 모든 노드에대해 검색을 한다.
   - 해당 노드가 letter 도시가 아니고 차수가 1인경우 갈 필요 없다고 표시한다.
   - 갈 필요 없는 노드는 제거 됐으므로, 연결되어 있는 노드의 차수를 1 감소 한다.
3. bfs로 한번 돌려서 어디서 시작해야 하는 지 찾아낸다.
4. bfs로 또 돌려서 최종 가중치의 합과 diameter로 답을 찾아낸다.

## **Source Code**
소스 코드가 엉망.. 나중에 다시 최적화 할 것
```java
import java.io.*;
import java.math.*;
import java.text.*;
import java.util.*;
import java.util.regex.*;


public class Solution {

    public static class Node
    {
        int to;
        int weight;
        
        Node(int to, int weight)
        {
            this.to = to;
            this.weight = weight;
        }
    }
    
    static boolean[] prune(boolean[] letterCity, ArrayList<Node> nodes[])
    {
        boolean[] doNotNeedToGo = new boolean[nodes.length];
        
        int[] degree = new int[nodes.length];
        
        Queue<Integer> checkQue = new LinkedList<>();        
        
        for(int i = 0; i < degree.length; i++)
        {
            degree[i] = nodes[i].size();
        }
        
        // 편지를 보낼 대상이 아니고, 차수(degree)의 수가 1인경우가 갈 필요 없는 곳이다.
        for(int i = 1 ; i < nodes.length ; i++)
        {
            if(letterCity[i] == true) continue;
                    
            if(degree[i] == 1) checkQue.add(i);
        }
        
        while(checkQue.isEmpty() == false)
        {
            int removeTarget = checkQue.poll();
            doNotNeedToGo[removeTarget] = true;
            
            // 주변을 살펴 본다.
            for(int i = 0 ; i < nodes[removeTarget].size() ; i++)
            {
                Node temp = nodes[removeTarget].get(i);
                int next = temp.to;
                if(letterCity[next] == true) continue;
                else if(--degree[next] == 1) 
                {
                    if(doNotNeedToGo[next] == false)
                    {
                        checkQue.add(next);
                    }
                }
            }
        }
        
        return doNotNeedToGo;
    }
    
    static int[] bfs(ArrayList<Node> nodes[], boolean[] doNotNeedToGo, int start)
    {
        int[] weightSum = new int[nodes.length];
        int[] result = new int[3];

        int bestWeightCity = 0;
        int weightTotal = 0;
        
        Queue<Integer> que = new LinkedList<>();
        
        que.add(start);
        
        for(int i = 0 ; i < weightSum.length; i++)
        {
            weightSum[i] = -1;
        }
        
        weightSum[start] = 0;
        bestWeightCity = start;
        
        while(que.isEmpty() == false)
        {
            int cur = que.poll();
            for(Node temp : nodes[cur]) {
                
                // 한번도 안간 곳이고, 갈 필요가 있는 곳이면
                int next = temp.to;
                if(weightSum[next] == -1 && doNotNeedToGo[next] == false)
                {
                    que.add(next);
                    int weight = temp.weight;
                    
                    weightTotal += weight;
                    
                    weightSum[next] = weightSum[cur] + weight;
                    
                    if(weightSum[next] > weightSum[bestWeightCity]) bestWeightCity = next;
                }
            }
        }
        
        result[0] = weightTotal;
        result[1] = weightSum[bestWeightCity];
        result[2] = bestWeightCity;
        
        return result;
    }
    
    /*
     * Complete the jeanisRoute function below.
     */
    static int jeanisRoute(int[] city, int[][] roads) {
        /*
         * Write your code here.
         */
        // 1. 도로 정보를 이용하여, 사용하기 편리한 데이터로 가공
        // 메모리가 아깝지만.. 속도 면에서는 더 빠름.
        // 최소 도로 데이터 보단 도시 수가 적음
        ArrayList<Node> nodes[] = new ArrayList[roads.length+1+1];
        for(int i = 0; i < nodes.length; i++)
        {
            nodes[i] = new ArrayList<>();
        }
        
        for(int i = 0 ; i < roads.length ; i++)
        {
            int from = roads[i][0];
            int to = roads[i][1];
            int weight = roads[i][2];
            nodes[from].add(new Node(to,weight));
            nodes[to].add(new Node(from,weight));
        }
        
        boolean[] letterCity = new boolean[nodes.length];
        
        for(int i = 0 ; i < city.length ; i++ )
        {
            letterCity[city[i]] = true;
        }
        
        // 2. 이제 돌 필요가 없는 도시를 찾는다.
        boolean[] doNotNeedToGo = prune(letterCity, nodes);
        
        // 3. 일단 bfs를 돌리면서 가장 먼곳을 찾는다.
        int result[] = bfs(nodes, doNotNeedToGo, city[0]);
        result = bfs(nodes, doNotNeedToGo, result[2]);
        
        int totalWeight = result[0];
        int diameter = result[1];
        
        return totalWeight * 2 - diameter;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
       BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int n = scanner.nextInt();
        int k = scanner.nextInt();
        int[] city = new int[k];

        for(int i = 0; i < k ; i++) {
            city[i] = scanner.nextInt();
        }
        
        int[][] roads = new int[n-1][3];

        for (int roadsRowItr = 0; roadsRowItr < n-1; roadsRowItr++) {
            for(int i =0 ; i < 3 ; i++) {
                roads[roadsRowItr][i] = scanner.nextInt();
            }
        }

        int result = jeanisRoute(city, roads);
        // System.out.println(result);
       bufferedWriter.write(String.valueOf(result));
       bufferedWriter.newLine();

       bufferedWriter.close();
    }
}

```