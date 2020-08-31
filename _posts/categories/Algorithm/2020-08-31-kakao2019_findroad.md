---
title: "프로그래머스(programmers) 2019 KAKAO BLIND RECRUITMENT 길찾기 게임 풀이"
categories: Algorithm
comments: true
---

## 사용언어
 > Visual studio 2019 C++  

문제링크 : <https://programmers.co.kr/learn/courses/30/lessons/42892?language=cpp>

## 길찾기 게임 풀이
sort 이후 노드를 어떻게 tree형태로 연결할 것인지 하루를 고민하다가 풀이를 찾아봤다.  
참고한 고수의 풀이: <https://chosh95.tistory.com/323>  

```c++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

struct Node {
	int x;
	int y;
	int idx;
	Node* left; // 노드 포인터로 가리킴
	Node* right;
};

vector<Node> tree;
vector<int> pre;
vector<int> post;

void insert(Node* parent, Node* child) {
	if (parent->x < child->x) { // parent의 x보다 child의 x가 더 크면 오른쪽 노드에 위치
		if (parent->right == NULL) 
			parent->right = child;
		else // 오른쪽 노드가 안비어있다면 그 오른쪽노드에서 다시한번 insert하기
			insert(parent->right, child);
	}
	else { // 위와 같이 왼쪽노드..!
		if (parent->left == NULL)
			parent->left = child;
		else
			insert(parent->left, child);
	}
}

bool compare(Node& a, Node& b) { // 비교문
	if (a.y == b.y)
		return a.x < b.x; // y가 같으면 x가 작은것부터 sort
	return a.y > b.y; // y가 큰것부터 sort
}

void preorder(Node* cur) { // 전위순회 중간 -> 왼쪽 -> 오른쪽
	if (cur == NULL)
		return;
	pre.push_back(cur->idx);  
	preorder(cur->left);
	preorder(cur->right);
}

void postorder(Node* cur) { // 후위순회 왼쪽 -> 오른쪽 -> 중간
	if (cur == NULL)
		return;
	postorder(cur->left);
	postorder(cur->right);
	post.push_back(cur->idx);
}

vector<vector<int>> solution(vector<vector<int>> nodeinfo) {
	for (int i = 0; i < nodeinfo.size(); i++)
		tree.push_back({ nodeinfo[i][0], nodeinfo[i][1], i + 1 });
	sort(tree.begin(), tree.end(), compare);
	
	Node* root = &tree[0];
	for (int i = 1; i < tree.size(); i++)
		insert(root, &tree[i]);
	preorder(root);
	postorder(root);

	vector<vector<int>> answer;
	answer.push_back(pre);
	answer.push_back(post);

	return answer;
}

// 맞았는지 체크하는부분 없애고 제출해야됨!
int main() {
	vector<vector<int>> r;
	r.resize(9);

	r[0].push_back(5);
	r[0].push_back(3);
	r[1].push_back(11);
	r[1].push_back(5);
	r[2].push_back(13);
	r[2].push_back(3);
	r[3].push_back(3);
	r[3].push_back(5);
	r[4].push_back(6);
	r[4].push_back(1);
	r[5].push_back(1);
	r[5].push_back(3);
	r[6].push_back(8);
	r[6].push_back(6);
	r[7].push_back(7);
	r[7].push_back(2);
	r[8].push_back(2);
	r[8].push_back(2);

	vector<vector<int>> result = solution(r);
}
```

포인터랑 주소값 사용하니까 완존복잡.. 어렵다잉...