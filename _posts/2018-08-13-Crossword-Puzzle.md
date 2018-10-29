---
layout: post
title: Crossword Puzzle
categories: HackerRank
tags:
- HackerRank
- Recursion
---


## **HackerRank link**
[Crossword Puzzle](https://www.hackerrank.com/challenges/crossword-puzzle/problem?h_r=internal-search)


## **풀이 핵심**
1. 십자말 퍼즐에 전부 단어를 넣어 보는 수 외에는 없다.
2. 십자말에 지속적으로 단어를 넣는 과정은 재귀함수를 통하여 구현한다.

* 알고리즘이 어렵지 않는 문제
* 유사 문제: [Ema's Supercomputer](https://superpowercoding.github.io/hackerrank/2018/09/19/Ema's-Supercomputer/)


## **Algorithm**
1. 다 풀렸는지 체크 한다. 다 풀렸으면 정답을 리턴한다.
2. Stack으로 부터 넣을 단어를 가져온다.
3. 현재 단어가 들어갈 위치를 구한다.
4. 해당 단어가 넣을 곳이 있다면, 차례 차례 그 단어가 들어갈 위치에 순서데로 단어를 넣는다.
5. 다음 단어를 넣는다. (재귀 호출 과정 1부터 다시)
6. 넣었던 단어 빈 공간으로 다시 원상 복귀 시키고, 다음 위치에 단어를 넣는다. 과정3.
7. 과정 3에서 단어를 넣을 곳이 없다면, stack에 현재 단어를 다시 넣는다.


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

    static final char TO_BE_FILLED = '-';
    static final int VERTICAL = 0;
    static final int HORIZON = 1;
    
    static class Direction
    {
        int x;
        int y;
        int dir;
    }
    
    static String[] result;
    static Stack<String> words = new Stack<>();
    static boolean debugPrint = false;
    static boolean firstTime = true;
    
    static boolean isSolved()
    {
        return words.isEmpty();
    }
    
    static ArrayList<Direction> getDirections(char[][] crossword, String word)
    {
        ArrayList<Direction> dir = new ArrayList();
        
        for(int i = 0 ; i < crossword.length; i++)
        {
            for(int j = 0 ; j < crossword[i].length; j++)
            {
                boolean horizonDir = true;
                boolean verticalDir = true;
                
                for(int k = 0 ; k < word.length() ; k++)
                {
                    // check horizon dir
                    if((j+k < crossword[i].length) && (crossword[i][j+k] != TO_BE_FILLED && crossword[i][j+k] != word.charAt(k)))
                    {
                        horizonDir = false;
                    }
                    
                    // check vertical dir
                    if((i+k < crossword.length) && (crossword[i+k][j] != TO_BE_FILLED && crossword[i+k][j] != word.charAt(k)))
                    {
                        verticalDir = false;
                    }
                    
                }
                        
                if(horizonDir && (j+word.length() < crossword[i].length+1))
                {
                    // 좌표 추가
                    Direction d = new Direction();
                    d.dir = HORIZON;
                    d.x = j;
                    d.y = i;
                    
                    dir.add(d);
                }
                
                if(verticalDir && (i+word.length() < crossword.length+1))
                {
                    // 좌표 추가
                    Direction d = new Direction();
                    d.dir = VERTICAL;
                    d.x = j;
                    d.y = i;
                    
                    dir.add(d);
                }    
            }            
        }
        
        return dir;
    }
    
    static void printCrossWord(char[][] crossword)
    {
        System.out.println();
        for(int i = 0 ; i< crossword.length ; i++)
        {
            for(int j = 0 ; j< crossword.length ; j++)
            {
                System.out.print(crossword[i][j]);
            }
            System.out.println();
        }
    }
    
    
    static void solve(char[][] crossword)
    {
        if(isSolved())
        {
            // save and return;
            if(debugPrint)printCrossWord(crossword);
            
            if(firstTime)
            {
                result = new String[crossword.length];
                for(int i = 0 ; i < crossword.length ; i++)
                {
                    result[i] = new String(crossword[i]);
                    System.out.println(result[i]);
                }
                firstTime = false;
            }
            
            
            return ;
        }        
        
        if(debugPrint)printCrossWord(crossword);
        
        String word = words.pop();
        
        ArrayList<Direction> dirs = getDirections(crossword, word);
        
        for(Direction dir : dirs)
        {
            // fill word            
            for(int j = 0 ; j < word.length() ; j++)
            {
                if(dir.dir == VERTICAL)
                {
                    crossword[dir.y + j][dir.x] = word.charAt(j);
                }
                else
                {
                    crossword[dir.y][dir.x + j] = word.charAt(j);
                }
            }
            
            // next word
            solve(crossword);
            
            // restore 
            for(int j = 0 ; j < word.length() ; j++)
            {
                if(dir.dir == VERTICAL)
                {
                    crossword[dir.y + j][dir.x] = TO_BE_FILLED;
                }
                else
                {
                    crossword[dir.y][dir.x + j] = TO_BE_FILLED;
                }
            }
        }
        
        words.push(word);
        
    }
    
    
    
    // Complete the crosswordPuzzle function below.
    static String[] crosswordPuzzle(char[][] crossword) {        
        
        solve(crossword);
        return result;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] crossword = new String[10];

        for (int i = 0; i < 10; i++) {
            String crosswordItem = scanner.nextLine();
            crossword[i] = crosswordItem;
        }

         String[] temp = scanner.nextLine().split(";");
        
        for(int i = 0 ; i < temp.length ; i++)
        {
            words.push(temp[i]);
            if(debugPrint)System.out.println(temp[i]);
        }

        char[][] cw = new char[crossword.length][];
        
        for(int i = 0; i < crossword.length ; i++)
        {
            cw[i] = crossword[i].toCharArray();
        }
        
        result = crosswordPuzzle(cw);

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
```
