---
layout: post
title: The Grid Search
categories: HackerRank
tags:
- HackerRank
- Implementation
---

## HackerRank link
    [The Grid Search](https://www.hackerrank.com/challenges/bomber-man/problem)

## 풀이 핵심
1. 패턴을 인식하려면, 전체를 검색해서 맞는지 확인해야 한다.
2. Java의 indexOf를 이용하면 쉽다. (해당 문자열의 시작 index 리턴).

* 알고리즘이 어렵지 않은 문제.


## Algorithm
1. 비교 문자열의 첫번째 index를 구한다.
2. -1이라면 없음 다음으로 이동
3. 만약 찾았다면, 패턴이 모두 일치하는지 확인.
4. 모두 일치 한다면 "YES"
5. 전체 검색 후에도 일치하는 패턴이 없다면, "NO" 리턴


## Source Code
{% highlight js %}
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.regex.*;

public class Solution {

     // Complete the gridSearch function below.
    static String gridSearch(String[] G, String[] P) {
        int idx = 0;
        
        
        for(int i = 0 ; i < G.length ; i++)
        {
            for(int j = 0 ; j <= G[i].length() - P[0].length(); j++)
            {
                int count = 1;
                idx = G[i].substring(j, G[i].length()).indexOf(P[0]); 
                
                //System.out.println(G[i].substring(j, G[i].length())+","+P[0]+"/"+idx);
                
                if(idx == -1) continue;
                for(int k = 1; k < P.length && ((i + k) < G.length) ; k++)
                {               
                    if (idx == G[i+k].substring(j, G[i].length()).indexOf(P[k])) count++;
                    else break;
                }
                
                // System.out.println(count);
                
                if(count == P.length)    return "YES";
                
            }
            // System.out.println();
            // System.out.println();
        }
        
        return "NO";
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int t = scanner.nextInt();
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        for (int tItr = 0; tItr < t; tItr++) {
            String[] RC = scanner.nextLine().split(" ");

            int R = Integer.parseInt(RC[0]);

            int C = Integer.parseInt(RC[1]);

            String[] G = new String[R];

            for (int i = 0; i < R; i++) {
                String GItem = scanner.nextLine();
                G[i] = GItem;
            }

            String[] rc = scanner.nextLine().split(" ");

            int r = Integer.parseInt(rc[0]);

            int c = Integer.parseInt(rc[1]);

            String[] P = new String[r];

            for (int i = 0; i < r; i++) {
                String PItem = scanner.nextLine();
                P[i] = PItem;
            }

            String result = gridSearch(G, P);

            bufferedWriter.write(result);
            bufferedWriter.newLine();
        }

        bufferedWriter.close();

        scanner.close();
    }
}
{% endhighlight %}

