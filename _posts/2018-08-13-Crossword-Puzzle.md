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
java
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
c++
```c++
#include <assert.h>
#include <limits.h>
#include <math.h>
#include <stdbool.h>
#include <stddef.h>
#include <stdint.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

char* readline();

// Complete the crosswordPuzzle function below.

// Please store the size of the string array to be returned in result_count pointer. For example,
// char a[2][6] = {"hello", "world"};
//
// *result_count = 2;
//
// return a;
//
typedef struct _point {
	int x;
	int y;
	int dir;
}point;

point * search(char** crossword, char * word, int * num) {
	point * p = (point *)malloc(sizeof(point) * 20);
	int found = 0;
	int len = strlen(word);

	// 가로
	for (int i = 0; i < 10; i++) {
		for (int j = 0; j < 10; j++) {
			// 들어 갈 수 있는 공간을 만나면,
			if (crossword[i][j] != '+') {

				// 빈공간은 그냥 들어갈 수 있음.
				// 빈공간이 아니라면, 알파벳이 맞아야 됨.
				// 가로 방향
				if (j + len <= 10) {
					int count = 0;
					for (int k = 0; k < len; k++) {
						if (crossword[i][j + k] != '-') {
							if (crossword[i][j + k] != word[k]) {
								break;
							}
							count++;
						}
						else {
							count++;
						}
					}

					if (count == len) {
						p[found].x = i;
						p[found].y = j;
						p[found++].dir = 1;
					}
				}

				// 세로 방향
				if (i + len <= 10) {
					int count = 0;
					for (int k = 0; k < len; k++) {
						if (crossword[i+k][j] != '-') {
							if (crossword[i+k][j] != word[k]) {
								break;
							}
							count++;
						}
						else {
							count++;
						}
					}

					if (count == len) {
						p[found].x = i;
						p[found].y = j;
						p[found++].dir = -1;
					}
				}
			}
		}
	}
	
	*num = found;

	return p;
}

char stack[10][11];
int sp = 0;

int clear = 0;

void push(char * word) {
	int len = strlen(word);

	strcpy(stack[sp++], word);

	// stack[sp++][len + 1] = 0;
}

char* pop(){
	return stack[--sp];
}


void fillWord(char** crossword, char * word, point * p) {
	
	int len = strlen(word);

	// 가로
	if (p->dir == 1) {
		for (int i = 0; i < len; i++) {
			crossword[p->x][p->y + i] = word[i];
		}
	}
	else {
		for (int i = 0; i < len; i++) {
			crossword[p->x +i][p->y] = word[i];
		}
	}
}

void reversWord(char** crosswordPuzzle, char * word, point * p) {

	int len = strlen(word);

	// 가로
	if (p->dir == 1) {
		for (int i = 0; i < len; i++) {
			crosswordPuzzle[p->x][p->y + i] = '-';
		}
	}
	else {
		for (int i = 0; i < len; i++) {
			crosswordPuzzle[p->x + i][p->y] = '-';
		}
	}
}

void print(char ** crossword) {
	for (int i = 0; i < 10; i++) {		
		printf("%s\n", crossword[i]);
	}
	puts("\n");
}

char** result;

void solve(char** crossword) {

	// 완료 됨.
	if (clear == 1) return;

	if (sp == 0) {
		// print(crossword);
		for (int i = 0; i < 10; i++) {
			strcpy(result[i], crossword[i]);
		}
		clear = 1;
		return;
	}

	print(crossword);

	char * word = pop();
	int found = 0;
	point * points = search(crossword, word, &found);

	for (int i = 0; i < found; i++) {

		// 채운다.
		fillWord(crossword, word, (points+i));

		// 다음으로 서치 한다.
		solve(crossword);
		
		// 돌려놓는다.
		reversWord(crossword, word, (points + i));
	}

	push(word);

	free(points);
}

char** crosswordPuzzle(int crossword_count, char** crossword, char* words, int* result_count) {

	// 문자열 나누기
	char ** separatedWord = (char **)malloc(10 * sizeof(char*));
	result = (char **)malloc(10 * sizeof(char*));
	char * pw = words;
	
	for (int i = 0; i < 10; i++) {
		separatedWord[i] = (char *)malloc(10 * sizeof(char) + 1);
		result[i] = (char *)malloc(10 * sizeof(char) + 1);;
	}
	
	int idx = 0;
	int widx = 0;

	while (*pw) {		
		if (*pw == ';') {
			separatedWord[widx][idx] = '\0';
			idx = 0;
			widx++;
		}
		else {
			separatedWord[widx][idx++] = *pw;
		}

		pw++;
	}
	separatedWord[widx][idx] = '\0';

	for (int i = 0; i <= widx; i++) {
		push(separatedWord[i]);
	}

	solve(crossword);


	*result_count = 10;

	for (int i = 0; i < 10; i++) {
		free(separatedWord[i]);
	}
	free(separatedWord);

	return result;
}

int main()
{
	// FILE* fptr = fopen(getenv("OUTPUT_PATH"), "w");

	char** crossword = (char **)malloc(10 * sizeof(char*));

	for (int i = 0; i < 10; i++) {
		char* crossword_item = readline();

		*(crossword + i) = crossword_item;
	}

	int crossword_count = 10;

	char* words = readline();

	int result_count;
	char** result = crosswordPuzzle(crossword_count, crossword, words, &result_count);

	for (int i = 0; i < result_count; i++) {
		// fprintf(fptr, "%s", *(result + i));
		printf("%s", *(result + i));

		if (i != result_count - 1) {
			// fprintf(fptr, "\n");
			printf("\n");
		}
	}

	for (int i = 0; i < 10; i++) {
		free(result[i]);
	}
	free(result);

	// fprintf(fptr, "\n");
	printf("\n");

	// fclose(fptr);

	return 0;
}

char* readline() {
	size_t alloc_length = 1024;
	size_t data_length = 0;
	char* data = (char *)malloc(alloc_length);

	while (true) {
		char* cursor = data + data_length;
		char* line = fgets(cursor, alloc_length - data_length, stdin);

		if (!line) {
			break;
		}

		data_length += strlen(cursor);

		if (data_length < alloc_length - 1 || data[data_length - 1] == '\n') {
			break;
		}

		alloc_length <<= 1;

		data = (char *)realloc(data, alloc_length);

		if (!line) {
			break;
		}
	}

	if (data[data_length - 1] == '\n') {
		data[data_length - 1] = '\0';

		data = (char *)realloc(data, data_length);
	}
	else {
		data = (char *)realloc(data, data_length + 1);

		data[data_length] = '\0';
	}

	return data;
}

```
