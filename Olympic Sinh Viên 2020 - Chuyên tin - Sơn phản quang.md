Olympic Sinh Viên 2020 - Chuyên tin - Sơn phản quang
===

[URL](https://oj.vnoi.info/problem/olp_ct20_reflective)

-----

### Hướng dẫn

Gọi hàm $f(l, r)$ là chi phí để sơn một đoạn $[l, r]$

Gọi hàm $[x = y]$ trả về $1$ khi $x = y$ và trả về $0$ khi $x \neq y$

Ta có $f(0, n) = \overset{n}{\underset{x = 0}{\Large \Sigma}} max\Large(\normalsize k\ \ \Large|\ \ \normalsize \frac{x}{2^k} \in Z \wedge\ \normalsize \frac{x}{2^{k+1}} \notin Z \Large) \normalsize = \overset{n}{\underset{x = 0}{\Large \Sigma}} \overset{log_2(x)}{\underset{k = 1}{\Large \Sigma}} \Large[\normalsize \frac{x}{2^k} \in \mathbb{Z} \Large] \normalsize = \overset{log_2(n)}{\underset{k = 1}{\Large \Sigma}} \overset{n}{\underset{x = 0}{\Large \Sigma}} \Large[\normalsize x = 2^k\Large] \normalsize = \overset{log_2(n)}{\underset{k = 1}{\Large \Sigma}} \Large \lfloor \normalsize \frac{n}{2^k} \Large \rfloor$

Ta có kết quả của bài toán $f(l, r) = f(0, r) - f(0, l - 1)$

Độ phức tạp: $O(log_2(n))$ thời gian - $O(1)$ bộ nhớ


-----

### Code

> **Time:** $O(log_2\ n)$
> **Space:** $O(1)$ 
> **Algo:** Math, Implementation
> 
```cpp=
#include <iostream>

using namespace std;

typedef long long ll;

ll f(ll n)
{
    ll res = 0;
    for (ll x = 2; x <= n; x *= 2)
        res += n / x;

    return res;
}

int main()
{
    ll l, r;
    cin >> l >> r;
    cout << f(r) - f(l - 1);
    return 0;
}
```