---
layout: post
title: Lily's Homework
categories: HackerRank
tags:
- HackerRank
- sorting
---

## **HackerRank link**
[Lily's Homework](https://www.hackerrank.com/challenges/lilys-homework/problem)


## **풀이 핵심**
1. 배열이 오름차순 또는 내림차순으로 정렬로 된 경우에 전후의 배열의 절대값의 합이 최소가 된다.
2. 스왑해야 하는 수를 구하기 위해서는 직접 구하는 방법외에는 없다.
3. 각 index가 스왑해야 하는지 판단하기 위해선 sorting을 해야 한다.
4. 스왑해야 하는 위치를 알기 위해서는 HashMap이 필요하다. (key, value)


## **Algorithm**
1. input을 받으면서 HashMap을 업데이트 한다.
2. 퍼포먼스가 좋은 내부 sort 함수를 이용하여 입력 sort한다.
3. 오름 차순에 대하여 swap 횟수를 구한다.
4. 내림 차순에 대하여 swap 횟수를 구한다.
5. 과정 3, 4 중 최소값을 리턴한다.


## **Source Code**
{% highlight js %}
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.regex.*;

public class Solution {
    static int[] sorted;
    static int[] inputArr;
    static HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();    
	
    /*
	1. 스왑해야 하는 장소를 알기 위해 Hash map을 만든다.
	
	2. 오름차순으로 정렬한 배열로, 오름차순 순으로 스왑을 한다.
	
	3. 정렬된 배열을 이용하여, 내림차순으로 스왑을 한다.
	
	*/
    static int ascendingCnt(int[] input, HashMap<Integer, Integer> map)
    {
        int cnt = 0;
        
        for(int i = 0 ; i < input.length; i++)
        {
            // 정렬된 것과 input이 다르면.... 스왑!
            if(input[i] != sorted[i])
            {
                cnt++;
                int target = (int)map.get(sorted[i]);
                int temp = input[target];                
                input[target] = input[i];
                input[i] = temp;
                
                // 한개는 이미 fix 되었으니 앞으로 바뀔 것의 idx 값만 바꾸면 됨.
                map.put(input[target], target);                
            }
        }
        
        return cnt;
    }
    
    static int descendingCnt(int[] input, HashMap<Integer, Integer> map)
    {
        int cnt = 0;
        int n = input.length - 1;
        
        for(int i = 0 ; i < input.length; i++)
        {
            if(input[i] != sorted[n-i])
            {
                cnt++;
                
                int target = (int)map.get(sorted[n-i]);
                int temp = input[target];                
                input[target] = input[i];
                input[i] = temp;
                
                // 한개는 이미 fix 되었으니 앞으로 바뀔 것의 idx 값만 바꾸면 됨.
                map.put(input[target], target);             
            }
        }
        
        return cnt;
    }
    
    // Complete the lilysHomework function below.
    static int lilysHomework() {
                
        Arrays.sort(sorted);
                
        int ascending = ascendingCnt(Arrays.copyOf(inputArr, inputArr.length), new HashMap<Integer, Integer>(map));
        int descending = descendingCnt(Arrays.copyOf(inputArr, inputArr.length), new HashMap<Integer, Integer>(map));        
        
        System.out.println(ascending+","+descending);
        
        return ascending > descending ? descending : ascending;
        //return ascending;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int n = scanner.nextInt();
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        sorted = new int[n];
        inputArr = new int[n];
        
        for(int i = 0 ; i < n; i++)
        {
            int val = scanner.nextInt(); 
            inputArr[i] = val;
            sorted[i] = val;
            map.put(val, i);
        }

        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        int result = lilysHomework();
        System.out.println(result);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedWriter.close();

        scanner.close();
    }
}
{% endhighlight %}
