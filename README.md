# Binary_Sort_Tree-
二叉排序树的实现及其相关操作

**问题描述及要求**
		产生一个菜单驱动的演示程序，用以说明二叉树的使用。元素由单个键组成，键为单个字符。用户能演示的二叉树基本操作至少包括：构造二叉树，按先序、中序、后序、层序遍历这棵二叉树，求二叉树的深度、宽度，统计度为0，1，2的结点数等。二叉树采用链式存储结构。对二叉查找树做上述工作，且增加以下操作：插入、删除给定键的元素、查找目标键。	


由于没使用类模板，请使用二叉树演示时此处为char
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201111182959297.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2FzYmJ2,size_16,color_FFFFFF,t_70#pic_center)而使用二叉排序树时将char 改为int。

**Code**

```cpp
#include<iostream>
#include<string>
#include<map>
#include<algorithm>
#include<memory.h>
#include<cmath>
#include<queue>
#define pii pair<int,int>
#define FAST ios::sync_with_stdio(false),cin.tie(0),cout.tie(0)
using namespace std;
typedef long long ll;
typedef char datatype;
const int Max = 1e3 + 5;
int h,ht, d1, d2, d0;

struct BiNode
{
	datatype data;
	BiNode *lchild, *rchild;
};
BiNode* fa;
class BiTree
{
public:
	BiTree() { root = Create(); }
	void PreOrder() { PreOrder(root); }
	void InOrder() { InOrder(root); }
	void PostOrder() { PostOrder(root); }
	void LevelOrder();
	void IterativePreorder();
	void get_high() { h = 0;get_high(root); }
	void get_node() { d0 = d1 = d2 = 0;get_node(root); }
	void get_width();
private:
	BiNode* Create();
	void Release(BiNode* bt);
	void PreOrder(BiNode* bt);
	void InOrder(BiNode* bt);
	void PostOrder(BiNode* bt);
	void get_high(BiNode* bt);
	void get_node(BiNode* bt);
	BiNode* root;
};

void BiTree::IterativePreorder()
{
	BiNode* stack[Max];
	int top = -1;
	if (root == NULL)return;
	stack[++top] = root;
	while (top!=-1)
	{
		BiNode* p = stack[top];
		cout << p->data << endl;
		top--;
		if (p->rchild != NULL)stack[++top] = p->rchild;
		if (p->lchild != NULL)stack[++top] = p->lchild;
	}
}

void BiTree::get_node(BiNode* bt)
{
	if (bt->lchild != NULL && bt->rchild != NULL)
	{
		d2++;
		get_node(bt->lchild);
		get_node(bt->rchild);
	}
	else if (bt->lchild!=NULL)
	{
		d1++;
		get_node(bt->lchild);
	}
	else if (bt->rchild != NULL)
	{
		d1++;
		get_node(bt->rchild);
	}
	else
	{
		d0++;
		return;
	}
}

void BiTree::get_high(BiNode* bt)
{
	ht++;
	if (bt == NULL)return;
	if (bt->lchild == NULL && bt->rchild == NULL)
	{
		h = max(h, ht);
		return;
	}
	get_high(bt->lchild);
	ht--;
	get_high(bt->rchild);
	ht--;
}

void BiTree::PreOrder(BiNode* bt)
{
	if (bt == NULL)return;
	else
	{
		cout << bt->data << endl;
		PreOrder(bt->lchild);
		PreOrder(bt->rchild);
	}
}

void BiTree::InOrder(BiNode* bt)
{
	if (bt == NULL)return;
	else
	{
		InOrder(bt->lchild);
		cout << bt->data << endl;
		InOrder(bt->rchild);
	}
}

void BiTree::PostOrder(BiNode* bt)
{
	if (bt == NULL)return;
	else
	{
		PostOrder(bt->lchild);
		PostOrder(bt->rchild);
		cout << bt->data << endl;
	}
}

void BiTree::LevelOrder()
{
	BiNode* Q[Max], * q = NULL;
	int front = -1, rear = -1;
	if (root == NULL) return;
	Q[++rear] = root;
	while (front != rear)
	{
		q = Q[++front];
		cout << q->data << endl;
		if (q->lchild != NULL)Q[++rear] = q->lchild;
		if (q->rchild != NULL)Q[++rear] = q->rchild;
	}
}

BiNode* BiTree::Create()
{
	BiNode* bt;
	datatype ch;
	cin >> ch;
	if (ch == '#')bt = NULL;
	else
	{
		bt = new BiNode;
		bt->data = ch;
		bt->lchild = Create();
		bt->rchild = Create();
	}
	return bt;
}

class BiSortTree
{
public:
	BiSortTree(int a[], int n);
	//~BiSortTree() { Release(root); }
	BiNode* InsertBST(datatype x) { return InsertBST(root, x); }
	void DeleteBST(datatype x);
	BiNode* SearchBST(datatype x) { return SearchBST(root, x); }
	BiNode* find(const datatype key) { return find(root, key); }
	void PreOrder() { PreOrder(root); }
	void InOrder() { InOrder(root); }
	void PostOrder() { PostOrder(root); }
	void LevelOrder();
	void get_high() { h = 0;get_high(root); }
	void get_node() { d0 = d1 = d2 = 0;get_node(root); }
	void get_width();
private:
	BiNode* InsertBST(BiNode* bt, datatype x);
	BiNode* SearchBST(BiNode* bt, datatype x);
	BiNode* find(BiNode* root, const datatype key);
	void get_high(BiNode* bt);
	void get_node(BiNode* bt);
	void Release(BiNode* bt);
	void PreOrder(BiNode* bt);
	void InOrder(BiNode* bt);
	void PostOrder(BiNode* bt);
	BiNode* root;
};



BiNode* BiSortTree::SearchBST(BiNode* bt, datatype x)
{
	if (bt == NULL) return NULL;
	if (bt->data == x)return bt;
	else if (bt->data > x)return SearchBST(bt->lchild, x);
	else return SearchBST(bt->rchild, x);
}

BiNode* BiSortTree::InsertBST(BiNode* bt, datatype x)
{
	if (bt == NULL)
	{
		BiNode* s = new BiNode;
		s->data = x;
		s->lchild = s->rchild = NULL;
		bt = s;
		return bt;
	}
	else if (bt->data > x)
	{

		if (bt->lchild == NULL)bt->lchild = InsertBST(bt->lchild, x);
		else InsertBST(bt->lchild, x);
	}
	else
	{
		if (bt->rchild == NULL)bt->rchild = InsertBST(bt->rchild, x);
		else InsertBST(bt->rchild, x);
	}
}

BiSortTree::BiSortTree(int a[], int n)
{
	root = NULL;
	for (int i = 0;i < n;i++)
	{
		if (i == 0) root = InsertBST(root, a[i]);//在插入操作时，当root节点为空时分配根节点空间，并且root的地址不会改变。
		else InsertBST(root, a[i]);
	}
}

BiNode* BiSortTree::find(BiNode* root,const datatype key)
{
	fa = NULL;
	BiNode* cur = root;
	while (cur != NULL && cur->data != key)
	{
		fa = cur;
		if (key > cur->data)cur = cur->rchild;
		else cur = cur->lchild;
	}
	if (cur == NULL)return NULL;
	return cur;
}

void BiSortTree::DeleteBST(datatype x)
{
	BiNode* p = find(x);
	if ((p->lchild == NULL) && (p->rchild == NULL))
	{
		if (fa == NULL);
		else if (fa->lchild == p)fa->lchild = NULL;
		else if (fa->rchild == p)fa->rchild = NULL;
		delete p;return;
	}
	if (p->rchild == NULL)
	{
		if (fa == NULL)
		{
			root = p->lchild;
			delete p;return;	
		}
		else
		{
			if (fa->lchild == p)fa->lchild = p->lchild;
			else if (fa->rchild == p)fa->rchild = p->lchild;
			delete p;return;
		}
	}
	if (p->lchild == NULL)
	{
		if (fa == NULL)
		{
			root = p->rchild;
			delete p;return;
		}
		else
		{
			if (fa->lchild == p)fa->lchild = p->rchild;
			else if (fa->rchild == p)fa->rchild = p->rchild;
			delete p;return;
		}
	}
	BiNode* par = p, * s = p->lchild;
	while (s->rchild != NULL)
	{
		par = s;
		s = s->rchild;
	}
	if (par->lchild == s)par->lchild = NULL;
	if (par->rchild == s)par->rchild = NULL;
	p->data = s->data;
	if (par == p)par->lchild = s->lchild;
	else par->rchild = s->lchild;
	delete s;
}

void BiSortTree::get_node(BiNode* bt)
{
	if (bt->lchild != NULL && bt->rchild != NULL)
	{
		d2++;
		get_node(bt->lchild);
		get_node(bt->rchild);
	}
	else if (bt->lchild != NULL)
	{
		d1++;
		get_node(bt->lchild);
	}
	else if (bt->rchild != NULL)
	{
		d1++;
		get_node(bt->rchild);
	}
	else
	{
		d0++;
		return;
	}
}

void BiSortTree::get_high(BiNode* bt)
{
	ht++;
	if (bt == NULL)return;
	if (bt->lchild == NULL && bt->rchild == NULL)
	{
		h = max(h, ht);
		return;
	}
	get_high(bt->lchild);
	ht--;
	get_high(bt->rchild);
	ht--;
}

void BiSortTree::PreOrder(BiNode* bt)
{
	if (bt == NULL)return;
	else
	{
		cout << bt->data << endl;
		PreOrder(bt->lchild);
		PreOrder(bt->rchild);
	}
}

void BiSortTree::InOrder(BiNode* bt)
{
	if (bt == NULL)return;
	else
	{
		PreOrder(bt->lchild);
		cout << bt->data << endl;
		PreOrder(bt->rchild);
	}
}

void BiSortTree::PostOrder(BiNode* bt)
{
	if (bt == NULL)return;
	else
	{
		PreOrder(bt->lchild);
		PreOrder(bt->rchild);
		cout << bt->data << endl;
	}
}

void BiSortTree::LevelOrder()
{
	BiNode* Q[Max], * q = NULL;
	int front = -1, rear = -1;
	if (root == NULL) return;
	Q[++rear] = root;
	while (front != rear)
	{
		q = Q[++front];
		cout << q->data << endl;
		if (q->lchild != NULL)Q[++rear] = q->lchild;
		if (q->rchild != NULL)Q[++rear] = q->rchild;
	}
}

void project()
{
	cout << "选择演示二叉树输入：1" << endl << "选择演示二叉查找树输入：2" << endl;
	int sec;cin >> sec;

	if (sec == 1)
	{
		cout << "请先创建二叉树" << "输入根节点后依次输入左子树结点和右子树结点，如果空则输入#" << endl;
		BiTree tree;
		system("cls");
		while (1)
		{
			system("cls");
			cout << "输入：1输出前序遍历" << endl;
			cout << "输入：2输出中序遍历" << endl;
			cout << "输入：3输出后序遍历" << endl;
			cout << "输入：4输出层序遍历" << endl;
			cout << "输入：5输出度为0、1、2的结点数" << endl;
			cout << "输入：6输出树的高度" << endl;
			cout << "输入：7输出前序遍历" << endl;
			cout << "输入：8输出树的宽度" << endl;
			int sec;cin >> sec;
			switch (sec)
			{
			case 1:
				tree.PreOrder();
				break;
			case 2:
				tree.InOrder();
				break;
			case 3:		
				tree.PostOrder();//tree.IterativePreorder();非递归
				break;
			case 4:
				tree.LevelOrder();
				break;
			case 5:
				tree.InOrder();
				break;
			case 6:
				tree.get_high();cout << "树的高度为" << h << endl;
				break;
			case 7:
				tree.get_node();cout << "度为0结点数：" << d0 << "度为1结点数：" << d1 << "度为2结点数：" << d2 << endl;
				break;
			}
			cout << "退出输入：1，继续操作输入：0" << endl;
			int f;cin >> f;
			if (f)break;
		}
	}
	else if (sec == 2)
	{
		cout << "请先创建二叉排序树" << "先输入要输入结点的个数，再输入一组整数以空格隔开" << endl;
		int lst[Max];
		int n;cin >> n;for (int i = 0;i < n;i++)cin >> lst[i];
		BiSortTree tree(lst,n);
		system("cls");
		while (1)
		{
			system("cls");
			cout << "输入：1输出前序遍历" << endl;
			cout << "输入：2输出中序遍历" << endl;
			cout << "输入：3输出后序遍历" << endl;
			cout << "输入：4输出层序遍历" << endl;
			cout << "输入：5输出度为0、1、2的结点数" << endl;
			cout << "输入：6输出树的高度" << endl;
			cout << "输入：7输出前序遍历" << endl;
			cout << "输入：8输出树的宽度" << endl;
			cout << "输入：9增加一个结点" << endl;
			cout << "输入：10删除一个结点" << endl;
			cout << "输入：11查找一个结点" << endl;
			int sec;cin >> sec;cout << endl;
			switch (sec)
			{
			case 1:
				tree.PreOrder();
				break;
			case 2:
				tree.InOrder();
				break;
			case 3:
				tree.PostOrder();
				break;
			case 4:
				tree.LevelOrder();
				break;
			case 5:
				tree.get_node();cout << "度为0结点数：" << d0 << "度为1结点数：" << d1 << "度为2结点数：" << d2 << endl;
				break;
			case 6:
				tree.get_high();cout << "树的高度为" << h << endl;
				break;
			case 7:
				tree.PreOrder();
				break;
			case 9:
				cout << "请输入想要加入的结点" << endl;
				int x;cin >> x;
				tree.InsertBST(x);
				break;
			case 10:
				cout << "请输入想要删除的结点" << endl;
				int xx;cin >> xx;
				tree.DeleteBST(xx);
				break;
			case 11:
				cout << "请输入想要查找的结点" << endl;
				int xxx;cin >> xxx;int f = 0;
				if (tree.find(xxx) == NULL)cout << "无此结点" << endl;
				else cout << "存在此节点" << endl;
				break;
			}
			cout << "退出输入：1，继续操作输入：0" << endl;
			int f;cin >> f;
			if (f)break;
		}
	}
}

int main()
{
	project();
	return 0;
}
```

