---
layout: post
published: true
title: Code Sample
imagefeature: 01.png
mathjax: false
featured: true
comments: true
headline: Making blogging easier for masses
categories: 
  - ACM/ICPC
tags: [ACM/ICPC, POJ]
---
[POJ 3216](http://poj.org/problem?id=3216)

<center><h1><font color="blue">Repairing Company</font></h1></center>
<center><strong>Time Limit</strong>: 1000MS		&nbsp;&nbsp;&nbsp;&nbsp; <strong>Memory Limit</strong>: 131072K    &nbsp;&nbsp;&nbsp;&nbsp;   <strong>Total Submissions</strong>: 6526		&nbsp;&nbsp;&nbsp;&nbsp; <strong>Accepted</strong>: 1758</center>


<h2><font color="blue">Description</font></h2>

Lily runs a repairing company that services the Q blocks in the city. One day the company receives M repair tasks, the ith of which occurs in block pi, has a deadline ti on any repairman’s arrival, which is also its starting time, and takes a single repairman di time to finish. Repairmen work alone on all tasks and must finish one task before moving on to another. With a map of the city in hand, Lily want to know the minimum number of repairmen that have to be assign to this day’s tasks.

<h2><font color="blue">Input</font></h2>

The input contains multiple test cases. Each test case begins with a line containing Q and M (0 < Q ≤ 20, 0 < M ≤ 200). Then follow Q lines each with Q integers, which represent a Q × Q matrix Δ = {δij}, where δij means a bidirectional road connects the ith and the jth blocks and requires δij time to go from one end to another. If δij = −1, such a road does not exist. The matrix is symmetric and all its diagonal elements are zeroes. Right below the matrix are M lines describing the repairing tasks. The ith of these lines contains pi, ti and di. Two zeroes on a separate line come after the last test case.

<h2><font color="blue">Output</font></h2>

For each test case output one line containing the minimum number of repairmen that have to be assigned.

<h2><font color="blue">Sample Input</font></h2>

1 2
0
1 1 10
1 5 10
0 0

<h2><font color="blue">Sample Output</font></h2>

2

<h2><font color="blue">Solution</font></h2>


{% highlight css %}

	/*
	Better to use this kind of comment. Do not use //.
	It will prevent errors when you paste your code to the web.
	*/
	#include <set>
	#include <map>
	#include <list>
	#include <cmath>
	#include <queue>
	#include <stack>
	#include <string>
	#include <vector>
	#include <cstdio>
	#include <cstring>
	#include <iostream>
	#include <algorithm>
	//#define ONLINE_JUDGE


	/*
	g++: INT_MAX need to be defined.
	c++: no need.
	*/
	//#define GPLUSPLUS
	#ifdef ONLINE_JUDGE

		#ifdef GPLUSPLUS
			#define INT_MAX 0x3FFFFFFF
		#endif
	#endif

	#ifndef ONLINE_JUDGE
		#include<unordered_map>
		#include<unordered_set>
		#include<Windows.h>
		#include<time.h>
	#endif

	using namespace std;

	#define debug(s) puts(s);
	#define loop0(i,n) for(int i=0;i<n;i++)
	#define loop1(i,n) for(int i=1;i<=n;i++)
	#define loop(i,a,b) for(int i=a;i<=b;i++)
	#define pb push_back
	#define RD(n) scanf("%d",&n)
	#define RD2(x,y) scanf("%d%d",&x,&y)
	#define RD3(x,y,z) scanf("%d%d%d",&x,&y,&z)
	#define RD4(x,y,z,w) scanf("%d%d%d%d",&x,&y,&z,&w)
	#define All(vec) vec.begin(),vec.end()
	#define MP make_pair
	#define PII pair<int, int>
	#define PQ priority_queue
	#define cmax(x,y) x = max(x,y)
	#define cmin(x,y) x = min(x,y)


	#ifndef ONLINE_JUDGE
		typedef struct ListNode 
		{
			  int val;
			  ListNode *next;
			  ListNode(int x) : val(x), next(NULL) {}
		 }ListNode;

		struct treeNode 
		{
			  int val;
			  treeNode *left;
			  treeNode *right;
			  treeNode(int x) : val(x), left(NULL), right(NULL) {}
		 };

		#define LH 1
		#define EH 0
		#define RH -1
		typedef struct avlNode
		{
			int val;
			int bf;
			struct avlNode * left, * right;

		}avlNode, * avlTree;

		struct Point {
			  int x;
			  int y;
			  Point() : x(0), y(0) {}
			  Point(int a, int b) : x(a), y(b) {}
		 };

		#define ENDCHILD 0
		#define ENDSYMBOL -1

		typedef treeNode * Tree;
		typedef int Elemtype;


		template <typename T> T  constructBtree(void);
		template <typename T> void inorder_core(const T& root);
		template <typename T> void preorder_core(const T& root);
		template <typename T> void inorder(const T& root);
		template <typename T> void preorder(const T& root);
		template <typename T> void freeTree(const T& root);
		template <typename T> void checkTree(const T& root);

		template <typename T> void R_Rotate(T &p);
		template <typename T> void L_Rotate(T &p);
		template <typename T> bool InsertAVL(T& avlroot, int e, bool & taller);
		template <typename T> void LeftBalance(T &avlroot);
		template <typename T> void RightBalance(T &avlroot);
	#endif

	#define Q 22
	#define M 205
	#define INFINITE 0x3f3f3f3f

	int q,m;

	int delta[Q][Q];
	int link[M];
	bool used[M];

	int graph[M][M];

	typedef struct task
	{
		int p;
		int t;
		int d;

		task(){}
		task(int x, int y, int z): p(x),t(y),d(z){}
	}task;

	task tsk[M];

	void floyd()
	{
		loop1(k,q)
			loop1(i,q)
				loop1(j,q)
				{
					delta[i][j] = min(delta[i][j], delta[i][k]+delta[k][j]);
				}
	}

	bool hungarian_dfs(int start)
	{
		loop1(i,m)
		{
			if(graph[start][i] && !used[i])
			{
				used[i] = true;

				if(link[i] == -1 || hungarian_dfs(link[i]))
				{
					link[i] = start;
					return true;
				}
			}
		}

		return false;
	}

	int main()
	{
	#ifndef ONLINE_JUDGE
		/*num is the suffix of your input files and output files*/
		char num[3] = "";

		char in[20] = "test data/ye.in";
		char out[20] = "test data/ye.out";

		for(int i=0;i<strlen(num);i++) in[strlen(in) + i] = num[i];
		for(int i=0;i<strlen(num);i++) out[strlen(out) + i] = num[i];

		freopen(in,"r",stdin);
		freopen(out,"w",stdout);
	#endif

		RD2(q,m);

		int p,t,d;

		while(q != 0 || m != 0)
		{
			loop0(i,M) link[i] = -1;

			loop0(i,m+1)
				loop0(j,m+1)
					graph[i][j] = 0;

			loop1(i,q)
				loop1(j,q)
				{
					RD(delta[i][j]);
					if(delta[i][j] == -1) delta[i][j] = INFINITE;
				}

			floyd();

			loop1(i,m)
			{
				RD3(p,t,d);

				tsk[i].p = p;
				tsk[i].t = t;
				tsk[i].d = d;
			}

			loop1(i,m)
				loop1(j,m)
				{
					if(delta[tsk[i].p][tsk[j].p] < INFINITE)
					{
						if((tsk[i].t+delta[tsk[i].p][tsk[j].p]+tsk[i].d)<=tsk[j].t)
							graph[i][j] = 1;
					}
				}

			int result = 0;

			loop1(i,m)
			{
				loop1(i,m) used[i] = false;

				if(hungarian_dfs(i)) result++;
			}

			printf("%d\n",m-result);

			RD2(q,m);
		}
		/*
		Tree root = constructBtree<Tree>();
		checkTree<Tree>(root);
		freeTree<Tree>(root);
		

		avlTree  avlroot = NULL;
		bool taller = false;
		InsertAVL<avlTree>(avlroot, 20, taller);
		InsertAVL<avlTree>(avlroot, 10, taller);
		InsertAVL<avlTree>(avlroot, 40, taller);
		InsertAVL<avlTree>(avlroot, 30, taller);
		InsertAVL<avlTree>(avlroot, 35, taller);
		checkTree<avlTree>(avlroot);
		freeTree<avlTree>(avlroot);
		*/
		// TO DO










	#ifndef ONLINE_JUDGE
		getchar();
		getchar();
	#endif

		return 0;
	}

	#ifndef ONLINE_JUDGE

		template <typename T> T  constructBtree(void)
		{
			T root,now,t;
			T maxLayer[256];
			Elemtype n;
			int count = 0, beingused = 0;
		
			now = (T)malloc(sizeof(treeNode));
			maxLayer[count++] = now;
		
			root = now;
		
			scanf("%d",&(root->val));
		
			scanf("%d", &n);
			while(n != ENDSYMBOL)
			{
					if(n != ENDCHILD)
					{
						 now = (T)malloc(sizeof(treeNode));
						 maxLayer[count++] = now;
						 now->val = n;
						 maxLayer[beingused]->left = now;
					 
					 
					}
					else
					{
						maxLayer[beingused]->left = NULL;
					
					
					}
				
					scanf("%d", &n);
					if(n != ENDCHILD)
					{
						 now = (T)malloc(sizeof(treeNode));
						 maxLayer[count++] = now;
						 now->val = n;
						 maxLayer[beingused]->right = now;
					 
					 
					}
					else
					{
						maxLayer[beingused]->right = NULL;
					
					
					}
				
					beingused++;
					scanf("%d", &n);
					  
			}

			return root;
		}

		template <typename T> void inorder_core(const T& root)
		{
			 if(!root) return;
		 
			 inorder_core(root->left);
			 printf("%d ", root->val);
			 inorder_core(root->right);
		} 

		template <typename T> void preorder_core(const T& root)
		{
			 if(!root) return;
		 
			 printf("%d ", root->val);
			 preorder_core(root->left);
			 preorder_core(root->right);
		} 

		template <typename T> void inorder(const T& root)
		{
			printf("inorder: ");
			inorder_core(root);
			printf("\n");
		}

		template <typename T> void preorder(const T& root)
		{
			printf("preorder: ");
			preorder_core(root);
			printf("\n");
		}

		template <typename T> void freeTree(const T& root)
		{
			if(root == NULL) return;

			freeTree(root->left);
			freeTree(root->right);
			free(root);//after free, root is not necessarily to be NULL. Be careful.
		}

		template <typename T> void checkTree(const T& root)
		{
			inorder(root);
			preorder(root);
		}

		template <typename T> void R_Rotate(T &p)
		{
			T lc = p->left;
			p->left = lc->right;
			lc->right = p;
			p = lc;
		}

		template <typename T> void L_Rotate(T &p)
		{
			T rc = p->right;
			p->right = rc->left;
			rc->left = p;
			p = rc;
		}

		template <typename T> void LeftBalance(T &avlroot)
		{
			T lc = avlroot->left;

			switch(lc->bf)
			{
				case LH:
					avlroot->bf = lc->bf = EH;
					R_Rotate(avlroot); 
					break;
				case RH:
					T rd = lc->right;
					switch(rd->bf)
					{
						case LH: avlroot->bf = RH; lc->bf = EH; break;
						case EH: avlroot->bf = lc->bf = EH; break;
						case RH: avlroot->bf = EH; lc->bf = LH; break;
					}
					rd->bf = EH;
					L_Rotate(avlroot->left);
					R_Rotate(avlroot);
			}
		}

		template <typename T> void RightBalance(T &avlroot)
		{
			T rc = avlroot->right;

			switch(rc->bf)
			{
				case RH:
					avlroot->bf = rc->bf = EH;
					R_Rotate(avlroot); 
					break;
				case LH:
					T ld = rc->left;
					switch(ld->bf)
					{
						case LH: avlroot->bf = RH; rc->bf = EH; break;
						case EH: avlroot->bf = rc->bf = EH; break;
						case RH: avlroot->bf = EH; rc->bf = LH; break;
					}
					ld->bf = EH;
					R_Rotate(avlroot->right);
					L_Rotate(avlroot);
			}
		}

		template <typename T> bool InsertAVL(T& avlroot, int e, bool & taller)
		{
			if(!avlroot)
			{
				avlroot = (T )malloc(sizeof(avlNode));
				avlroot->val = e;
				avlroot->left = avlroot->right = NULL;
				avlroot->bf = EH;
				taller = true;
			}
			else
			{
				if(e == avlroot->val)
				{
					taller = false;
					return false;
				}
			
				if(e < avlroot->val)
				{
					if(!InsertAVL(avlroot->left,e,taller)) return false;

					if(taller)
					{
						switch(avlroot->bf)
						{
							case LH:
								LeftBalance(avlroot);
								taller = false;
								break;
							case EH:
								avlroot->bf = LH;
								taller = true;
								break;
							case RH:
								avlroot->bf = EH;
								taller = false;
								break;
						}
					}
				}
				else
				{
					if(!InsertAVL(avlroot->right,e,taller)) return false;

					if(taller)
					{
						switch(avlroot->bf)
						{
							case LH:
								avlroot->bf = EH;
								taller = false;
								break;
							case EH:
								avlroot->bf = RH;
								taller = true;
								break;
							case RH:
								RightBalance(avlroot);
								taller = false;
								break;
						}
					}
				}
			}

			return true;
		}
	#endif

	/*
	Leave a few blank lines at the end. Some OJ demands this.
	*/
	
	
{% endhighlight %}