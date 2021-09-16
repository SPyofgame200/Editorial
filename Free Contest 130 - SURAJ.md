
Free Contest 130 - SURAJ
===

[https://oj.vnoi.info/problem/fc130_suraj](https://oj.vnoi.info/problem/fc130_suraj)

-----

### Hướng dẫn

Vì chi phí càng tăng khi càng mở nhiều tab cho một cửa sổ, nên ta ưu tiên mở các cửa sổ có ít tab nhất trước.

Để đẩy nhanh tiến độ, ta có thể tìm số $x$ lớn nhất mà $v = k \times \frac{x(x+1)}{2} \leq m$ sau đó ta mở $k$ cửa sổ với $x$ tab đầu tiên.
Phần thừa còn lại ta sẽ dành để mở được $\lfloor \frac{m - v}{x + 1} \rfloor$ cửa sổ tại tab thứ $x + 1$

Dễ thấy $f(x) = k \times \frac{x(x+1)}{2}$ là hàm tăng đơn điệu nên ta có thể chặt nhị phân tìm $x$ lớn nhất thỏa $f(x) \leq m$

Ngoài ra ta có thể dùng toán như sau:
- Nghiệm của phương trình $n = \frac{x(x+1)}{2}$ với $n, x \in \mathbb{N}$ là $\frac{\sqrt{8n + 1} - 1}{2}$
- Đặt $n = \left \lfloor \frac{m}{k} \right \rfloor$ ta dễ dàng tìm được $x$ lớn nhất thỏa $f(x) \leq m$



-----

### Code 

> **Time:** $O(q\times \log_2(m))$ total - $O(\log_2(m))$ query
> **Space:** $O(1)$ 
> **Algo:** Binary Search, Math

```cpp=
#include <iostream>
#include <cstdio>
#include <cmath>

using namespace std;

typedef long long ll;
int main()
{
    ios::sync_with_stdio(NULL);
    cin.tie(NULL);

    int q;
    cin >> q;

    while (q-->0)
    {
        ll m;
        int k;
        cin >> m >> k;
        ll n = m / k;

        int x = 0;
        for (int l = 0, r = sqrt(m); l <= r; )
        {
            int t = (l + r) >> 1;
            if (1LL * t * (t + 1) / 2 <= n)
            {
                x = t;
                l = t + 1;
            }
            else 
            {
                r = t - 1;
            }
        }
        
        ll v = 1LL * x * (x + 1) / 2;
        ll res = 1LL * x * k + (m - v * k) / (x + 1);
        cout << res << "\n";
    }
    

    return 0;
}
```

-----

### Code 

> **Time:** $O(q)$ total - $O(1)$ query
> **Space:** $O(1)$ 
> **Algo:** Math

```cpp=
#include <iostream>
#include <cstdio>
#include <cmath>

using namespace std;

typedef long long ll;
int main()
{
    ios::sync_with_stdio(NULL);
    cin.tie(NULL);

    int q;
    cin >> q;

    while (q-->0)
    {
        ll m, k;
        cin >> m >> k;
        ll n = m / k;

        ll x = (sqrtl(8 * n + 1) - 1) / 2;
        ll v = k * x * (x + 1) / 2;
        ll res = x * k + (m - v) / (x + 1);
        cout << res << "\n";
    }
    

    return 0;
}
```