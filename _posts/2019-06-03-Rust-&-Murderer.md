---
layout: post
title: Rust & Murderer
categories: HackerRank
tags: 
- HackerRank
- Graph Theory
---

## **HackerRank link**
[Rust & Murderer](https://www.hackerrank.com/challenges/rust-murderer/problem)

## **풀이 과정**
일단 제일 간단하게 생각해 볼 수 있는 것은

2차원의 커다란 배열의 map을 만들고 입력이 들어 올때 표시를 해둔다.

그리고 아까 표시된 입력을 제외한 경로로 BFS를 한다.

그런데 여기서 문제가 있다. 노드의 수가 엄청나게 많다.

이렇게 map 데이터를 만드려면 2x10^5 x 2x10^5 = 4x10^10 java에서는 heap이 부족하다.

다른 방법을 생각해야 한다.

기존의 BFS등의 트리의 이동관련 알고리즘은 갈 수 있는 node 데이터를 중심으로 다음 가야 하는 데이터에 대한 중심으로 이동한다.

이 때문에 완전 반대로 생각해 보았다.

먼저 이렇게 할 수 있고, 빠르다고 생각한것은 

> The graph/map of the city is ensured to be a sparse graph.

이기 때문이다. (확실하게 Node의 수 보다 엣지의 수가 월등히 적다.)

먼저 전부 갈 수 있다고 생각하고, 

가면 안되는 엣지만 체크 한다.

만약 이 엣지가 가면 안되는 곳을 제외하고, 갈 수 있는 곳과 이동수 차이가 1난다면,

이 엣지는 갈 수 있는 곳과 연결되어 있는 것이므로 이동된것이다.

일단 모든 노드가 시작 점을 제외하고 1이라고 생각하고,

이동하면 안되는 것들은 2로 고정해두고

이동하면 안되는 부분만 생각하면 된다.

에디토리얼과 좀 다르게 풀었는데, 결과적으로는 내 풀이가 훨씬 빠르다.

이유는 에디토리얼은 리스트 2개를 이용해서 계산하는데, 

리스트 두개를 이용하면서 삭제와 추가가 너무 빈번해서 조금 더 느린 것 같다.

**Editorial 풀이**
에디토리얼과 좀 다른데, 에디토리얼 풀이를 분석하면

2개의 리스트를 이용하고, 전부 이동했다고 생각하고 이동 하면 안되는 것들을 제외 시키는 알고리즘이다.

1. 이동한 노드의 리스트
2. 이동해야 하는 노드의 리스트

알고리즘은 다음과 같다
1. 일단 모두 이동했다고 생각하고, 이동한 리스트에 등록한다.
2. 처음은 제외한다.
3. 처음을 기준으로 이동 못 하는 경로가 이동 안했으면 리스트 1번에서 제거 하고, 리스트 2번에 등록한다.
4. 리스트 1번에서 제거 안된 노드들은 이동했으니까 거리를 1씩 증가 시킨다.
5. 리스트 1을 모두 제거 하고, 리스트 2의 데이터를 모두 등록한다.

## **풀이 핵심**
1. 일단 이동했다고 생각하고, 이 노드가 진짜 이동했는지 안했는지 판단한다.
2. 이동 해서는 안되는 노드가 아니고, 다른 노드들과 비교하고 노드가 거리 값이 1차이면 이동가능

## **Algorithm**
1. 일단 처음 시작지를 중심으로 모두 이동했다고 생각한다.
2. 자기 자신은 0으로 초기화
3. 이동 해서는 안되는 노드들은 거리 정보를 1씩 증가시키고, 이 노드들을 체크 한다.
4. 이동 해서는 안되는 노드가 아니고, 다른 노드들과 비교하고 노드가 거리 값이 1차이면 이동가능 하므로 볼 피요가 없다.
5. 만약 4번이 아니라면, 거리를 1로 증가 시키고 앞으로 살펴봐야 한다고 큐에 등록
6. 현재 살펴봐야할 노드를 모두 봤다면, 앞으로 살펴봐야 하는 노드의 데이터를 옮기고 반복한다.

## **Source Code**
```java
import java.io.*;
import java.math.*;
import java.text.*;
import java.util.*;
import java.util.regex.*;

public class Solution {

     static int[] rustMurderer(int n, int s, int[][] roads) {
        /*
         * Write your code here.
         */
        int []result = new int[n-1];
        List<Integer> mainRoad[] = new ArrayList[n+1];
        for(int i = 1 ; i <= n; i++) {
            mainRoad[i] = new ArrayList<>();
        }

        for(int i = 0; i < roads.length; i++) {
            int to = roads[i][0];
            int from = roads[i][1];
            
            mainRoad[to].add(from);
            mainRoad[from].add(to);            
        }
        
        for(int i = 1; i <= n; i++) {
            Collections.sort(mainRoad[i]);
        }
        
        
        int[] isMoved = new int[n+1];        
        for(int i = 0; i <= n; i++) {
            isMoved[i] = 1;
        }
        
        isMoved[s] = 0;
        Queue<Integer> haveToCheck = new LinkedList<>();                
        for(int e : mainRoad[s]) {
            isMoved[e]++;
            haveToCheck.add(e);
        }
        
        Queue<Integer> next = new LinkedList<>();        
        int curDis = 1;
        
        while(haveToCheck.isEmpty() == false ) {
            
            int cur = haveToCheck.poll();
            int dont = 0;
            int idx = 0;
            if(mainRoad[cur].size() >= idx + 1) {
                dont = mainRoad[cur].get(idx++);
            }
            boolean found = false;
            for(int i = 1; i <= n; i++) {
                if(i == dont) {
                    if(mainRoad[cur].size() >= idx + 1) {
                        dont = mainRoad[cur].get(idx++);
                    }
                } else {
                    if(isMoved[i] == curDis) {
                        found = true;
                        break;
                    }
                }
            }
            
            if(found == false) {
                isMoved[cur]++;
                next.add(cur);
            }
            
            if(haveToCheck.isEmpty()) {
                   haveToCheck.addAll(next);
                   next.clear();
                   curDis++;
            }
            
        }
        
        
        int count = 0;
        for(int i = 1 ; i <= n; i++) {
            if(i != s) {
                result [count++] = isMoved[i];
            }
        }
        
        return result;
    }
        

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int t = Integer.parseInt(scanner.nextLine().trim());

        for (int tItr = 0; tItr < t; tItr++) {
            int n = scanner.nextInt();
            int m = scanner.nextInt();

            int[][] roads = new int[m][2];

            for (int roadsRowItr = 0; roadsRowItr < m; roadsRowItr++) {    
                 roads[roadsRowItr][0] = scanner.nextInt();
                 roads[roadsRowItr][1] = scanner.nextInt();
            }

            int s = scanner.nextInt();

            int[] result = rustMurderer(n, s, roads);

            for (int resultItr = 0; resultItr < result.length; resultItr++) {
                bufferedWriter.write(String.valueOf(result[resultItr]));

                if (resultItr != result.length - 1) {
                    bufferedWriter.write(" ");
                }
            }

            bufferedWriter.newLine();
        }

        bufferedWriter.close();
    }
}

```

에디토리얼 포팅
```java
import java.io.*;
import java.math.*;
import java.text.*;
import java.util.*;
import java.util.regex.*;

public class Solution {

     static int[] rustMurderer(int n, int s, int[][] roads) {
        /*
         * Write your code here.
         */
        int []result = new int[n-1];
        List<Integer> mainRoad[] = new ArrayList[n+1];
        for(int i = 1 ; i <= n; i++) {
            mainRoad[i] = new ArrayList<>();
        }

        for(int i = 0; i < roads.length; i++) {
            int to = roads[i][0];
            int from = roads[i][1];
            
            mainRoad[to].add(from);
            mainRoad[from].add(to);            
        }
        
        PriorityQueue<Integer> moved = new PriorityQueue<>();
        PriorityQueue<Integer> left = new PriorityQueue<>();
        
        boolean[] isMoved = new boolean[n+1]; 
        int[] distance = new int[n+1];
        
        for(int i = 1; i <= n; i++) {
            moved.add(i);
        }
        
        moved.remove(s); 
        
        isMoved[s] = true;
        Queue<Integer> haveToCheck = new LinkedList<>();
        haveToCheck.add(s);
        
        while(!haveToCheck.isEmpty()) {
            
           int cur = haveToCheck.poll();
           for(int e : mainRoad[cur]) {
               if(!isMoved[e]) {
                  moved.remove(e);
                  left.add(e);                      
               }
           }
           
           while(!moved.isEmpty()) {
               int e = moved.poll();
               isMoved[e] = true;
               distance[e] = distance[cur] + 1;
               haveToCheck.add(e);               
           }
            
           moved.addAll(left);
           left.clear();
        }
        
        
        int count = 0;
        for(int i = 1 ; i <= n; i++) {
            if(i != s) {
                result [count++] = distance[i];
            }
        }
        
        return result;
    }
        

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int t = Integer.parseInt(scanner.nextLine().trim());

        for (int tItr = 0; tItr < t; tItr++) {
            int n = scanner.nextInt();
            int m = scanner.nextInt();

            int[][] roads = new int[m][2];

            for (int roadsRowItr = 0; roadsRowItr < m; roadsRowItr++) {    
                 roads[roadsRowItr][0] = scanner.nextInt();
                 roads[roadsRowItr][1] = scanner.nextInt();
            }

            int s = scanner.nextInt();

            int[] result = rustMurderer(n, s, roads);

            for (int resultItr = 0; resultItr < result.length; resultItr++) {
                bufferedWriter.write(String.valueOf(result[resultItr]));

                if (resultItr != result.length - 1) {
                    bufferedWriter.write(" ");
                }
            }

            bufferedWriter.newLine();
        }

        bufferedWriter.close();
    }
}


```
