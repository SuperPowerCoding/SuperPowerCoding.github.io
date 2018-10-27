---
layout: post
title: Fraudulent Activity Notifications
categories: HackerRank
tags:
- HackerRank
- sorting
- radix sorting
---

## **HackerRank link**
[Fraudulent Activity Notifications](https://www.hackerrank.com/challenges/fraudulent-activity-notifications/problem)


## **풀이 핵심**
1. median 값을 sorting 없이 구할 수 있는 방법은 없다.
2. median의 값을 구하기 위해 검색 해야 하는 날(d) 기준으로 매번 일반적인 sorting 한다면, 퍼포먼스에 큰 영향을 미친다.
3. 0 <=expenditure[i] <= 200으로 radix sort을 이용한다면, median 값을 구하기 위해 매번 100회 정도의 검색만으로도 구할 수 있다.


## **Algorithm**
1. 처음 d날짜를 기준으로 radix를 업데이트 한다.
2. radix를 이용하여 median을 구한다.
3. 알림을 해야 하는지 판단한다.
4. radix를 업데이트 한다.


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
    
    static int[] radix = new int[201];
    
    static int getMedian(int d)
    {
        int median = 0;
        int cnt = 0;
        
        // odd  : 0 1 2, 3/2 + 1 = 2
        //        0 1 2 3 4, 5/2 + 1 = 3
        // even : 0 1  , 2/2 = 1 
        //        0 1 2 3, 4/2 = 2
        int mid = d % 2 == 0 ? d / 2 : d / 2 + 1;
        
        for(int i = 0; i < radix.length ; i++)
        {
            if(radix[i] > 0)
            {
                cnt += radix[i];
                if(cnt >= mid)
                {
                    median += i;
                    if(d % 2 == 0)
                    {
                        if( median != i) break;
                        
                        if(cnt > mid) 
                        {
                            median += i;
                            break;
                        }
                        
                    }
                    else
                    {
                        median += i;
                        break;                        
                    }
                }
            }                
        }
        
        return median;
    }
    // Complete the activityNotifications function below.
    static int activityNotifications(int[] expenditure, int d) {
        
        if( expenditure.length == d) return 0;
        
        int notice = 0;
        
        // set first radix for first d days.
        for(int i = 0 ; i < d; i++)
        {
            radix[expenditure[i]]++;
        }
        
        // set first radix for first d days.
        for(int i = d ; i < expenditure.length; i++)
        {
            int median = getMedian(d);            
            
            if(median <= expenditure[i]) notice++;
            
            System.out.println(median+","+expenditure[i]);
            
            radix[expenditure[i - d]]--;
            radix[expenditure[i]]++;
            
        }
        
        return notice;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] nd = scanner.nextLine().split(" ");

        int n = Integer.parseInt(nd[0]);

        int d = Integer.parseInt(nd[1]);

        int[] expenditure = new int[n];

        String[] expenditureItems = scanner.nextLine().split(" ");
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        for (int i = 0; i < n; i++) {
            int expenditureItem = Integer.parseInt(expenditureItems[i]);
            expenditure[i] = expenditureItem;
        }

        int result = activityNotifications(expenditure, d);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedWriter.close();

        scanner.close();
    }
}

{% endhighlight %}
