---
title:  "레드블랙 트리(삽입)"


categories:
  - Algorithm
tags:
  - Algorithm
last_modified_at: 2022-01-08T08:22:00-05:00
---


 오늘은 레드 블랙 트리구조에 대해 알아보도록 하겠습니다. 레드 블랙 트리구조는, 이진 탐색 트리중에서도 특수한 형태를 가진 균형 이진 트리입니다.

 일반적인 이진 탐색 트리의 경우에는 삽입 시 루트 노드로부터 비교를 시작하여 삽입될 수가 루트노드 값보다 크다면 삽입될 수와 오른쪽 자식노드 값의 비교를 하고 루트노드 값보다 작다면 왼쪽 자식 노드 값과 비교를 합니다.
 각각의 노드에 대해서도 같은 방법으로 비교를 해나가며, 삽입될 값이 어느 노드의 값보다 크고 그 노드의 오른쪽 자식이 없다면 오른쪽 자식 노드의 자리에 값을 삽입하고, 삽입될 값이 그 노드의 값보다 작고 그 노드의 왼쪽 자식이 없다면 그 노드의 왼쪽 자식 노드 자리에 값을 삽입합니다.

 일반적으로 탐색, 삽입, 삭제 시 시간 복잡도가 O(log n)이겠지만 이전에 삽입한 수보다 큰 값을 계속 삽입한다면 트리가 선형 구조가 되므로 트리의 높이가 n이되어 최악의 경우 시간 복잡도가 O(n)이 나올 수 있습니다.

그러나, 레드 블랙 트리의 경우에는 삽입, 삭제 시 편파적으로 배치되지 않도록 기존 이진 탐색 트리의 조건에 규칙, 조건을 추가해 보다 균형잡힌 구조로 만들어 항상 일정한 실행시간을 보장받을 수 있도록 고안되어 최악의 경우에도 O(log n)이라는 시간 복잡도를 보장받을 수 있습니다.       
이러한 특성때문에 cpp stl에서 제공하는 map,set 컨테이너 등 많은 곳에 이용이 됩니다.

레드블랙 트리의 조건은 아래와 같습니다.


    1. 모든 노드는 빨간색 혹은 검은색이다.

    2. 루트 노드는 검은색이다.

    3. 모든 리프 노드(NIL)들은 검은색이다. (NIL : null leaf, 값을 갖지 않는 트리의 끝을 나타내는 노드)

    4. 빨간색 노드의 자식은 검은색이다.(빨간색 노드가 연속으로 나올 수 없다.double red)->검은색 노드의 자식은 검은색일수도, 빨간색일수도 있다.

    5. 모든 리프 노드에서 Black Depth는 같다. (리프노드에서 루트 노드까지 가는 경로에서 만나는 검은색 노드의 개수가 같다.)




위 조건을 만족하도록 삽입하는 과정을 한번 살펴보도록 하겠습니다..


    새로 삽입하는 원소를 N(New) 

    N의 부모원소를 P(Parent)

    P의 형제,N의 삼촌 요소를 U(Uncle)

    P,U의 부모 요소를 G(GrandFather)

라고 합시다,

우선 삽입 시 노드의 색깔 속성은 무조건 빨간색입니다. 그 이후에 주변 노드의 상태에 따라 조건에 맞도록 색과 형태를 조정해 주는 작업을 합니다.

조건 1,3은 삽입 시에 위반행위에 영향을 주지 않으므로 조건 2,4,5에 대해서만 신경쓰도록 합시다. 
* * *
# 첫번째 case
트리에 원소가 없는 경우 첫 삽입 시 삽입되는 원소는 루트 원소가 되므로 조건2에 의해 삽입 후 원소 색을 빨간색에서 검은색으로 변경해줍니다.  

* * *

# 두번째 case

붉은 색 원소 N에대해 부모노드 P가 검은색이라면 N을 추가해도 조건 4,5에 영향을 미치지 않으므로 넘어가 줍시다. 

* * *

그럼 부모노드가 빨간색일 경우만 보면 되겠군요. 새 노드 N이 빨간색이므로 다음 과정부터는 double red가 무조건 생기며, 부모P의 형제 노드U의 색을 보고 어떻게 처리할지 판단을 해야합니다.(형제 노드는NIL(검은색)이 될 수도 있습니다.)

* * *

# 세번째 case

부모 노드 P와 삼촌 노드 U가 모두 붉은색 노드일 경우, 빨간 노드가 연속으로 나오는 double red 현상이 발생했습니다.

double red 현상을 해결하고, 조건 5를 유지하기 위해서, P와 U를 모두 검은색으로 바꾸고 할아버지 노드 G를 붉은색으로 바꿉니다.(recoloring)


그러나 할아버지 노드G를 붉은 색으로 바꾸는 작업을 했기 때문에 G의 부모 노드가 검은 색이라면 문제가 없겠지만 붉은 색이었다면 또다시 double red 현상이 발생하여 G의 부모노드 입장에서는 빨간 노드를 자식으로 삽입한 것과 같은 효과가 있으므로 상황에 따라 재귀적으로 **recoloring**을 하거나 **restructuring**을 합니다. 

(위 선택을 결정 짓는 요소는 삼촌 요소의 색이 됩니다.삼촌요소가 붉은 색이라면 recoloring 검은색이라면restructuring)

![image](/assets/images/Red-black_tree_insert_case_3.png)

그림의 삼각형은 노드에 달린 서브트리입니다.
* * *

# 네번째 case

네번째 경우는 부모 노드 P는 붉은색이지만 삼촌 노드 U는 검은색일때, 새로운 노드 N이 P의 **왼쪽** 자식 노드이고, P가 할아버지 노드 G의 왼쪽 자식 노드인 상황입니다. 참고로, 레드블랙 트리 구조가 전제된 상황에서 붉은 노드를 추가하는 것이기 때문에 G는 검은색입니다. 

이 경우 G에 대해 오른쪽 회전을 합니다.(P가 루트, G가 오른 쪽 자식이 되도록 회전,회전 시 P의 오른쪽 서브트리는 G의 왼쪽 자식으로 넘깁니다.) 결과적으로 부모 노드였던 P는 새로운 노드 N과 할아버지 노드 G를 자식 노드로 갖게 됩니다. 

그 후 조건 5를 만족시키기 위해 P(빨간색에서 검은 색으로)와 G(검은색에서 빨간색으로)의 색을 반대로 바꾸면 모든 서브트리에 영향을 주지 않고 모든 조건을 충족시킨 채로 삽입이 완료됩니다.
(restructuring)

![image](/assets/images/Red-black_tree_insert_case_4.png)

* * *

# 다섯번째 case

다섯번 째는 부모 노드 P가 붉은색인데, 삼촌 노드 U는 검은색일때, 새로운 노드 N이 P의 **오른쪽** 자식 노드이고, P는 할아버지 노드 G의 왼쪽 자식 노드인 상황입니다. 네번째 case와 같이 G는 검은 색입니다.

이 경우, N과 P의 역할을 변경하기 위해서 왼쪽 회전을 하게 합니다.(N이 루트, P가 왼쪽 자식이 되도록 회전, 회전 시 N의 왼쪽 서브 트리는 P의 오른쪽 자식이 되도록 조정한다.) 그 후, 모든 색깔과 모양이 네번 째 케이스와 같은 상황에 놓이게 되므로 P가 네번 째 케이스의 N, N이 네번째 케이스의 P에 대입해서 네번째 케이스의 처리를 해주면 됩니다.

![image](/assets/images/Red-black_tree_insert_case_5.png) -->
![image](/assets/images/Red-black_tree_insert_case_4.png)

* * *

4번째, 5번째 경우는 결과적으로 P,N,G를 이진트리의 규칙대로 부모와 자식관계로 엮고, 조건 4,5를 만족시키기 위해 회전 후 부모를 검은색으로  자식들은 빨간색으로 조정하고, P,N,G,U에 달린 서브트리들에 대해 (black depth)가 이전과 같도록 배치해줍다.   
(restructuring 해줍니다)

네번째, 다섯번째 case에 대하여 노드 P가 노드G의 오른 쪽 자식인 경우에는 방향만 바꿔서 생각해 주면 됩니다.
조건 4에 대해서 최단 경로는 검은색 노드만 있는 경우이고, 최장경로는 검은색과 빨간생이 번갈아 나오는 경우일 것이므로 모든 경로는 최단경로의 2배보다 작거나 같아 이전보다 훨씬 균형잡힌 구조가 되었습니다.

다음은 코드를 보겠습니다.


#  삽입 코드
### 레드블랙 트리의 노드 구성요소와 red-black트리 class(틀)입니다.
```cpp
 /** C++ implementation for
Red-Black Tree Insertion
This code is adopted from
the code provided by
Dinesh Khandelwal in comments **/
#include <bits/stdc++.h>
using namespace std;

enum Color {RED, BLACK};

struct Node
{
	int data;
	bool color;
	Node *left, *right, *parent;

	// Constructor
	Node(int data)
	{
	this->data = data;
	left = right = parent = NULL;

    //새로 삽입되는 노드는 항상 빨간색 입니다.
	this->color = RED;
	}
};

// Class to represent Red-Black Tree
class RBTree
{
private:
	Node *root;
protected:
	void rotateLeft(Node *&, Node *&);
	void rotateRight(Node *&, Node *&);
	void fixViolation(Node *&, Node *&);
public:
	// Constructor
	RBTree() { root = NULL; }
	void insert(const int &n);
	void inorder();
	void levelOrder();
};
```

### 레드블랙트리를 오름차순으로 프린트하도록 도와주는 메소드와 레벨 순서로 프린트하도록 도와주는 메소드입니다. 
```cpp

// A recursive function to do inorder traversal
//노드의 값이 오름 차순으로 출력되도록 재귀순회합니다.(중위 순회)
void inorderHelper(Node *root)
{
	if (root == NULL)
		return;

	inorderHelper(root->left);
	cout << root->data << " ";
	inorderHelper(root->right);
}

// Utility function to do level order traversal
//bfs를 이용해 rbt트리를 루트 노드로 부터 리프 노드까지 
//레벨이 같다면 왼쪽 자식에서 오른쪽 자식 방향으로 출력합니다.
void levelOrderHelper(Node *root)
{
	if (root == NULL)
		return;

	std::queue<Node *> q;
	q.push(root);

	while (!q.empty())
	{
		Node *temp = q.front();
		cout << temp->data << " ";
		q.pop();

		if (temp->left != NULL)
			q.push(temp->left);

		if (temp->right != NULL)
			q.push(temp->right);
	}
}

```

### 이진 탐색 트리 방식의 삽입메소드와 트리의 좌회전, 우회전 메소드입니다. 

```cpp
/* A utility function to insert
	a new node with given key
in BST */
//기존 이진 탐색의 규칙에 따라 삽입을 하는 코드입니다.
Node* BSTInsert(Node* root, Node *pt)
{
	/* If the tree is empty, return a new node */
	if (root == NULL)
	return pt;

	/* Otherwise, recur down the tree */
	if (pt->data < root->data)
	{
		root->left = BSTInsert(root->left, pt);
		root->left->parent = root;
	}
	else if (pt->data > root->data)
	{
		root->right = BSTInsert(root->right, pt);
		root->right->parent = root;
	}

	/* return the (unchanged) node pointer */
	return root;
}


//트리에서 지정 노드 기준으로 좌회전 시킵니다.
void RBTree::rotateLeft(Node *&root, Node *&pt)
{
	Node *pt_right = pt->right;

	pt->right = pt_right->left;

	if (pt->right != NULL)
		pt->right->parent = pt;

	pt_right->parent = pt->parent;

	if (pt->parent == NULL)
		root = pt_right;

	else if (pt == pt->parent->left)
		pt->parent->left = pt_right;

	else
		pt->parent->right = pt_right;

	pt_right->left = pt;
	pt->parent = pt_right;
}

//트리에서 지정노드 기준으로 우회전 시킵니다,
void RBTree::rotateRight(Node *&root, Node *&pt)
{
	Node *pt_left = pt->left;

	pt->left = pt_left->right;

	if (pt->left != NULL)
		pt->left->parent = pt;

	pt_left->parent = pt->parent;

	if (pt->parent == NULL)
		root = pt_left;

	else if (pt == pt->parent->left)
		pt->parent->left = pt_left;

	else
		pt->parent->right = pt_left;

	pt_left->right = pt;
	pt->parent = pt_left;
}
```
### double red 발생 시 red-black 트리의 모든 조건을 만족하도록 고쳐주는 메소드입니다. 

```cpp
// This function fixes violations
// caused by BST insertion
//노드를 삽입했을 때 rbt에 미치는 영향을 고쳐줍니다.(to fix double red)
void RBTree::fixViolation(Node *&root, Node *&pt)
{
	Node *parent_pt = NULL;
	Node *grand_parent_pt = NULL;

	while ((pt != root) && (pt->color != BLACK) &&
		(pt->parent->color == RED))
	{

		parent_pt = pt->parent;
		grand_parent_pt = pt->parent->parent;

		/* Case : A
			Parent of pt is left child
			of Grand-parent of pt */

        //    P가 G의 왼쪽 자식일 경우처리해야할 과정입니다.
		if (parent_pt == grand_parent_pt->left)
		{

			Node *uncle_pt = grand_parent_pt->right;

			/* Case : 1
			The uncle of pt is also red
			Only Recoloring required */
            //N의 삼촌노드 U가 빨간색일 경우에는 recoloring 작업을 해줍니다.
			if (uncle_pt != NULL && uncle_pt->color ==
												RED)
			{
				grand_parent_pt->color = RED;
				parent_pt->color = BLACK;
				uncle_pt->color = BLACK;
				pt = grand_parent_pt;
			}

            //N의 삼촌노드 U 가 검은색인 경우에 처리해야할 과정입니다,
			else
			{
				/* Case : 2
				pt is right child of its parent
				Left-rotation required */
                //새로운 노드 N이 부모 P의 오른쪽 자식일 경우에 좌회전 해줍니다.
                //(case 4에 대해서 case 5의 형태로 바꿔줍니다.)  
				if (pt == parent_pt->right)
				{
					rotateLeft(root, parent_pt);
					pt = parent_pt;
					parent_pt = pt->parent;
				}

				/* Case : 3
				pt is left child of its parent
				Right-rotation required */
                //새로운 노드N이 P의 왼쪽 자식이라면 우회전을 시켜주고, P와 G의 색깔을 바꿔줍니다.
				rotateRight(root, grand_parent_pt);
				swap(parent_pt->color,
						grand_parent_pt->color);
				pt = parent_pt;
			}
		}

		/* Case : B
		Parent of pt is right child
		of Grand-parent of pt */
        //P가 G의 오른쪽 자식일 경우
		else
		{
			Node *uncle_pt = grand_parent_pt->left;

			/* Case : 1
				The uncle of pt is also red
				Only Recoloring required */
			if ((uncle_pt != NULL) && (uncle_pt->color ==
													RED))
			{
				grand_parent_pt->color = RED;
				parent_pt->color = BLACK;
				uncle_pt->color = BLACK;
				pt = grand_parent_pt;
			}
			else
			{
				/* Case : 2
				pt is left child of its parent
				Right-rotation required */
				if (pt == parent_pt->left)
				{
					rotateRight(root, parent_pt);
					pt = parent_pt;
					parent_pt = pt->parent;
				}

				/* Case : 3
				pt is right child of its parent
				Left-rotation required */
				rotateLeft(root, grand_parent_pt);
				swap(parent_pt->color,
						grand_parent_pt->color);
				pt = parent_pt;
			}
		}
	}

	root->color = BLACK;
}

```
### red-black트리의 삽입 메소드입니다. 새 노드를 만들어 삽입하고, 고쳐주는 작업까지 합니다.

```cpp
//rbt트리에 삽입을 하고 규칙을 위반하지 않도록 고쳐줍니다.
// Function to insert a new node with given data
void RBTree::insert(const int &data)
{
	Node *pt = new Node(data);

	// Do a normal BST insert
	root = BSTInsert(root, pt);

	// fix Red Black Tree violations
	fixViolation(root, pt);
}

// Function to do inorder and level order traversals
void RBTree::inorder()	 { inorderHelper(root);}
void RBTree::levelOrder() { levelOrderHelper(root); }

```
### main입니다. rbtree를 만들어 원소를 삽입하고 두 방식으로 출력해 봅니다.
```cpp
// Driver Code
int main()
{
	RBTree tree;

	tree.insert(7);
	tree.insert(6);
	tree.insert(5);
	tree.insert(4);
	tree.insert(3);
	tree.insert(2);
	tree.insert(1);

	cout << "Inorder Traversal of Created Tree\n";
	tree.inorder();

	cout << "\n\nLevel Order Traversal of Created Tree\n";
	tree.levelOrder();

	return 0;
}


```

