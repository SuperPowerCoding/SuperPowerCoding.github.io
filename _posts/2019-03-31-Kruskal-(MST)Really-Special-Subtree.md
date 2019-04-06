---
layout: post
title: Kruskal (MST) Really Special Subtree
categories: HackerRank
tags: 
- HackerRank
- Kruskal MST
---

## **HackerRank link**
[Kruskal (MST): Really Special Subtree](https://www.hackerrank.com/challenges/kruskalmstrsub/problem)


## **풀이 핵심**
Kruskal 알고리즘에 대해 잘 이해하고 있다.  
해당 문제는 그래프에서 subtree 중 최소 가중치를 원하고 있다.  
Kruskal 알고리즘을 잘 알고 있는 경우 쉽게 풀린다.

※ MST(Minimal Spanning Tree : 최소 신장 트리) 알고리즘은 대표적으로 Kruskal MST와 Prim's MST가 있다.

둘다 Greedy 알고리즘의 일종이지만,   
Prim's 알고리즘은 트리를 greedy하게 만들고(계속 이어 붙임),  
Kruskal은 엣지를 greedy하게 Tree를 만든다(만들다가 이어 붙임).


## **Algorithm**
1. 입력을 우선순위 큐를 이용하여 정렬한다.
2. 모든 노드에 대해 검색을 한다.
3. 우선순위 큐에서 노드를 받는다. (최소의 가중치)
4. 트리가 그래프가 되지 않도록 한다.  
   4-1. 최상위 부모 노드를 파악한다. (find)  
   4-2. 최상위 부모 노드가 같다면, 연결이 되므로 패스  
   4-3. 같지 않다면 연결한다. (union)

아래 코드는 해당 문제를 kruskal과 prim's 모두 이용해 풀었는데..  
항상 주의 할 것이 입력 조건에서 간선의 weight 조건이.. 0이 포함되어 있는지 꼭 확인 할것...  
prim's 알고리즘 구현하면서 처음에 ArrayList가아닌 배열로 구현하면서 무게 조건이 0인경우 패스라고 했다가 큰 낭패를 봤었음.....

## **Source Code**
```java
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.regex.*;

class Result {

    static public class Node implements Comparable<Node>
    {
        int start;
        int end;
        int weight;
        
        Node(int start, int end, int weight)
        {
            super();
            this.start = start;
            this.end = end;
            this.weight = weight;
        }

        @Override
        public int compareTo(Node o) {
            // TODO Auto-generated method stub
            return  o.weight >= this.weight ? -1 : 1; 
        }
    }

    public static int prim(int gNodes, List<Integer> gFrom, List<Integer> gTo, List<Integer> gWeight) {
        int minWeight = 0;
    
        // map 만들기
        ArrayList<Node> map[] = new ArrayList[gNodes+1];
        
        for(int i = 0 ; i < map.length; i++)
        {
            map[i] = new ArrayList<>();
        }
        
        Iterator[] it = new Iterator[3];
        it[0] = gFrom.iterator();
        it[1] = gTo.iterator();
        it[2] = gWeight.iterator();
        
        while(it[0].hasNext())
        {
            int start = (int)it[0].next();
            int end = (int)it[1].next();
            int weight = (int)it[2].next();
            
            map[start].add(new Node(start,end,weight));
            map[end].add(new Node(end,start,weight));
        }
        
        PriorityQueue<Node> minWeigtQue = new PriorityQueue<>();
        Queue<Integer> haveToSearch = new LinkedList<>(); 
        boolean[] isMoved = new boolean[gNodes+1];
        
        haveToSearch.add(1);
                    
        while(haveToSearch.isEmpty() == false)
        {
            // 서치 큐에서 현재 서치를 해야 하는 시작점을 받아 온다.
            int start = haveToSearch.poll();
            ArrayList tempList = map[start];
            
            // 이동한 곳은 이동했다는 표시를 한다.
            isMoved[start] = true;
            
            // 이미 이동했던 곳을 제외하고 현재 노드에서의 이동 가능한 모든 경로를 찾는다.
            for(int i = 0 ; i < tempList.size() ; i++)
            {                
                Node tempNode = (Node)tempList.get(i);
                
                // 이동안한곳이라면..
                if(isMoved[tempNode.end] == false)
                {
                    // if(debugPrint) System.out.print("("+i+")");
                    // 만약 있다면, 우선순위 큐에 넣는다.
                    minWeigtQue.add(tempNode);
                }            
            }
            
            // 모두 다 넣었으면, 이중 가장 최단길을 우선순위 큐에서 뽑아 낸다.                
            while(minWeigtQue.isEmpty() == false)
            {
                Node temp = minWeigtQue.poll();
                int next = temp.end;
                
                // 중복을 방지하기 위해서, 이미 이동한 값은 무시한다.
                if(isMoved[next] == true) continue;
                
                minWeight += temp.weight;
                // if(debugPrint) System.out.println("->"+temp.end+"("+temp.weight+")");
                if(debugPrint) System.out.println(temp.weight);
                
                haveToSearch.add(next);
                break;
            }
        }

        return minWeight;
    }
    /*
     * Complete the 'kruskals' function below.
     *
     * The function is expected to return an INTEGER.
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
    static boolean debugPrint = false;
    
    public static void printParents(int[] parents)
    {
        if(debugPrint == false) return;
        
        for(int i = 0 ; i < parents.length; i++)
        {
            System.out.print(i+":"+parents[i]+"->");            
        }
        System.out.println();
    }
    
    // 부모 노드에 대해 탐색한다.
    public static int find(int[] parents, int a)
    {
        if(parents[a] == a) 
        {
            return a;
        }
        else
        {
            parents[a] =  find(parents, parents[a]);    // 검색할때마다 최상위 부모를 다시 설정. ( 검색 시 마다의 성능 향상을 위함)
            return parents[a];
        }
    }
    
    // 합친다.
    public static void union(int[] parents, int a, int b)
    {
        int rootA = find(parents, a);
        int rootB = find(parents, b);
        
        // 각각의 루트가 같은 경우 합칠 필요가 없음.
        if( rootA == rootB ) return;
        
        // 다르면 재 설정한다.
        parents[rootA] = rootB;
    }
    
    public static int kruskals(int gNodes, List<Integer> gFrom, List<Integer> gTo, List<Integer> gWeight) {
        int minWeight = 0;
        
        // 1. 데이터를 넣으면서 바로바로 가중치가 낮은 큐를 우선으로 하기 위해 우선순위 큐를 만든다.
        PriorityQueue<Node> que = new PriorityQueue<>();
        
        Iterator<Integer> it[] = new Iterator[3]; 
        it[0] = gFrom.iterator();
        it[1] = gTo.iterator();
        it[2] = gWeight.iterator();
        
        while(it[0].hasNext())
        {
            int start = it[0].next();
            int end = it[1].next();
            int weight = it[2].next();
            que.add(new Node(start, end , weight));
        }
        
        int nodes = que.size() + 1;
        
        // 부모 노드는 일단 연결하기 전이기 때문이니 자기 자신으로 지정.
        int[] parents = new int[nodes];
        for(int i = 0; i < parents.length; i++)
        {
            parents[i] = i;
        }
        
        // 모든 노드에 대해 검색 한다.        
        for(int i = 0 ; i < nodes - 1 ; i++)
        {
            printParents(parents);
            // 최소가 되는
            Node node = que.poll();
            int start = node.start;
            int end = node.end;
            
            // 서브 트리의 연결 시 연결이 되어 그래프 구조가 되면 안되기 때문에, 
            // 최상위 부모가 같은지 체크 한다.
            int rootStart = find(parents, start);
            int rootEnd = find(parents, end);
            
            if(debugPrint) System.out.print(start+"("+rootStart+")"+"->"+end+"("+rootEnd+")");
            
            // 그리고 부모가 같은 경우는 서로 연결되어 있는것이 아니므로, 다음으로 넘어간다. 
            if( rootStart == rootEnd ) 
            {
                System.out.println();
                continue;
            }
            
            if(debugPrint) System.out.println(":"+node.weight);
            
            // 부모가 서로 다른경우에는 서로 합친다. ( 부모를 같이 한다. ex) 3 -> 4로 이동하는 경우 3과 4 모두 최상위 노드가 
            minWeight += node.weight;
            union(parents, start, end);
        }        
        
        return minWeight;
    }


}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] gNodesEdges = bufferedReader.readLine().replaceAll("\\s+$", "").split(" ");

        int gNodes = Integer.parseInt(gNodesEdges[0]);
        int gEdges = Integer.parseInt(gNodesEdges[1]);

        List<Integer> gFrom = new ArrayList<>();
        List<Integer> gTo = new ArrayList<>();
        List<Integer> gWeight = new ArrayList<>();

        for (int i = 0; i < gEdges; i++) {
            String[] gFromToWeight = bufferedReader.readLine().replaceAll("\\s+$", "").split(" ");

            gFrom.add(Integer.parseInt(gFromToWeight[0]));
            gTo.add(Integer.parseInt(gFromToWeight[1]));
            gWeight.add(Integer.parseInt(gFromToWeight[2]));
        }

        //int res = Result.kruskals(gNodes, gFrom, gTo, gWeight);
        int res = Result.prim(gNodes, gFrom, gTo, gWeight);        

        // Write your code here.
        bufferedWriter.write(String.valueOf(res));
        // System.out.println(res);

        bufferedReader.close();
        bufferedWriter.close();
    }
}

```