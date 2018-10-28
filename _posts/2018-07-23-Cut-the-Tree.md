---
layout: post
title: Cut the Tree
categories: HackerRank
tags:
- HackerRank
- Search
- Depth First Search
---

## **HackerRank link**
[Cut the Tree](https://www.hackerrank.com/challenges/cut-the-tree/problem)


## **풀이 핵심**
1. node의 탐색 방법으로는 DFS를 이용.
2. node를 탐색하면서 노드의 값을 더하면서 값을 계산함.

*  이 문제 에서 Java의 경우 input reference 코드의 퍼포먼스가 매우 안좋게 되어 있어서, 풀이법이 맞아도 계속적으로 timeout error가 발생함.
*  유사한 문제 : [Even Tree](https://superpowercoding.github.io/hackerrank/2018/09/12/Even-Tree/)

### 퍼포먼스를 좋게 하기 위한 방법
1. 입력을 받을 때 총합을 미리 계산한다. => node 탐색 시 계산에 용이
2. 입력 시 아래 코드와 같이 scanner.nextInt(); 를 사용하는 방법이 훨씬 빠름.

{% highlight js %}  
String[] arrItems = scanner.nextLine().split(" ");  // 쓰지 말것

int[] input = new int[input_num];
for(int i = 0 ; i < input_num ; i++)
{
    input[i] = scanner.nextInt();
}
{% endhighlight %}

여러 단계를 거쳐서 오는 문제점도 있고,  
자바에서 String을 다룰때 String Class는 퍼포먼스가 매우 좋지 않으니 조심.

## **Algorithm**
1. input으로 부터 tree를 구성한다. (Array List를 이용)
2. dfs를 이용하여 노드를 탐색한다.
3. 막다른 지점에서 다시 돌아오는 곳에서 부터 node의 길이를 count 한다.
4. 만약 count 값이 짝수라면, 자르는 값 증가.
5. 최종 입력이 무조건 짝수 이므로, 마지막 node까지 가게 되면 결과 값을 하나 더 하게 됨. 이때문에 마지막에 -1을 해줌.

## **Source Code**
{% highlight js %}
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.regex.*;


//오름차순
class Ascending implements Comparator<Integer> {

    @Override
    public int compare(Integer o1, Integer o2) {
       return o1.compareTo(o2);
    }

}


public class Solution {

    static int MAXIMUN_INPUT    =    100000+1;
    
    static int minimun = Integer.MAX_VALUE;
    
    static boolean[] visit = new boolean[MAXIMUN_INPUT];
    static ArrayList<Integer> tree[] = new ArrayList[MAXIMUN_INPUT];
    
    static int[] data = new int[MAXIMUN_INPUT];
    
    static int totalSum = 0;

   static int dfs(int start)
    {
        System.out.print(start + " ");
        
        int sum = 0;
        visit[start] = true;
        sum += data[start - 1];
        
        for(int i = 0 ; i < tree[start].size() ; i++)
        {
            int next = tree[start].get(i);
            if(visit[next] == false)
            {
                sum += dfs(next);
            }
        }
        
        System.out.println("("+sum + ")");
        
        int rest = totalSum - sum;
        int dif = sum > rest ? sum - rest : rest - sum;
        
        if(minimun > dif) minimun = dif;
        
        // System.out.print("("+dif + ")");
        
        return sum;
    }
    
    
    // Complete the solve function below.
    static int solve() { 
        dfs(1);        
        return minimun;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int n = scanner.nextInt();
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        // data = new int[n];

        for (int i = 0; i < n; i++) {
            int dataItem = scanner.nextInt();
            data[i] = dataItem;
            totalSum += dataItem;
            // System.out.print(data[i]+" ");
            tree[i] = new ArrayList<Integer>();        
        }
        
        tree[n] = new ArrayList<Integer>();        
        
        // System.out.println();
        
        int[][] edges = new int[n-1][2];

        for (int i = 0; i < n-1; i++) {           

            for (int j = 0; j < 2; j++) {
                int edgesItem =scanner.nextInt();
                edges[i][j] = edgesItem;
                
            }            
            int p0 = edges[i][0];
            int p1 = edges[i][1];
            tree[p0].add(p1);
            tree[p1].add(p0);
        }

        int result = solve();

        // System.out.println(result);
        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedWriter.close();

        scanner.close();
    }
}
{% endhighlight %}

