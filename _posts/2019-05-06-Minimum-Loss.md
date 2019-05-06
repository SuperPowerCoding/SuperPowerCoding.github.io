---
layout: post
title: Minimum Loss
categories: HackerRank
tags: 
- HackerRank
- Search
- HashMap
---

## **HackerRank link**
[Minimum Loss](https://www.hackerrank.com/challenges/minimum-loss/problem)


## **풀이 핵심**
본인 풀이
1. 어떤 값을 둘의 차이가 최소인 두 년도를 찾아야 한다. → sort 후 앞뒤 뺌
2. 두 값의 앞뒤가 앞은 크고 뒤는 작아야됨.            → HashMap으로 판단

Editorial  
이진트리를 이용한다.


## **Algorithm**
1. 입력 시 HashMap에 키(price)와 index를 저장한다.
2. sort를 한다.
3. 차례데로 살펴 보면서 Hash 맵을 이용하여, 두 사이의 gap이 작은 부분을 캐치 한다.

## **Source Code**
```java
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.regex.*;

import javax.swing.plaf.synth.SynthSeparatorUI;

public class Solution {

    static HashMap<Long, Integer> map = new HashMap<>();
    // Complete the minimumLoss function below.
    static long minimumLoss(long[] price) {
        long min = Integer.MAX_VALUE;
        Arrays.sort(price);
        
        // 오름차순
        // 20 [7] 8 2 [5]
        // 2 5 7 8 20
        for(int i = 0; i < price.length - 1; i++) { 
            // 두 값이 뭐가 됐든 더 작은 값의 키 값이 더 커야된다.
            // 더 큰게 왼쪽 - 더 작은게 오른쪽          
            if(map.get(price[i]) - map.get(price[i+1]) > 0) {
                long dif = price[i+1] - price[i];                
                if(dif < min) min = dif;
            }
        }
        
        return min;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int n = scanner.nextInt();
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        long[] price = new long[n];

        String[] priceItems = scanner.nextLine().split(" ");
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        for (int i = 0; i < n; i++) {
            long priceItem = Long.parseLong(priceItems[i]);
            price[i] = priceItem;
            map.put(priceItem, i);
        }

        long result = minimumLoss(price);
        
        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedWriter.close();

        scanner.close();
    }
}
```

editorial
```cpp
#include<assert.h>

#define pf printf
#define sf scanf

using namespace std;

typedef long long vlong;

const vlong inf = 1000000000000000000LL;


vlong arr[200005];
void solution() {

	set<vlong>st;
	set<vlong>::iterator up;

	int n;
	vlong temp, ans = inf, mx = 0;

	sf("%d", &n);
	assert(n>1 && n <= 200000);

	for (int i = 1; i <= n; i++)
	{
		sf("%lld", &arr[i]);
		assert(arr[i]>0 && arr[i] <= 10000000000000000LL);
	}

	st.insert(arr[1]);
	mx = max(mx, arr[1]);
	for (int i = 2; i <= n; i++)
	{
		if (mx > arr[i])
		{
			up = st.upper_bound(arr[i]);
			temp = *up - arr[i];
			ans = min(ans, temp);
		}
		st.insert(arr[i]);
		mx = max(mx, arr[i]);
	}

	assert(ans != inf);

	pf("%lld\n", ans);
}


int main() {

	solution();

	return 0;
}
```