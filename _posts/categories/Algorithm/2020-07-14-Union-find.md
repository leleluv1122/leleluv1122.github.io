---
title: "Union-find(유니온 파인드)/disjoint-set 알고리즘 + 백준추천"
categories: Algorithm
comments: true
---

## 사용언어
 > Visual studio 2019 C++ 

## 생각하기
 Union-find(유니온 파인드)/disjoint-set 알고리즘

## 설명

union연산과 find연산을 사용하는 자료구조이다.  

```
    2		3	7
1   5   6		4
      8
```

{1, 2, 5, 6, 8}, {3, 4}, {7}
위와 같이 구성된 트리가 존재한다고 생각해보자  
find 연산으로 정점의 루트를 찾을 수 있다.  만약, 자신이 루트라면 자신을 리턴한다.  

```c++
int find(int a) {
	if(p[a] == a)
		return a;
	return p[a] = find(p[a]);
}
```

이제 루트들을 find연산을 실행했으면 union연산을 해볼건데 이건 두 집합을 하나로 합쳐줄 때 사용한다.  

```c++
void merge(int a, int b) {
	a = find(a);
	b = find(b);

	if(a == b)
		return;
	p[a] = b;
}
```


<https://www.acmicpc.net/problem/16562>

이 문제를 예시로 보자

```c++
#include <iostream>
#include <algorithm>

#define MAX 10001
#define INF 2e9
#define endl "\n"
using namespace std;

int fcost[MAX];
int p[MAX];
int total[MAX];

void init() {
	ios::sync_with_stdio(false);
	cin.tie(0);
	cout.tie(0);
}

int find(int a) {
	if (a == p[a])
		return a;
	return p[a] = find(p[a]);
}

void friend_union(int a, int b) {
	a = find(a);
	b = find(b);

	if (a == b)
		return;
	p[a] = b;
}

int main() {
	init();

	int n, m, k;
	cin >> n >> m >> k;

	for (int i = 0; i <= n; i++)
		p[i] = i;

	for (int i = 0; i < n; i++)
		cin >> fcost[i];
	
	int v, w;
	for (int i = 0; i < m; i++) {
		cin >> v >> w;
		friend_union(v - 1, w - 1);
	} // 이 작업을 통해 p 배열에는 root 가 들어가있다.
	
	fill(total, total + n, INF);

	// 집합안의 가장작은 cost를 root에 넣기
	for (int i = 0; i < n; i++)
		total[find(i)] = min(total[find(i)], fcost[i]); 
	
	int sum = 0;
	for (int i = 0; i < n; i++) {
		if (total[i] >= INF)
			continue;
		sum += total[i];
	}

	if (sum <= k)
		cout << sum << endl;
	else
		cout << "Oh no" << endl;
}
```


## 백준 문제 추천
 - [1717 집합의 표현](https://www.acmicpc.net/problem/1717)
   - 풀이: [1717 풀이](https://github.com/leleluv1122/Algorithm/blob/master/_BAEKJOON_/_BAEKJOON_/1717_%EC%A7%91%ED%95%A9%EC%9D%98_%ED%91%9C%ED%98%84.cpp)
 - [1976 여행 가자](https://www.acmicpc.net/problem/1976)
   - 풀이: [1976 풀이](https://github.com/leleluv1122/Algorithm/blob/master/_BAEKJOON_/_BAEKJOON_/1976_%EC%97%AC%ED%96%89%EA%B0%80%EC%9E%90.cpp)
 - [16562 친구비](https://www.acmicpc.net/problem/16562)
   - 풀이: [16562 풀이](https://github.com/leleluv1122/Algorithm/blob/master/_BAEKJOON_/_BAEKJOON_/16562_%EC%B9%9C%EA%B5%AC%EB%B9%84.cpp)
 - [4195 친구 네트워크](https://www.acmicpc.net/problem/4195)
   - 풀이: [4195 풀이](https://github.com/leleluv1122/Algorithm/blob/master/_BAEKJOON_/_BAEKJOON_/4195_%EC%B9%9C%EA%B5%AC_%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC.cpp)