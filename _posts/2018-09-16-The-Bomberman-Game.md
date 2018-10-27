---
layout: post
title: The Bomberman Game
categories: HackerRank
tags:
- HackerRank
- Implementation
---

## **HackerRank link**
[The Bomberman Game](https://www.hackerrank.com/challenges/bomber-man/problem)


## **풀이 핵심**
1. 각 시점별로 특징이 있고, 반복이 된다.
2. 폭탄이 꽉차는 시점이라면 계산할 필요가 없다.
3. 구석진 곳에서는 처음과 다른 케이스가 나올 수 있으니 주의.

* 알고리즘이 어렵지 않는 문제.

## **Algorithm**
1. 1초 후 라면 그대로 리턴. (구할 필요가 없음)
2. String -> char 변환 (편집 용이성)
3. n초후를 조정한다. (특정 시점 이후 반복)
4. n초후의 폭탄 상황에 대해 구한다.
5. 마지막 상황 리턴

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

    static final char bomb = 79;
    static final char blank = '.';
    static final char willBeBlank = '1';
    
    static boolean debugPrint = false;
            
    static int[][] afterOneSec(int[][] grid)
    {
        int[][] dir =
        {
            // i , j 
                {+1,+0},    // 상
                {-1,+0}, // 하
                {+0,-1}, // 좌
                {+0,+1}, // 우
        };
        
        for(int i = 0 ; i < grid.length ; i++)
        {
            for(int j = 0 ; j < grid[i].length ; j++)
            {
                if(grid[i][j] != -2)
                {
                    grid[i][j]++;
                }
                
                if(grid[i][j] == 3)
                {
                    grid[i][j] = -2;
                    for(int k = 0; k < dir.length ; k++)
                    {
                        int x = j + dir[k][1];
                        int y = i + dir[k][0];
                        
                        if( x < 0 || x >= grid[i].length) continue;
                        if( y < 0 || y >= grid.length) continue;
                        
                        if(grid[y][x] != 3 && grid[y][x] != 2)
                        grid[y][x] = -2;
                    }
                }
            }
        }
        
        for(int i = 0 ; i < grid.length ; i++)
        {
            for(int j = 0 ; j < grid[i].length ; j++)
            {
                if(grid[i][j] == -2)
                {
                    grid[i][j] = -1;
                }
                if(debugPrint)
                {
                    if(grid[i][j] == -1)
                    {
                        System.out.print(blank);
                    }
                    else
                    {
                        System.out.print(grid[i][j]);
                    }
                }
                
                
            }
            
            if(debugPrint)System.out.println();
        }
        
        
        return grid;
    }
    
    // Complete the bomberMan function below.
    static String[] bomberMan(int n, String[] grid) {
        
        if(n == 1) return grid;
        
        char[][] ch = new char[grid.length][];
        
        for(int i = 0 ; i < grid.length ; i++)
        {
            ch[i] = new char[grid[i].length()];
        }
        
        int[][] map = new int[grid.length][];        
        for(int i = 0 ; i < grid.length ; i++)
        {
            map[i] = new int [grid[i].length()];
        }
        
        for(int i = 0 ; i < grid.length ; i++)
        {
            for(int j = 0; j < grid[i].length() ; j++)
            {
                if(grid[i].charAt(j) == bomb)
                {
                    map[i][j] = 1;
                }
                else
                {
                    map[i][j] = -1;
                }
            }
        }
        
        if( n % 2 == 0)
        {
            n = 2;
        }
        else if(n == 3)
        {
            // do nothing
        }
        else if((n+3) % 4 == 0) // when 4k - 1 = n  where k = 2,3,4... => n = 5,9,11 
        {
            n = 5;
        }
        else
        {
            n = 7;
        }
        
        // 2. after n sec (after 2sec)
        for(int i = 0 ; i < n-1; i++)
        {
            if(debugPrint)System.out.println((i+2) + "sec");
            map = afterOneSec(map);
            if(debugPrint)System.out.println();
        }
        
        for(int i = 0 ; i < grid.length ; i++)
        {
            for(int j = 0; j < grid[i].length() ; j++)
            {
                if(map[i][j] == -1)
                {
                    ch[i][j] = blank;
                }
                else
                {
                    ch[i][j] = bomb;
                }
                
            }
        }  
        
        String[] returnGrid = new String[grid.length];
        
        for(int i = 0 ; i < returnGrid.length ; i++)
        {
            returnGrid[i] = new String(ch[i]);
        }
        
        return returnGrid;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] rcn = scanner.nextLine().split(" ");

        int r = Integer.parseInt(rcn[0]);

        int c = Integer.parseInt(rcn[1]);

        int n = Integer.parseInt(rcn[2]);

        String[] grid = new String[r];

        for (int i = 0; i < r; i++) {
            String gridItem = scanner.nextLine();
            grid[i] = gridItem;
        }

        String[] result = bomberMan(n, grid);

        for (int i = 0; i < result.length; i++) {
            bufferedWriter.write(result[i]);

            if (i != result.length - 1) {
                bufferedWriter.write("\n");
            }
        }

        bufferedWriter.newLine();

        bufferedWriter.close();

        scanner.close();
    }
}


{% endhighlight %}
