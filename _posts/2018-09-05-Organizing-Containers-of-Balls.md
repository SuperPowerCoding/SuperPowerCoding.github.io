---
layout: post
title: Organizing Containers of Balls
categories: HackerRank
tags:
- HackerRank
- Implementation
---

## **HackerRank link**
[Organizing Containers of Balls](https://www.hackerrank.com/challenges/organizing-containers-of-balls/problem)


## **풀이 핵심** 
각 타입의 총 수와 각 컨테이너가 갖고 있는 ball의 총 수가 일치 해야 함.

ex)  
<table>
 <tr>
  <th></th>
  <th>type</th>
  <th></th>
 </tr>
 <tr>
  <th></th>
  <th>0</th>
  <th>1</th>
 </tr>
 <tr>
  <th>containers</th>
  <th>1</th>
  <th>1</th>
 </tr>
 <tr>
  <th></th>
  <th>0</th>
  <th>2</th>
 </tr>
</table>
 
type 0번의 총 갯수가 1, container 0의 총 갯수는 2  
type 1번의 총 갯수가 3, container 1의 총 갯수는 2  

container의 0에 type 0을 다 swap해서 넣는다고 할때, type 0은 1개만 있는데 container의 0은 2개가 있으므로, 절대 type 0만 담을 수 없다.


## **Algorithm**
1. type 별 총 갯수를 구한다. (각 행의 합)
2. container가 갖고 있는 수를 구한다. (각 열의 합)
3. 한쪽으로 각 타입을 몰아 넣는다. (정렬한다)
4. 타입의 총 수와 conatainer의 총수가 일치 하는지 확인.

## **Source Code**
{% highlight js %}
import java.io.*;
import java.util.*;
import java.text.*;
import java.math.*;
import java.util.regex.*;

public class Solution {

    static String organizingContainers(int[][] container) {
        // Complete this function
        boolean debugPrint = true;
        long[] sumRow = new long[container.length];
        long[] sumCol = new long[container[0].length];
       
        // sum col
        for(int i = 0 ; i < container.length ; i++)
        {
            for(int j = 0 ; j < container.length ; j++)
            {
                sumRow[i] += (long)container[i][j];
            }
        }
        
        // sum row
        for(int i = 0 ; i < container.length ; i++)
        {
            for(int j = 0 ; j < container.length ; j++)
            {
                sumCol[i] += (long)container[j][i];
            }
        }
        
        Arrays.sort(sumRow);
        Arrays.sort(sumCol);
        
        for(int i = 0 ; i < sumRow.length ; i++)
        {
            if(sumRow[i] != sumCol[i])  return "Impossible";    
        }
        
        
        return "Possible";
    }

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int q = in.nextInt();
        for(int a0 = 0; a0 < q; a0++){
            int n = in.nextInt();
            int[][] container = new int[n][n];
            for(int container_i = 0; container_i < n; container_i++){
                for(int container_j = 0; container_j < n; container_j++){
                    container[container_i][container_j] = in.nextInt();
                }
            }
            String result = organizingContainers(container);
            System.out.println(result);
        }
        in.close();
    }
}
{% endhighlight %}

