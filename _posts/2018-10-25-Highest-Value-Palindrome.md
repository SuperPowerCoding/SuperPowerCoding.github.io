---
layout: post
title: Highest Value Palindrome
categories: HackerRank
tags: 
- HackerRank
- string
---

### HackerRank link
* * *
[Highest Value Palindrome](https://www.hackerrank.com/challenges/richie-rich/problem)

### 풀이 핵심
* * *
1. 앞자리가 클 수록 큰 숫자 이다.
2. 입력 k 보다 변경해야 할 숫자가 더 크면 불가능하다. (-1 리턴)
3. k가 변경 해야 할 숫자가 작으면, 숫자를 9로 바꾸면서 가장 큰 수를 만들어 낼 수 있다.

### Algorithm
* * *
1. 변경해야 할 숫자와 변경해야 하는 index를 기록한다. 이때, 앞뒤가 같도록 숫자를 변경한다.
2. 가능/불가능 여부를 체크한다.
3. 숫자가 이미 9인경우에는 변경할 필요가 없음으로 패스.
4. 변경이 불필요 하는 경우, 나머지 변경 가능한 수가 2번인 경우 변경 : 나머지 차감 -2
5. 그렇지 않는 경우(변경해야 하는 index) 변경한다. : 나머지 차감 -1
6. 길이가 홀수 인 경우, 가운데 index를 추가 변경이 가능한 경우 9로 변경해 준다.

### Source Code
* * *
{% highlight js %}
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.regex.*;

public class Solution {

    // Complete the highestValuePalindrome function below.
    static String highestValuePalindrome(String s, int n, int k) {
        
        // idx :0 1 2 3 4 
        // 홀수 : 1 2 3 4 5
        //길이 5 : length / 2 = 2 OK
        // 짝수 : 1 2 3 4
        // 길이 2 : length / 2 = 2
        int[] front;
        int[] back;
        boolean[] changed;
        int haveToChange = 0;
        int len = s.length() / 2;
        
        front = new int[len];
        back = new int[len];
        changed = new boolean[len];
        
        // len => 3 : 3 / 2 = 1        
        for(int i = 0 ; i < len ; i++)
        {
            front[i] = Integer.parseInt(s.substring(i, i+1));    //"smiles".substring(1, 5) returns "mile"
            back[i] = Integer.parseInt(s.substring(s.length() - i-1, s.length() - i));
            if(front[i] != back[i])
            {
                haveToChange++;
                if(front[i] > back[i])
                {
                    back[i] = front[i];
                }
                else
                {
                    front[i] = back[i];
                }
                changed[i] = true;
            }
        }      
        
        int rest = k - haveToChange;
        if(rest < 0) return "-1";    
        
        System.out.println(k+","+haveToChange+","+rest);
        int i = 0;
        while(rest > 0 && i < len)
        {   
            if(front[i] != 9)
            {
                if(changed[i] == false && rest >= 2)
                {
                    
                    front[i] = 9;
                    back[i] = 9;
                    rest -= 2;
                }
                else if(changed[i])
                {
                    front[i] = 9;
                    back[i] = 9;
                    rest --;
                }                
            }
            
            i++;
        }
        
        System.out.println(k+","+haveToChange+","+rest);
        
        
        StringBuilder str = new StringBuilder();
        
        //홀수 미드 처리 ( 남는거 있으면 9로)
        
        for( i = 0 ; i < len ; i++)
        {
            str.append(front[i]);
        }
        
        if(s.length() % 2 != 0)
        {
            if(rest > 0)
            {
                str.append("9");
            }
            else
            {
                str.append(s.substring(len, len+1));
            }
        }        
        
        for( i = 0 ; i < len ; i++)
        {
            str.append(back[len-1-i]);
        }        
        
        return str.toString();

    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] nk = scanner.nextLine().split(" ");

        int n = Integer.parseInt(nk[0]);

        int k = Integer.parseInt(nk[1]);

        String s = scanner.nextLine();

        String result = highestValuePalindrome(s, n, k);

        bufferedWriter.write(result);
        bufferedWriter.newLine();

        bufferedWriter.close();

        scanner.close();
    }
}

{% endhighlight %}
