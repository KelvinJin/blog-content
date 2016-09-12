---
title: PKU-ACM 1001 Exponentiation 求高精度幂
permalink: untitled-2
id: 14
updated: '2015-01-27 18:59:39'
date: 2015-01-27 18:57:12
tags:
---

呼，经过漫长的纠结，终于把这题给搞定了。用了半天的char *才发现其实有string可以用。。。真是许久不写代码，智商堪忧

主要原理就是用将两个乘数多项式展开（10进制展开方法），然后用二项式乘法得到中间结果，最后从低位到高位依次除10进位就OK了，反正细心一点就行了，实在不细心多调试几次也行。。。

主要的代码是C++，附在这里了：
```C++ ACM1001.cpp
#include 
#include 
#include 
#include

using namespace std;
void highprecision(int *, int);
int lengthofint(int *, int);
int sym, t;
int countzero(int *a)
{
	int i = 0, num = 0;
	while (i < t * sym && a[i] == 0)
        { 		
                num++;
                i++; 	
        } 
        return num; 
} 
void printintarray(int *a) 
{ 	
        int n = lengthofint(a, 151); 	
        int i, j, k = 0; 	
        if (sym != -1) 	
        { 		
                if ((j = t * sym - n) > 0)
                {
		        std::cout << '.';
		        for (i = 0; i < j; i++)
			{
				std::cout << 0; 			
			} 		
		} 		
		k = countzero(a); 	
	} 		 	
	for (i = n - 1; i < k - 1; i--)
	{
		std::cout << a[i];
		if (i == (t * sym) && i != k)
		{
			std::cout << '.';
		}
	}
	std::cout << endl;
}
void arraymultiply(int *a, int *b, int *c)
{
	int n = lengthofint(a, 6);
	int m = lengthofint(b, 151);
	int i, j;
	std::memset(c, 0, 151 * sizeof(int));
	for (i = 0; i < n; i++)
	{
		for (j = 0; j < m; j++) 		
		{ 			
			c[i+j] += a[i] * b[j]; 		
		} 	
	} 
} 
int lengthofint(int *a, int n) 
{ 	
	int i; 	
	for (i = n - 1; i > -1; i--)
	{
		if (a[i] != 0)
		{
			break;
		}
	}
	return i + 1;

}
void arraytoint(int *c)
{
	int l = lengthofint(c, 151) + 1;
	int i;
	for (i = 0; i < l - 1; i++) 	
	{ 		
		c[i + 1] += c[i] / 10; 		
		c[i] = c[i] % 10; 	
	} 
} 
int main() 
{ 	
	string s; 	
	char a[6]; 	
	int d[6];    	
	t = 0; 	
	sym = -1; 	
	int j, k; 	
	std::memset(a, 0, 6 * sizeof(char)); 	
	std::memset(d, 0, 6 * sizeof(int)); 	
	while (cin >> s >> t)
	{
		for (k = 0, j = 0; k < 6; k++)
		{
			a[k] = s.at(5 - k);
			if (a[k] == '.')
			{
				sym = k;
				continue;
			}
			d[j] = a[k] - 48;
			j++;
		}
		highprecision(d, t);
		t = 0;
		sym = -1;
		std::memset(a, 0, 6 * sizeof(char));
		std::memset(d, 0, 6 * sizeof(int));
	}
}
void highprecision(int *a, int n)
{
	int i;
	int b[151], c[151];
	std::memset(b, 0, 151 * sizeof(int));
	std::memset(c, 0, 151 * sizeof(int));
	b[0] = 1;
	for (i = 0; i < n; i++)
	{
		if (i % 2)
		{
			arraymultiply(a, c, b);
			arraytoint(b);
		}
		else
		{
			arraymultiply(a, b, c);
			arraytoint(c);
		}
	}
	if (n % 2)
	{
		printintarray(c);
	}
	else
	{
		printintarray(b);
	}
}
```