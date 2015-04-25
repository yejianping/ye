---
layout: post
published: true
title: Code Sample
mathjax: false
featured: true
comments: true
headline: Making blogging easier for masses
categories: 
  - webdevelopment
tags: ACM/ICPC
---
[POJ 3216](http://poj.org/problem?id=3216)

<center><h1><font color="blue">Repairing Company</font></h1></center>
<center><strong>Time Limit</strong>: 1000MS		&nbsp;&nbsp;&nbsp;&nbsp; <strong>Memory Limit</strong>: 131072K</center>

<center><strong>Total Submissions</strong>: 6526		&nbsp;&nbsp;&nbsp;&nbsp; <strong>Accepted</strong>: 1758</center>

<h2><font color="blue">Description</font></h2>

Lily runs a repairing company that services the Q blocks in the city. One day the company receives M repair tasks, the ith of which occurs in block pi, has a deadline ti on any repairman’s arrival, which is also its starting time, and takes a single repairman di time to finish. Repairmen work alone on all tasks and must finish one task before moving on to another. With a map of the city in hand, Lily want to know the minimum number of repairmen that have to be assign to this day’s tasks.

<h2><font color="blue">Input</font></h2>

The input contains multiple test cases. Each test case begins with a line containing Q and M (0 < Q ≤ 20, 0 < M ≤ 200). Then follow Q lines each with Q integers, which represent a Q × Q matrix Δ = {δij}, where δij means a bidirectional road connects the ith and the jth blocks and requires δij time to go from one end to another. If δij = −1, such a road does not exist. The matrix is symmetric and all its diagonal elements are zeroes. Right below the matrix are M lines describing the repairing tasks. The ith of these lines contains pi, ti and di. Two zeroes on a separate line come after the last test case.

<h2><font color="blue">Output</font></h2>

For each test case output one line containing the minimum number of repairmen that have to be assigned.

<font color="blue">###Sample Input</font>

1 2
0
1 1 10
1 5 10
0 0

<font color="blue">###Sample Output</font>

2