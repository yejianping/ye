---
layout: post
published: true
title: POJ 2282
imagefeature: 01.jpg
mathjax: false
featured: true
comments: true
headline: Making blogging easier for masses
categories: ACM/ICPC
tags: [ACM/ICPC, POJ]
---
Solve the problem: [POJ 2282](http://poj.org/problem?id=2282)

The basic idea:

First, we count the digits from 0 to a and the digits from 0 to b. Then, we do subtraction and get the result.

So, the key problem is to count digits from 0 to n.

Take 3456 as as example. And I want to count 1s from 0 to 3456.

My solution is:
Consider from the most significant digit to the least significant.

3:

- 0xxx
	- how many 1s in this case? (we need to calcualte how many 1s in no more than 3 digits number. Named as dp[3][1]. They are: 1,10,11,12...19,21,31...91,100, 101,102,...)
- 1xxx
	- how many 1s? dp[3][1]? Right? The answer is no. Because the 1 in the most significant emerges 10 * 10 * 10 times. We need to add this.
- 2xxx
	- how many 1s? (dp[3][1])

4:

- 30xx   how many 1s in this case? (we need to calcualte how many 1s in no more than 2 digits number. Named as dp[2][1]. They are: 1,10,11,12...19,21,31...91.)
- 31xx   how many 1s? dp[2][1]? Right? Of couse the answer is no. Because the 1 in the second-most significant emerges 10 * 10 times. We need to add this.
- 32xx   how many 1s? (dp[2][1])
- 33xx   how many 1s? (dp[2][1])

5:

- 340x   how many 1s in this case? (we need to calcualte how many 1s in 1-digit number. Named as dp[1][1]. Obviously, there is only one: 1)
- 341x   how many 1s? dp[1][1]? The answer is no. Because the 1 in the third-most significant emerges 10 times. We need to add this.
- 342x   how many 1s? (dp[1][1])
- 343x   how many 1s? (dp[1][1])
- 344x   how many 1s? (dp[1][1])

6:

- 3450   how many 1s in this case? 0.
- 3451   how many 1s in this case? 1.
- 3452   how many 1s in this case? 0.
- 3453   how many 1s in this case? 0.
- 3454   how many 1s in this case? 0.
- 3455   how many 1s in this case? 0.

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
#include<stdlib.h>
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

const int LEN = 12;
const int DIGITS = 10;

long long dp[LEN][DIGITS];

/*
Calculate how many digits(0-9) from 0 to p, inclusive.

Parameters:
	count[i]: record the number of corresponding digits.
	p: number converted to char array.
*/
void digitsCount(long long * count, char * p)
{
	//Initialization
	loop0(i,DIGITS)
	{
		count[i] = 0;
	}

	int len = strlen(p);
		
	//dgA[0] = 1;
	/*
	calculate how many 0s from 0 to p.
	Counting 0 is special and needs to be carefully taken care of.
	*/
	
	for(int j=0;j<len;j++)
	{
		/*
		For example: p is 2345. I consider from the most to the least significant digit.
		The most significant digit needs special treatment.

		2: 0, 1 are less than 2. Considering 0, we have dp[len-1][0] numbers. 
		Considering 1, be careful, we have dp[len-1][1](dp[len-1][1],dp[len-1][2],
		...,dp[len-1][9] are the same.) numbers. But not dp[len-1][0] numbers! Because in
		dp[len-1][0] case, 0xx is not legal. However in dp[len-1][1] case, 0xx is legal!

		3: 0,1,2 are less than 3. Considering 0, we have dp[len - j - 1][1] numbers, 
		not dp[len - j - 1][0] numbers. Right? Considering 1 or 2, we also have
		dp[len - j - 1][1] numbers. Then we also consider more on 0. You see, the current
		state is 20xx, so the 0 in the next significant position emerges 10*10 times.

		4,5: the same analysis as the above.
		*/
		if(j == 0)
		{
			count[0] += dp[len-1][0];
			count[0] += (p[j] - '0' - 1)* dp[len- 1][1];
			continue;
		}

		if((p[j]-'0') == 0) count[0] += (atol(p+j+1) + 1);
		else
		{
			//count[0] += dp[len - j - 1][0];

			count[0] += (p[j] - '0')* dp[len - j - 1][1];

			int power10 = 1;

			loop0(k,len - j - 1) power10 *= 10;
			count[0] += power10;
		}
		/*
		count[0] += (p[j] - '0')* dp[len - j - 1][0];

		int power10 = 1;

		loop0(k,len - j - 1) power10 *= 10;
		if((p[j]-'0') > 0) count[0] += power10;
		if((p[j]-'0') == 0) count[0] += (atol(p+j+1) + 1);
		*/
	}


	
	/*
	calculate how many 1-9s from 0 to p
	*/
	for(int i=1;i<DIGITS;i++)
	{
		/*
		For example: p is 3245. I consider from the most to the least significant digit.
		For clarification, here I demonstrate how to calculate 2s.

		3: 0, 1, 2 are less than 3. Considering 0, we have dp[len - j - 1][2] numbers. 
		Considering 1 and 2, we have dp[len - j - 1][2] numbers, too. However, we need to
		consider more for 2. In such a case, the number is like 2xxx, so the 2 in the most
		significant position emerges 10*10*10 times!

		2: 0,1 are less than 2. Considering 0, we have dp[len - j - 1][2] numbers. 
		Considering 1, we have dp[len - j - 1][2] numbers, too. 
		However, we also need to consider 2
		here. 32xx, so the 2 in the next significant position emerges 45+1 times.

		4,5: the same analysis as the above.
		*/
		for(int j=0;j<len;j++)
		{
			count[i] += (p[j] - '0')* dp[len - j - 1][i];

			int power10 = 1;

			loop0(k,len - j - 1) power10 *= 10;
			if((p[j]-'0') > i) count[i] += power10;
			if((p[j]-'0') == i) count[i] += (atol(p+j+1) + 1);
		}


	}

	
	//loop0(i,len) count[p[i] - '0']++;
}
int main()
{
	#ifndef ONLINE_JUDGE
	
		/*
		This part deals with the input and output file suffix. 
		By default, the input file is ye.in and output is ye.out.

		Array num is the last few characters of the file.
		ye.in1, ye.in2, ye.in3 ...
		ye.out1, ye.out2, ye.out3 ...
		*/
		char num[3] = "";

		char in[20] = "test data/ye.in";
		char out[20] = "test data/ye.out";

		for(int i=0;i<strlen(num);i++) in[strlen(in) + i] = num[i];
		for(int i=0;i<strlen(num);i++) out[strlen(out) + i] = num[i];

		freopen(in,"r",stdin);
		freopen(out,"w",stdout);
	#endif

	// TO DO
	/*
	dp[i][j]: For no greater than i-digits number, 
	how many j(0,1,2,3,4,5,6,7,8,9) are there from 0 ? 

	For example:
		dp[4][1]: for x, xx, xxx, xxxx numbers, how many 1s are there in them?
		dp[4][0]: for x, xx, xxx, xxxx numbers, how many 0s are there in them?
	*/
	

	loop0(i,LEN)
		loop0(j,DIGITS)
			dp[i][j] = 0;

	loop0(i,DIGITS) dp[1][i] = 1;

	for(int i=2;i<LEN;i++)
	{
		/*
		calculate dp[i][0] because 0 is very special.
		For example: a 4-digit number, xxxx.
		When the most significant is 0, there are dp[i-1][0] 0s.
		When the most significant is 1, be careful, there are dp[i-1][1]
		(dp[i-1][1],...,dp[i-1][9] are the same.) 0s, not dp[i-1][0] 0s.

		*/
		dp[i][0] = 9 * dp[i-1][1] + dp[i-1][0];

		/*
		calculate dp[i][1-9]. Here I will explain how to calculate dp[i][1].
		For example: a 4-digit number, xxxx.

		When the most significant is 0, there are dp[i-1][1] 0s.
		When the most significant is 1, there are dp[i-1][1] 0s.
		...
		When the most significant is 9, there are dp[i-1][1] 0s.
		However, we need to consider more when the most significant is 1. 
		Because in such a case, 1xxx, the 1 in the most significant position
		emerges 10 * 10 * 10 times.


		In the same way, we can deal with the other digits. 
		*/
		for(int j=1;j<DIGITS;j++)
		{
			long long power10 = 1;

			loop0(k,i-1) power10 *= 10;

			dp[i][j] = 10 * dp[i-1][j] + power10;
		}
	}

	//dp[0][0] = 1;
	char a[LEN];
	char b[LEN];

	long long dgA[DIGITS];
	long long dgB[DIGITS];

	int len;

	int aa,bb;
	scanf("%d%d",&aa,&bb);

	while(aa != 0 && bb != 0)
	{
		if(aa > bb)
		{
			int tmp = aa;
			aa = bb;
			bb = tmp;
		}

		//_itoa(aa,a,10);
		//_itoa(bb,b,10);
		sprintf(a,"%d",aa);
		sprintf(b,"%d",bb);
		/*
		Calculate how many digits(0-9) from 0 to a, inclusive.
		*/
		digitsCount(dgA,a);
		/*
		Calculate how many digits(0-9) from 0 to b, inclusive.
		*/
		digitsCount(dgB,b);

		len = strlen(a);
		loop0(i,len) dgA[a[i] - '0']--;

		loop0(i,DIGITS) printf("%lld ",dgB[i] - dgA[i]);
		printf("\n");


		scanf("%d%d",&aa,&bb);
	}




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