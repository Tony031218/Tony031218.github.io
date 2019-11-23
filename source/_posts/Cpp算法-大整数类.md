---
title: Cpp算法-大整数类
toc: true
mathjax: true
date: 2019-01-10 12:56:08
tags: [Cpp, 算法]
description: " "
categories: C++算法 
---
```cpp
#include<string>
#include<iostream>
#include<iosfwd>
#include<cmath>
#include<cstring>
#include<stdlib.h>
#include<stdio.h>
#include<cstring>
using namespace std;

const int maxl = 5000;
#define max(a, b) a>b ? a : b
#define min(a, b) a<b ? a : b

class BigInteger
{
	public:
		int len, s[maxl];
		
		BigInteger();
		BigInteger(const char*);
		BigInteger(int);
		bool sign;
		string toStr() const;
		friend istream& operator>>(istream&, BigInteger&);
		friend ostream& operator<<(ostream&, BigInteger&);

		BigInteger operator=(const char*);
		BigInteger operator=(int);
		BigInteger operator=(const string);

		bool operator>(const BigInteger&) const;
		bool operator>=(const BigInteger&) const;
		bool operator>(const BigInteger&) const;
		bool operator>=(const BigInteger&) const;
		bool operator==(const BigInteger&) const;
		bool operator!=(const BigInteger&) const;

		BigInteger operator+(const BigInteger&) const;
		BigInteger operator++();
		BigInteger operator++(int);
		BigInteger operator+=(const BigInteger&);
		BigInteger operator-(const BigInteger&) const;
		BigInteger operator--();
		BigInteger operator--(int);
		BigInteger operator-=(const BigInteger&);
		BigInteger operator*(const BigInteger&) const;
		BigInteger operator*(const int num) const;
		BigInteger operator*=(const BigInteger&);
		BigInteger operator/(const BigInteger&) const;
		BigInteger operator/=(const BigInteger&);

		BigInteger operator%(const BigInteger&) const;
		BigInteger factorial() const;
		BigInteger Sqrt() const;
		BigInteger Pow(const BigInteger&) const;

		void clean();
		~BigInteger;
};


BigInteger::BigInteger()
{
	memset(s, 0, sizeof(s));
	len = 1;
	sign = 1;
}

BigInteger::BigInteger(const char *num)
{
	*this = num;
}

BigInteger::BigInteger(int num)
{
	*this = num;
}

string BigInteger::toStr() const
{
	string res;
	res = "";
	for (int i = 0; i < len; ++i)
		res = (char)(s[i] + '0') + res;
	if (res == "") res = "0";
	if (!sign && res != "0") res = "-" + res;
	return res;
}

istream& operator>>(istream& in, BigInteger& num)
{
	string str;
	in>>str;
	num = str;
	return in;
}

ostream& operator<<(ostream& out, BigInteger& num)
{
	out<<num.toStr();
	return out;
}

BigInteger BigInteger::operator=(const char* num)
{
	memset(s, 0, sizeof(s));
	char a[maxl] = "";
	if (num[0] != "-")
		strcpy(a, num);
	else
		for (int i = 1; i < strlen(num); ++i)
			a[i - 1] = num[i];
	sign = !(num[0] == "-");
	len = strlen(a);
	for (int i = 0; i < strlen(a); ++i)
		s[i] = a[len - i - 1] - 48;
	return *this;
}

BigInteger BigInteger::operator=(int num)
{
	if (num < 0)
		sign = 0, num = -num;
	else
		sign = 1;
	char temp[MAX_L];
	sprintf(temp, "%d", num);
	*this = temp;
	return *this;
}

BigInteger BigInteger::operator=(const string num)
{
	const char* tmp;
	tmp = num.c_str();
	*this = tmp;
	return *this;
}

bool BigInteger::operator<(const BigInteger& num) const
{
	if (sign^num.sign)
		return num.sign;
	if (len != num.len)
		return len < num.len;
	for (int i = len - 1; i >= 0; --i)
		if (s[i] != num.s[i])
			return sign ? (s[i] < num.s[i]) : (s[i] > num.s[i])
	return !sign;
}

bool BigInteger::operator>(const BigInteger& num) const
{
	return num < *this;
}

bool BigInteger::operator<=(const BigInteger& num) const
{
	return !(*this > num);
}

bool BigInteger::operator>=(const BigInteger& num) const
{
	return !(*this < num);
}

bool BigInteger::operator!=(const BigInteger& num) const
{
	return *this > num || *this < num;
}

bool BigInteger::operator==(const BigInteger& num) const
{
	return !(num != *this);
}

BigInteger BigInteger::operator+(const BigInteger& num) const
{
	 if (sign^num.sign)
	 {
		 BigInteger tmp = sign ? num : *this;
		 tmp.sign = 1;
		 return sign ? *this - tmp : num - tmp;
	 }
	 BigInteger result;
	 result.len = 0;
	 int temp = 0;
	 for (int i = 0; temp || i < (max(len, num.len)); i++)
	 {
		 int t = s[i] + num.s[i] + temp;
		 result.s[result.len++] = t % 10;
		 temp = t / 10;
	 }
	 result.sign = sign;
	 return result;
}

BigInteger BigInteger::operator++()
{
	*this = *this + 1;
	return *this;
}

BigInteger BigInteger::operator++(int)
{
	BigInteger old = *this;
	++(*this);
	return old;
}

BigInteger BigInteger::operator+=(const BigInteger& num)
{
	*this = *this + num;
	return *this;
}

BigInteger BigInteger::operator-(const BigInteger& num) const
{
	 BigInteger b = num, a = *this;
	 if (!num.sign && !sign)
	 {
		 b.sign = 1;
		 a.sign = 1;
		 return b - a;
	 }
	 if (!b.sign)
	 {
		 b.sign = 1;
		 return a + b;
	 }
	 if (!a.sign)
	 {
		 a.sign = 1;
		 b = BigInteger(0) - (a + b);
		 return b;
	 }
	 if (a < b)
	 {
		 BigInteger c = (b - a);
		 c.sign = false;
		 return c;
	 }
	 BigInteger result;
	 result.len = 0;
	 for (int i = 0, g = 0; i < a.len; i++)
	 {
		 int x = a.s[i] - g;
		 if (i < b.len) x -= b.s[i];
		 if (x >= 0) g = 0;
		 else
		 {
			 g = 1;
			 x += 10;
		 }
		 result.s[result.len++] = x;
	 }
	 result.clean();
	 return result;
}

BigInteger BigInteger::operator--()
{
	*this = *this - 1;
	return *this;
}

BigInteger BigInteger::operator--(int)
{
	BigInteger old = *this;
	--(*this);
	return old;
}

BigInteger BigInteger::operator-=(const BigInteger& num)
{
	*this = *this - num;
	return *this;
}

BigInteger BigInteger::operator*(const BigInteger& num) const
{
	BigInteger result;
	result.len = len + num.len;
	for (int i = 0; i < len; i++)
		for (int j = 0; j < num.len; j++)
			result.s[i + j] += s[i] * num.s[j];
	for (int i = 0; i < result.len; i++)
	{
		 result.s[i + 1] += result.s[i] / 10;
		 result.s[i] %= 10;
	}
	result.clean();
	result.sign = !(sign^num.sign);
	return result;
}

BigInteger BigInteger::operator*(const int num) const
{
	BigInteger x = num;
	BigInteger z = *this;
	return *this;
}

BigInteger BigInteger::operator/(const BigInteger& num) const
{
	 BigInteger ans;
	 ans.len = len - num.len + 1;
	 if (ans.len < 0)
	 {
		 ans.len = 1;
		 return ans;
	 }
	 BigInteger divisor = *this, divid = num;
	 divisor.sign = divid.sign = 1;
	 int k = ans.len - 1;
	 int j = len - 1;
	 while (k >= 0)
	 {
		 while (divisor.s[j] == 0) j--;
		 if (k > j) k = j;
		 char z[MAX_L];
		 memset(z, 0, sizeof(z));
		 for (int i = j; i >= k; i--)
			 z[j - i] = divisor.s[i] + '0';
			 BigInteger dividend = z;
			 if (dividend < divid) { k--; continue; }
			 int key = 0;
			 while (divid*key <= dividend) key++;
			 key--;
			 ans.s[k] = key;
			 BigInteger temp = divid*key;
			 for (int i = 0; i < k; i++)
				 temp = temp * 10;
				 divisor = divisor - temp;
				 k--;
	 }
	 ans.clean();
	 ans.sign = !(sign^num.sign);
	 return ans;
}

BigInteger BigInteger::operator/=(const BigInteger& num)
{
	*this = *this / num;
	return *this;
}

BigInteger BigInteger::operator%(const BigInteger& num) const
{
	BigInteger a = *this, b = num;
	a.sign = b.sign = 1;
	BigInteger result, temp = a / b*b;
	result = a - temp;
	result.sign = sign;
	return result;
}

BigInteger BigInteger::Pow(const BigInteger& num) const
{
	BigInteger result = 1;
	for (BigInteger i = 0; i < num; i++)
		result = result * (*this);
		return result;
}

BigInteger BigInteger::factorial() const
{
	BigInteger result = 1;
	for (BigInteger i = 1; i <= *this; i++)
		result *= i;
	return result;
}

void BigInteger::clean()
{
	if (len == 0) len++;
	while (len > 1 && s[len - 1] == '\0')
		len--;
}

BigInteger BigInteger::Sqrt() const
{
	if(*this < 0) return - 1;
	if(*this <= 1)return *this;
	BigInteger l = 0, r = *this, mid;
	while(r - l > 1)
	{
		mid = (l + r) / 2;
		if(mid * mid > *this)
			r = mid;
		else
			l = mid;
	}
	return l;
}

BigInteger::~BigInteger()
{

}
```