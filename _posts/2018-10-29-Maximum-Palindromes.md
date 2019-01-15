---
layout: post
title: Maximum Palindromes
categories: HackerRank
tags: 
- HackerRank
- Strings
---

## **HackerRank link**
* [Maximum Palindromes](https://www.hackerrank.com/challenges/maximum-palindromes/problem)


## **풀이 핵심**
1. 페르마의 소정리을 이용한 Factorial의 나눗셈 modulo 연산
2. 분할정복을 이용한 빠른 거듭제곱 계산
3. 퍼포먼스를 위한 초기 계산   

Solution의 답은 중복이 포함된 경우의 수를 구하는 문제이므로 쉽게 구할 수 있다.  

> (짝수개의 문자 수/2)! x (중간에 올 수 있는 수) <br>
/ (중복된 문자 'a'의 수)! / (중복된 문자 'b'의 수)! / ... / (중복된 문자 'z'의 수)!  

하지만 입력 조건에서 문자열의 최대 길이는 100000이고, 이 때문에 factorial을 구하는 경우 매우 큰 값이 나올 수 있기 때문에, 답을 구할 때 10^9+7 값의 나머지를 리턴하라고 하고 있다.  

곱셈에서의 나머지 연산은 다음 식과 같이 아무때나 연산을 해도 성립하지만, 문제는 나눗셈에서는 성립하지 않는 것이다.

> (a x b) % p = (a % p) x (b % p) <br>
(a/b) % p != (a%p) / (b%p)  <br><br> 
<span style="color:red">ex) </span> <br>
(4! % 11) / (3! % 11) = 2 / 6 = 0.33  <br>
(4! / 3!) % 11 = 4

**페르마의 소정리 이용** <br>
이 문제를 해결하기 위해 [페르마의 소정리](https://ko.wikipedia.org/wiki/%ED%8E%98%EB%A5%B4%EB%A7%88%EC%9D%98_%EC%86%8C%EC%A0%95%EB%A6%AC)를 이용한다.  

페르마의 소정리에서 a^(p-1) = 1 (mod p) 이므로, a x a^(p-2) = 1 (mod p) 이다. <br>
즉, a의 역원은 a^(p-2) 임을 이용한다.  
ex) (4! % p) / (3! % p) = (4! % p) x ((3!)^(p-2) % p)  

**분할 소거법 이용** <br>
그런데 여기서 p-2의 거듭제곱을 구하는데, p가 매우 크므로 분할정복을 이용하여 빠르게 계산을 한다. [Exponentiation by squaring](https://en.wikipedia.org/wiki/Exponentiation_by_squaring)  

**미리 계산** <br>
마지막으로, 입력의 l과 r로 입력 문자열에서 'a' ~ 'z'가 각각 몇개가 있는지 알아야 답을 구할 수 있다. 테스트 케이스에서 l과 r이 입력으로 들어올 때마다 문자열을 확인하여 구할수 있지만, 입력 최대 문자열의 길이가 10^5 이고, 최대 test case의 수도 10^5 이기 때문에, 이 방법은 속도면에서 매우 비효율 적이다. <br>

이를 해결하기 위해 입력을 받을 때, 각 문자열 길이 별로 a~z까지의 수를 계산을 해두면 빠르게 계산이 가능하다. (메모리가 많이 사용 되는 단점이 있지만, 속도는 훨씬 빠르다.)

최악의 경우 대략적으로 연산량을 계산하면 (l = 1, r = 10^5, q = 10^5), <br>
- 미리 계산 안 하는 경우: 10^10번 연산 <br>
- 미리 계산하는 경우 : 10^5 x 24번 연산 <br>

**자바의 경우 : reference code 최적화** <br>
자바의 경우 String을 계속적으로 다루는 경우 메모리 사용에 의해 퍼포먼스가 매우 낮다. 이 때문에, reference 코드에서 하기의 코드를 수정한다. <br>
```java 
//변경 전
String[] lr = scanner.nextLine().split(" ");
int l = Integer.Parse(lr[0]);
int r = Integer.Parse(lr[1]);

// 변경 후
int l = scanner.nextInt();
int r = scanner.nextInt();
```


## **Algorithm**
**초기화 부분 : initialize**
1. modulo 계산이 포함된 Factorial을 구한다. 
2. modulo 계산이 포함된 Inverse Factorial을 구한다.
3. a~z까지의 갯수를 문자열 0 ~ 입력 길이 만큼 저장한다.

**계산 부분 : answerQuery**
1. 미리 연산된 데이터를 이용하여 'a'~'z'까지의 수를 구한다.
2. 'a'~'z'까지의 수를 이용하여 답을 구한다.

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

    static final int size = 100001;
    static long mod = (long)Math.pow(10,9)+7;
    static long[] factorial = new long[size];
    static long[] inverseFac = new long[size];
    static int a2z[][];
    
    static long powerWithMod(long a, long n)
    {
        long ret = 1;
        while(n > 0)
        {
            if(n % 2 == 1)    // 홀수 인경우 한번 먼저 곱
            {
                ret *= a;
                ret %= mod;
                
            }
            a *= a;
            a %= mod;
            n /= 2;
        }
        
        return ret;
    }
    
    
    // Complete the initialize function below.
    static void initialize(String s) {
        // This function is called once before all queries.
        // initialize factorial
        factorial[0] = 1;
        factorial[1] = 1;
        for(int i = 2 ; i < factorial.length ; i++)
        {
            factorial[i] = factorial[i-1] * i % mod;
        }
        
        // inverse factorial for modulation.
        // a^(P-1)
        // a x X = 1 (mod P-1)
        // X = a^(P-2)
        
        inverseFac[size-1] = powerWithMod(factorial[size-1], mod - 2);
        for (int i = size - 2; i >= 0; i--) 
        {
            inverseFac[i] = (inverseFac[i + 1] * (i + 1)) % mod;     
        }
        
        a2z = new int[s.length()+1]['z'-'a'+1];                
        
        for(int i = 0 ; i < s.length() ; i++)
        {
            System.arraycopy(a2z[i], 0, a2z[i + 1], 0, 'z' - 'a' + 1);
            a2z[i+1][s.charAt(i)-'a']++;
        }
    }

    // Complete the answerQuery function below.
    static int answerQuery(int l, int r) {
        // Return the answer for this query modulo 1000000007.
        int mid = 0;
        int total = 0;
        long inverse = 1;
        long result =1;
        int[] aTozCnt = new int['z'-'a' + 1];
        
        for(int i = 0 ; i < aTozCnt.length ; i++)
        {
            aTozCnt[i] = a2z[r][i] - a2z[l-1][i];
        }        
        
        for(int cnt : aTozCnt)
        {
            if(cnt % 2 == 0)    // 짝수
            {
                total += cnt/2;
                inverse = (inverse * inverseFac[cnt/2])%mod;
                
            }
            else // 홀수
            {
                if(cnt == 1)    // 한개
                {
                    mid++;
                }
                else
                {
                    total += (cnt - 1)/2;
                    mid++;
                }
                inverse = (inverse * inverseFac[(cnt-1)/2])%mod;
            }
        }
        if(mid == 0) mid =1;
        
        result = (factorial[total] * inverse) % mod;
        result = (result * mid) % mod;
   
        return (int)result;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        String s = scanner.nextLine();

        initialize(s);
        int q = scanner.nextInt();
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        for (int qItr = 0; qItr < q; qItr++) {
            // String[] lr = scanner.nextLine().split(" ");

            int l = scanner.nextInt();

            int r = scanner.nextInt();

            int result = answerQuery(l, r);

            System.out.println(result);
        }

        scanner.close();
    }
}
```