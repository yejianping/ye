---
layout: post
published: true
title: Code example(POJ 3208)
mathjax: false
featured: true
comments: true
headline: Jianping Ye's personal blog
categories: ACM/ICPC
tags: [ACM/ICPC, POJ]
---
[POJ 3208](http://poj.org/problem?id=3208) 

It's claimed to be the most difficult digital dynamic programming problem in POJ.

<center><h1><font color="blue">Apocalypse Someday</font></h1></center>
<center><strong>Time Limit</strong>: 1000MS		&nbsp;&nbsp;&nbsp;&nbsp; <strong>Memory Limit</strong>: 131072K    &nbsp;&nbsp;&nbsp;&nbsp;   <strong>Total Submissions</strong>: 1575		&nbsp;&nbsp;&nbsp;&nbsp; <strong>Accepted</strong>: 736</center>


<h2><font color="blue">Description</font></h2>

The number 666 is considered to be the occult "number of the beast" and is a well used number in all major apocalypse themed blockbuster movies. However the number 666 can't always be used in the script so numbers such as 1666 are used instead. Let us call the numbers containing at least three contiguous sixes beastly numbers. The first few beastly numbers are 666, 1666, 2666, 3666, 4666, 5666...

Given a 1-based index n, your program should return the nth beastly number.

<h2><font color="blue">Input</font></h2>

The first line contains the number of test cases T (T ≤ 1,000).

Each of the following T lines contains an integer n (1 ≤ n ≤ 50,000,000) as a test case.

<h2><font color="blue">Output</font></h2>

For each test case, your program should output the nth beastly number.

<h2><font color="blue">Sample Input</font></h2>

3

2

3

187

<h2><font color="blue">Sample Output</font></h2>

1666

2666

66666


<h2><font color="blue">Solution</font></h2>




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
	My OJ template definitions. Just for convenience.
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


	#define LEN 15
	#define DIGITS 10

	int main()
	{
		#ifndef ONLINE_JUDGE
			/*
			This part deals with the input and output file suffix. By default, 
			the input file is ye.in and output is ye.out.

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
		There are 4 states for the numbers:
		0: the last digit is not 6 and it's not a beastly number
		1: the last digit is 6, the penultimate is not 6 and it's not a beastly number
		2: the last 2 digits are 66, and it's not a beastly number
		3: it's a beastly number
		*/

		/*
		tranfer[i][j] means: If the current number is in state i, we append digit j to the number, 
		what's the state of the new number? 
		*/
		int transfer[4][DIGITS] =
		{
			{0,0,0,0,0,0,1,0,0,0},
			{0,0,0,0,0,0,2,0,0,0},
			{0,0,0,0,0,0,3,0,0,0},
			{3,3,3,3,3,3,3,3,3,3}
		};

		/*
		If the number has k digits, we divide the k digits into two parts: the last i digits and the former 
		(k-i) digits.
		dp[i][j]: When the number comprising of the former digits(k-i) is in state j, 
		how many beastly numbers can we create using the last i digits?
		
		For example, i == 2, and the former digits give a number, such as 126 
		(so this number is in state 1, right?), how many beastly numbers can we create using i digits? 
		The answer is 1: 12666. Because a beastly number must have "666" in it.

		The reduction formula is:

		dp[i][j] = dp[i-1][j->k], where k is the state when we append a digit to the former (k-i) digits.
		*/
		long long dp[LEN][4];

		//dp[3][0] = 800;
		//dp[3][1] = 90;
		//dp[3][2] = 9;
		//dp[3][3] = 1;
		loop0(i,LEN)
			loop0(j,4) dp[i][j] = 0;

		dp[0][0] = 0;
		dp[0][1] = 0;
		dp[0][2] = 0;
		dp[0][3] = 1;

		

		loop1(i,LEN-1)
			loop0(j,4)
				loop0(k,DIGITS)
				{
					dp[i][j] += dp[i-1][transfer[j][k]];
				}

		int len;

		int t;

		RD(t);

		int n;
		int bits;
		long long result;
		int state;

		while(t-- > 0)
		{
			RD(n);

			len = 0;// len is the length(digits) of n
			while(dp[len][0] < n) len++;/*calculate how many digits in n*/

			result = 0;
			state = 0;
			int tmp;
			/*
			From the most significant digit to the least significant, we determine them one by one.
			*/
			for(int i=len;i>0;i--)
			{
				tmp = 0;

				loop0(j,DIGITS)//list the digits from 0-9 to see which one is correct.
				{
					tmp += dp[i-1][transfer[state][j]];

					if(tmp >= n)
					{
						//The i-th digit is determined to be j.
						result = result * 10 + j;

						tmp -= dp[i-1][transfer[state][j]];

						n -= tmp;
						//Calculate the state of current number.
						state = transfer[state][j];
						break;
					}
				}
			}

			printf("%lld\n",result);
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
	