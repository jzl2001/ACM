  Let Fi be fibonacci number. 

  F0 = 0, F1 = 1, Fi = Fi-1 + Fi-2  

  Given n and m, please calculate
$$
\sum^{n}_{i = 0}F^{m}_{i}
$$

## 输入描述:

```
The first and only line contains two integers n, m(1 <= n <= 1000000000, 1 <= m <= 1000).
```

## 输出描述:

```
Output a single integer, the answer module 1000000000.
```

 示例1 

## 输入

[复制](javascript:void(0);)

```
5 5
```

## 输出

[复制](javascript:void(0);)

```
3402
```

## 说明

```
05 + 15 + 15 + 25 + 35 + 55 = 3402
```

 示例2 

## 输入

[复制](javascript:void(0);)

```
10 10
```

## 输出

[复制](javascript:void(0);)

```
696237975
```

## 说明

```
Don't forget to mod 1000000000
```

 示例3 

## 输入

[复制](javascript:void(0);)

```
1000000000 1000
```

## 输出

[复制](javascript:void(0);)

```
641796875
```

## 说明

```
Time is precious.
```

将1e9拆成 512 * 1953125

循环节分别为768, 7812500

CRT合并

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
#ifdef LOCAL
#define debug(x) cout << "[" __FUNCTION__ ": " #x " = " << (x) << "]\n"
#define TIME cout << "RuningTime: " << clock() << "ms\n", 0
#else
#define TIME 0
#endif
#define continue(x) { x; continue; }
#define break(x) { x; break; }
const ll mod = 1e9;
ll m[2] = { 512, 1953125 };
ll fpow(ll a, ll b) { ll res = 1; for (; b > 0; b >>= 1) { if (b & 1) res = res * a % mod; a = a * a % mod; } return res; }
ll exgcd(ll a, ll b, ll& x, ll& y) { if (!b) { x = 1, y = 0; return a; } ll d = exgcd(b, a % b, y, x); y -= (a / b) * x; return d; }
const int N = 8e6 + 10;
ll fb[N];
ll sum[N];
ll a[3];
const ll len[] = { 768, 7812500 };
ll mul(ll a, ll b, ll mod)
{
	int flag = b < 0 ? 1 : 0;
	ll res = 0;
	b = abs(b);
	while (b)
	{
		if (b & 1)
			res = (res + a) % mod;
		b >>= 1;
		a = (a + a) % mod;
	}
	if (flag)
		return -res % mod;
	else
		return res % mod;
}
void CRT(int n)
{
	ll x, y;
	for (int i = 1; i < n; i++)
	{
		ll d = exgcd(m[0], m[i], x, y);
		ll u = a[i] - a[0];
		x = mul(x, u, m[i]) / d;
		m[i] /= d;
		x = (x % m[i] + m[i]) % m[i];
		a[0] += m[0] * x;
		m[0] *= m[i];
	}
	cout << a[0] % mod << endl;
}
int main()
{
#ifdef LOCAL
	freopen("D:/input.txt", "r", stdin);
#endif
	int n, m;
	cin >> n >> m;
	fb[1] = sum[1] = 1;
	for (int i = 2; i <= N - 10; i++)
	{
		fb[i] = (fb[i - 1] + fb[i - 2]) % mod;
		sum[i] = (sum[i - 1] + fpow(fb[i], m)) % mod;
	}
	a[0] = (n / len[0]) * sum[len[0]] + sum[n % len[0]];
	a[1] = (n / len[1]) * sum[len[1]] + sum[n % len[1]];
	CRT(2);
	return TIME;
}
```

