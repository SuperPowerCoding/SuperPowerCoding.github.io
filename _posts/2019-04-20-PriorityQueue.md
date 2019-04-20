---
layout: post
title: PriorityQueue
categories: DataStructure
tags: 
- Data Structure
- PriorityQueue
- C++
---

# **PriorityQueue**
큐는 선입 선출을 위한 자료구조이다.  

이 때문에 큐는 배열 또는 링크드 리스트로 구현이 가능하다.  

우선순위 큐는 우선 순위가 있는 큐이다. 

먼저 들어온 데이터가 우선이 되지 않고, 우선순위가 높은 데이터가 먼저나가는 구조이다.  

이 때문에, Heap을 이용하여 구현한다.  

Heap 구조를 이용하기 때문에 시간 복잡성은 O(logn) 이다.

[추상자료형]
|         | name | input | output | 설명 |
|-------|--------|---------|---------|---------| 
|데이터 전부 삭제   | clear  | -      | - | 데이터를 전부 삭제 한다.|
|큐 사이즈 정보 반환| size     | -      | size | 큐의 사이즈 정보를 반환한다. |
|데이터 추가       | add  | 데이터      | - | 큐에 데이터를 추가한다.|
|데이터 poll       | poll      |  - | 데이터 | 큐에서 데이터를 poll 한다.|
|데이터 peek       | peek      |  - | 데이터 | 큐에서 데이터를 peek 한다.|

poll은 큐에서 데이터를 완전히 받아와서 큐에서는 해당 데이터가 존재 X  
peeK는 큐의 데이터를 살짝 볼 수 있도록 데이터를 받지만 삭제되지 않는다.

# **사용예**
대표적으로 우선순위 큐는 MST(Minimal Spanning Tree)를 구현하는데 많이 사용한다.  
- Prim's MST
- Kruskal MST  

둘다 최소의 간선을 잇는 알고리즘으로 먼저 우선순위 큐에 간선 정보를 입력하고 최소 간선을 받아서 잇는다.

다른 사용 예로는 무언가 우선순위가 필요한 시뮬레이션이라든지 이런 프로그램 구현시 사용 가능하다.  

ex) 생산량이 다른 두 아르바이트 생의 커피 생산 시뮬레이션, 은행, 생산 등


# **Source Code**
```c++
#include <iostream>

using namespace std;

class Element {
public :	
	int key;
	int item;
	Element(int key) {
		this->item = 0;
		this->key = key;
	}

	Element() {
		this->item = 0;
		this->key = 0;
	}

	// 요것만 수정하면 최대/최소로 바뀐다.
	int compare(Element newNode) {
		return this->key <= newNode.key ? -1 : 1;	// 최소 
		// return this->key >= newNode.key ? -1 : 1; // 최대
	}
};

class PriorityQueue {

private :
	int totalSize;
	int maxSize;
	Element * data;

public :
	PriorityQueue(int totalSize) {
		
		this->totalSize = 0;
		// 편의상 0번은 버릴 것이기 때문에 +1을 해준다.
		this->maxSize = totalSize+1;
		data = new Element[totalSize+1];
	}

	~PriorityQueue() {
		delete[] data;
	}

	void clear() {
		totalSize = 0;
	}

	int size() {
		return totalSize;
	}

	void add(Element newitem) {
		// 1. 일단 맨 뒤에 넣는다고 생각하고
		// 2. 상위 부모와의 역전 관계를 확인한다.

		// 예외 처리. 사실 상  초기화 시에 입력으로 올 데이터 만큼 크게 선언해준다.
		if (totalSize + 1> maxSize) {
			return;
		}


		int child = ++totalSize;
		int parent = child / 2;

		while (parent != 0) {

			// 상위 부모와의 역전 관계를 확인한다.
			// 바꾸지 말아야 한다면, 그만 한다.
			if (data[parent].compare(newitem) < 0) {
				break;
			}

			// 바꿔야 되니까 자식과 부모를 바꾸고
			data[child] = data[parent];

			// 상위로 올라가서 다시 검사한다.
			child = parent;
			parent = child / 2;
		}

		data[child] = newitem;

	}

	// 최상위 값을 보여준다.
	Element peek() {
		return data[1];
	}

	Element poll() {
		Element returnData = data[1];
		int tail = totalSize--;

		// 맨 아래가 헤드로 온다고 생각하고 들어갈 자리를 찾는다.
		int parenet = 1;
		int child = 2;

		while (child <= totalSize) {
			
			// 자식 둘중 더 최소 또는 최대에 맞는지 확인한다.
			if (child + 1 <= totalSize && data[child + 1].compare(data[child]) < 0) {
				child++;
			}

			// 바꿔야 되는지 확인한다.
			if (data[tail].compare(data[child]) < 0) {
				break;
			}

			data[parenet] = data[child];
			
			parenet = child;
			child = parenet * 2;

		}

		data[parenet] = data[tail];

		return returnData;
	}
};


int main() {

	PriorityQueue * pq = new PriorityQueue(1024);

	pq->add(Element(5));
	pq->add(Element(1));
	pq->add(Element(2));
	pq->add(Element(7));
	pq->add(Element(9));
	pq->add(Element(11));
	pq->add(Element(4));
	pq->add(Element(8));

	while (pq->size() != 0) {
		cout << pq->poll().key << endl;
	}
}
```