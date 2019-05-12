---
layout: post
title: Xor and Sum
categories: HackerRank
tags: 
- HackerRank
- Dynamic Programming
---

## **HackerRank link**
[Xor and Sum](https://www.hackerrank.com/challenges/xor-and-sum/problem)

## **풀이 과정**
XOR을 많이 안쓰다 보니 조금 생소하다. 문제 자체는 굉장히 심플하다. 주어진 수식을 계산 하면 되는 문제 이다.   

XOR 연산은 두 값의 각 자릿수를 비교해,값이 0으로 같으면 0, 값이 1로 같으면 0, 다르면 1을 계산한다.  

자세히 보면, 같으면 해당 자릿수는 빼기 다르면 더하기가 된다.
 
b의 경우 left shift를 계속적으로 하는데 shift는 1번 할때마다 2배가 된다.
 
String으로 들어 오는 두 값을 생각해보자. a, b 둘다  1 <= a,b <= 2^(10^5)
그런데, a는 증가 하지 않고 b는 2배씩 증가 한다.

shift 하다 보면 언젠가는 a랑 겹치지 않는 부분이 있다. 

이후는 계속 두 수를 더하면된다. 

즉, 겹치는 부분은 b에서 1 bit가 a의 1 bit 만날 때 이루어지고, 해당 수 만큼 빼주면 된다.

solution = b의 총합 + a * 314160 - (겹치는 부분)
 
b의 총합은 등비수열의 합이므로, sum a*r^(n-1) = a * ((r^n)-1) / (r-1) : ( r > 1)
 
문제는 겹치는 부분인데
첫번째로 계산 방법은 2중 for문이다.   
```java
for 0 -> a.length  
    for 0 -> b.length  
	   if ( a(i) == b(j) ) count++;  
```	  
이런식으로 그런데 input이 2^(10^5) 이므로 2중 for문 사용시 어마어마한 계산량이...
	  
그렇다면 한번에 가능할 방법이 있지 않을까?
	 
뒤쪽부터 계산 하면 가능 하다.

## **풀이 핵심**
1. Xor 계산에서 같은 자릿수의 bit가 같으면 "빼기" 다르면 "더하기"가 된다.
2. solution = sum(b) + a * 314160 - (겹치는 부분) 으로 계산이 된다.
3. 겹치는 부분을 한번에 계산하기 위해서 최상위 비트부터 계산한다.
4. 제곱을 계산하기 위해서 분할 정복법을 이용하여 빠르게 계산한다.

## **Algorithm**
1. 겹치는 부분을 계산한다. 그 겹치는 부분의 2배를 추후 빼줄 것이다. 2배인 이유는 a도 빠지고 b도 빼야되기 때문.
2. b의 합을 구한다. 등비수열의 합을 구한다. a(r^n-1)/(r-1)   (r > 1)  
3. a의 합을 구한다. a * (314159 + 1) 0부터 134159번이므로
4. 결과를 구한다. solution = b의 총합 + a * 314160 - (겹치는 부분)

## **Source Code**
```java
import java.io.*;
import java.math.*;
import java.text.*;
import java.util.*;
import java.util.regex.*;

public class Solution {

    /*
     * Complete the xorAndSum function below.
     */
	static final long modulo = 1000000000 + 7;
	static final long until = 314159 + 1;

	/*
	 * XOR 연산은 두 값의 각 자릿수를 비교해,값이 0으로 같으면 0, 값이 1로 같으면 0, 다르면 1을 계산한다.
	 * 즉, 같으면 해당 자릿수는 빼기 다르면  더하기가 된다.
	 * 
	 * b의 경우 left shift를 계속적으로 하는데 shift는 1번 할때마다 2배가 된다.
	 * 
	 * String으로 들어 오는 두 값을 생각해보자. a, b 둘다  1 <= a,b <= 2^(10^5)
	 *  그런데, a는 증가 하지 않고 b는 2배씩 증가 한다.
	 *  
	 * b를 shift 하다 보면 언젠가는 a랑 겹치지 않는 부분이 있다. 이런 경우 계속 두 수를 더하면된다.
	 * 그런데 겹치는 부분이 있다. 그런경우 그 수를 빼면된다.
	 *  
	 * 즉, 겹치는 부분은 b에서 한 bit가 a의 bit와 만날 때 이루어지고, 해당 수 만큼 빼주면 된다.
	 *  
	 * solution = b의 총합 + a * 314159 - (겹치는 부분)
	 *  
	 * b의 총합은 등비수열의 합이므로, sum a*r^(n-1) = a * ((r^n)-1) / (r-1) : ( r > 1)
	 * 
	 * 문제는 겹치는 부분인데
	 * 첫번째로 계산 방법은 2중 for문이다. 
	 *  for 0 -> a.length
	 *   for 0 -> b.length
	 *     if ( a(i) == b(j) ) count++;
	 *  
	 * 이런식으로 그런데 input이 2^(10^5) 이므로 2중 for문 사용시 어마어마한 계산량이...
	 *  
	 * 그렇다면 한번에 가능할 방법이 있지 않을까?
	 *  
	 * 뒤쪽부터 계산 하면 가능 하다.
	 */
	static int powerWithMod(long a, long pow) {

		/*
		if(pow%2 == 1) {
			result = ((powerWithMod(a, pow/2) * powerWithMod(a, pow/2)) % modulo * a)  % modulo;
		} else {
			result = (powerWithMod(a, pow/2) * powerWithMod(a, pow/2)) % modulo;
		}
		*/
		
		long result = 1;
		long x = a;
				
		while(pow > 0) {
			
			if( (pow & 1) > 0) {
				result = (result * x) % modulo;
			}
			
			pow = pow >> 1;
			
			x = (x * x) % modulo;
		}
		
		return (int)result;
	}
	static long pluse(long a, long b) {
		return (a+b) % modulo;
	}
	
	static long sub(long a, long b) {
		return (a + (modulo - b))%modulo;
	}
	
	static long mul(long a, long b) {
		return (a*b) % modulo;
	}
	
	static long covertInt(String s) {
		long result = 0;
		for(int i = 0; i < s.length(); i++) {    		
    		if(s.charAt(i) == '1'){
    			result = pluse(result, powerWithMod(2, s.length() - (i+1)));
    		}
    	}
		
		return result;
	}
	

    static long xorAndSum(String a, String b) {
        /*
         * Write your code here.
         */

    	// b의 시작 지점    	
    	
    	int j = 0;
    	
    	//   1 0 1 1	: 4
    	// 1 0 1 1 1	: 5

    	int gap = 0;
    	
    	if(a.length() < b.length()) {
    		j = b.length() - a.length();
    	} else {
    		gap = a.length() - b.length();
    	}
    	
    	long powSum = 0;
    	long restSum = 0;
    	
		for(int i = 0; i < a.length() && j < b.length(); i++) {    		
    		if(a.charAt(i) == '1'){
    			powSum = pluse(powSum, powerWithMod(2, a.length() - (i+1)));			
    		}
    		
    		if(b.charAt(j) == '1') {
				// 자릿수가 같으면 증가
				if((j + gap) == i) {     					
					restSum = pluse(powSum, restSum);					
					j++;
				}
			} else {
				j++;
			}
    	}
		
		// 나머지 처리
		for( ; j < b.length() ; j++) {
			if(b.charAt(j) == '1') { 
				restSum = pluse(powSum, restSum);
			}
		}
    	
    	
    	restSum = mul(restSum, 2);
    	
    	long bSum = mul(covertInt(b), powerWithMod(2,until) - 1);
    	long aSum = mul(covertInt(a),until);
    	
    	return sub(pluse(bSum, aSum), restSum);    	
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
//        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String a = scanner.nextLine();

        String b = scanner.nextLine();

        long result = xorAndSum(a, b);
        System.out.println(result);

//        bufferedWriter.write(String.valueOf(result));
//        bufferedWriter.newLine();

//        bufferedWriter.close();
    }
}

```
