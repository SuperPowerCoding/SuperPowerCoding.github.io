---
layout: post
title: Minimum Penalty Path
categories: HackerRank
tags: 
- HackerRank
- Graph Theory
---

## **HackerRank link**
[Minimum Penalty Path](https://www.hackerrank.com/challenges/beautiful-path/problem)

## **풀이 과정**

OR연산의 가장 큰 특징은 꼭 작은 수로만 or를 한다고 제일 작은 수라고 보장이 되지 않는 점이다.

4 or 2 = 6 이지만, 4 or 4 = 4이다.  

아래와 같은 입력이 들어 온 경우
```
4 5 
1 2 1 
1 2 2 
2 3 10 
3 4 2 
3 4 1 
1 4 
답: 10
```
이 때문에 다익스트라 알고리즘을 이용하여 풀려고 하는 경우 풀 수가 없다.

그러면 일반 BFS나 DFS를 이용하여 그래프를 탐색하면서 다음 수가 최소 인지 알 수 있수 있을까?

굉장히 어려워 보인다. 왜냐하면 뒤에 추가적으로 나올 수가 무엇인지도 모르기 때문이다.

그럼 결론은 전부 다 검색해 봐야 한다.

그런데 여기서 문제가 있다. 제약 조건에서 N보다 M이 훨등히 많다. 

즉, 노드의 수 보다 엣지의 수가 월등히 많다. 따라서, 효율적으로 서치를 해야 한다.

BFS나 DFS 모두 이용해도 답은 나올 것이다 왜냐하면 모든 엣지를 서치 하기만 하면 되기 때문이다.

문제는 이 문구 이다. 

> Loops and multiple edges are allowed.

모든 경우의 서치를 해야 하는데 다수의 엣지가 허용 되기 때문이 일반적인 서치를 통해서는 엄청나게 많은 수가 나올 것이다.

그런데 OR 연산의 경우 특이하게도 중복이 굉장히 많이 될 가능성이 있다.

ex) 6 or 2 = 6, 6 or 4 = 6, 6 or 6 = 6

즉, 앞에서 2, 4, 6 등이었고 다음이 또 6이었다면 앞에 2 -> 6, 4 -> 6, 6 -> 6 3가지 모두 할 필요없는 것이다.

그리고 node의 최댓값은 1~1023이다. 

이를 이용하여 각 노드를 1024개의 배열로써 표시하면 최대한 빠르게 서치 가능하다.

```java
boolean[][] isMoved = new boolean[n+1][1024];
```


## 다른 풀이 ##
인상 적인 풀이로는 
> [nbleier3](https://www.hackerrank.com/nbleier3?hr_r=1) : [코드](https://www.hackerrank.com/challenges/beautiful-path/submissions/code/72924204)

풀이의 핵심 이동할 필요 없는 엣지를 지운다는 것이다.

만약 256보다 큰 엣지는 이동하지 않는다라고 하고, BFS를 이용하여 서치를 한다.

만약 도착을 못 했다면, 엣지의 크기가 256인 엣지는 무조건 이동 시 필요한 엣지이다.

or연산이고, 엣지의 크기가 최대 1023이므로 최대 10번만 루프를 돌리면 된다.

**알고리즘**
1. 이동 관련 데이터를 만든다.
2. 일단 BFS로 한번 서치 해본다.
3. 만약 도달하지 못 헀다면, 답이 없다고 리턴
4. 512보다 큰 엣지에 대하여 이동하지 않고 탐색해 본다.
5. 만약 도달하지 못 했다면, 512보다 큰 엣지는 512를 뺀 상태로 이동 관련 데이터를 갱신한다. (다음 번 서치에 이동 가능해야 하므로)
6. 다음 512를 2를 나누고 다시 시도한다.
7. 과정 4-7를 a가 0보다 큰 경우 계속 돌린다.
8. 이제 남은 노드들을 or 연산 하여 답을 도출한다. (이 노드들은 무조건 이동)


## **풀이 핵심**
1. 모든 노드를 1024개의 배열로써 전 노드로부터 어떤 값이 왔는지 저장한다.

## **Algorithm**
1. 각 node의 입력 저장한다.
2. **모든 노드를 1024크기의 배열로 확보한다.**
3. BFS 또는 DFS를 이용하여 서치 한다.

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

    // Complete the beautifulPath function below.
    static public class Node {
        int to;
        int weight;
        
        Node(int to, int weight) {            
            this.to = to;
            this.weight = weight;            
        }
    
    }
    
    static int beautifulPath(int n, int[][] edges, int A, int B) {
                
        List<Node> map[] = new ArrayList[n+1];
        for(int i = 0; i < map.length; i++) {
            map[i] = new ArrayList<>();
        }
        
        for(int i = 0; i < edges.length; i++) {
            int from = edges[i][0];
            int to = edges[i][1];
            int weight = edges[i][2];
            
            map[from].add(new Node(to, weight));
            map[to].add(new Node(from, weight));
        }
        
        boolean[][] isMoved = new boolean[n+1][1024];
        
        isMoved[A][0] = true;
        
        Queue<Integer> haveToMove = new LinkedList<>();
        haveToMove.add(A);
        
        Queue<Integer> accumulatedOR = new LinkedList<>();
        accumulatedOR.add(0);
        
        while (!haveToMove.isEmpty())
        {
            int cur = haveToMove.poll();
            int or = accumulatedOR.poll();
            
            for(Node node : map[cur]) {
                int to = node.to;
                int weight = node.weight | or;
                
                if(!isMoved[to][weight]) {
                    isMoved[to][weight] = true;
                    haveToMove.add(to);
                    accumulatedOR.add(weight);                    
                }
            }
            
        }
        for (int i = 0; i < 1024; i++)
        {
            if (isMoved[B][i])
            {
                return i;
            }
        }
        
        return -1;
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

        String[] AB = scanner.nextLine().split(" ");

        int A = Integer.parseInt(AB[0]);

        int B = Integer.parseInt(AB[1]);

        int result = beautifulPath(n, edges, A, B);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedWriter.close();

        scanner.close();
    }
}

```
