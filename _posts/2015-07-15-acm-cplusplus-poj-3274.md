---
layout: post
published: true
title: POJ 3274
imagefeature: 01.jpg
mathjax: false
featured: true
comments: true
headline: Making blogging easier for masses
categories: ACM/ICPC
tags: [ACM/ICPC, POJ]
---
>>> Solve the problem: [POJ 3274](http://poj.org/problem?id=3274)

Considering the sample: 7 6 7 2 1 4 2

leading zeros       000       000

7--->111----------->111------>111
6--->110----------->221------>110(*)
7--->111----------->332------>110
2--->010----------->342------>120
1--->001----------->343------>010
4--->100----------->443------>110(*)
2--->010----------->453------>120

Intuitively, I begin by calculating the accumulating number of features from the beginning to the end. Then, for each result, I substract any feature from other features and get the last column.
At last, I will find all the rows with the same figure and the distance of the farthest pair of such figures(Starred) is the result.

Why? What is behind my solution?

Let f[i][j] denote the number of feature j from the beginning to i, included. If the result range is between [x+1, y], then we must have 

f[y][1] -  f[x][1] = f[y][2] -  f[x][2] = f[y][3] -  f[x][3] = ... = f[y][k] -  f[x][k]

Thus, 

f[y][1] -  f[y][k] = f[x][1] -  f[x][k]

f[y][2] -  f[y][k] = f[x][2] -  f[x][k]

...

f[y][k] -  f[y][k] = f[x][k] -  f[x][k]

In the above example, we substract the last feature(from left to right) from all the other features.

The next problem is how to find the same pair of numbers. Of course we can't use brute-force method. It's time complexity is O(n*n). 
We may use hash table. Of course, we can use the STL in C++. But you need to find a way to the denote the numbers in the last column. Notice that, there are maybe negative numbers.
Here, I implement a very simple HASH table. It's much more efficient, of course, compared to the STL hash table.

<h2><font color="blue">Solution</font></h2>

{% highlight css %}

/*
	Better to use this kind of comment. Do not use //.
	It will prevent errors when you paste your code to the web.
*/
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
Ignore this comment.
g++: INT_MAX need to be defined.
c++: no need.

The end: give up INT_MAX. Define INF is good and enough.
*/


#ifndef ONLINE_JUDGE
	#include<unordered_map>
	#include<unordered_set>
	#include<Windows.h>
	#include<time.h>
#endif

using namespace std;

/*
My OJ template definitions.Just for convenience.
*/
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

/*
VS compiler does not support array clarification like dp[length][length].
So I need to do this dynamically.
*/
template<typename TT>

TT ** alloc(int row, int col)
{
	TT ** p;

	p = new TT*[row];

	loop0(i,row) p[i] = new TT[col];

	return p;
}

const int N = 100000;
const int K = 32;
const unsigned long hashsize = 100003;

int entry[hashsize], Next[hashsize];
int bit[N+1][K];
int n,k;

/*BKDR hash*/
unsigned long HASH(int index)
{
	int hash = 0;

	loop0(i,k)
	{
		hash = hash*131+bit[index][i];
	}

	return (hash & 0x7fffffff);
}

/*compare two rows in the last column to see whether they are exactly the same.*/
bool same(int x,int y)
{
	loop0(i,k)
	{
		if(bit[x][i] != bit[y][i]) return false;
	}

	return true;
}



int main()
{
	#ifndef ONLINE_JUDGE
	
		/*
		This part deals with the input and output file suffix. By default, the input file is ye.in and output is ye.out.

		Array num is the last few characters of the file.
		ye.in1, ye.in2, ye.in3 ...
		ye.out1, ye.out2, ye.out3 ...
		*/
		char num[3] = "1";

		char in[20] = "test data/ye.in";
		char out[20] = "test data/ye.out";

		for(int i=0;i<strlen(num);i++) in[strlen(in) + i] = num[i];
		for(int i=0;i<strlen(num);i++) out[strlen(out) + i] = num[i];

		freopen(in,"r",stdin);
		freopen(out,"w",stdout);
	#endif

	// TO DO
	

	

	RD2(n,k);

	int cow;

	loop0(j,k) bit[0][j] = 0;
	/*
	Get the second column.
	*/
	loop1(i,n)
	{
		RD(cow);

		loop0(j,k)
		{
			

			if(cow & 1)
			{
				bit[i][j] = 1;
			}
			else
			{
				bit[i][j] = 0;
			}

			cow >>= 1;
		}
	}

	/*
	Get the third column.
	*/
	for(int i=1;i<=n;i++)
		loop0(j,k)
		{
			bit[i][j] += bit[i-1][j];
		}
	
	/*
	Get the last column.
	*/
	for(int i=0;i<=n;i++)
		loop0(j,k)
			bit[i][j] -= bit[i][k-1];

	loop0(i,hashsize)
	{
		entry[i] = -1;
		Next[i] = -1;
	}
	
	int result = 0;
	int hash;
	unsigned long key;
	for(int i=0;i<=n;i++)
	{
		/*
		Standard hash routine.
		*/
		hash = HASH(i);
		key = hash%hashsize;

		int e = entry[key];
		int prev = -1;
		while(e != -1)
		{
			if(same(i,e))
			{
			/*
			Once we find the first same figure, it's enough to terminate this search because it's already the farthest figure.
			*/	
				result = max(result,i-e);
				break;
			}
			prev = e;
			e = Next[e];
		}

		if(entry[key] == -1)
		{
			entry[key] = i;
			
		}
		else
		{
			Next[prev] = i;
			//Next[i] = -1;
		}

	}


	printf("%d\n",result);





#ifndef ONLINE_JUDGE
	getchar();
	getchar();
#endif

	return 0;
}


/*
	Leave a few blank lines at the end. Some OJ demands this.
*/




	
{% endhighlight %}