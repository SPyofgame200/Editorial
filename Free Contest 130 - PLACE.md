

Free Contest 130 - PLACE
===

[https://oj.vnoi.info/problem/fc130_place](https://oj.vnoi.info/problem/fc130_place)

-----


### Gợi ý:

- Khoảng cách lớn nhất giữa $2$ số nguyên tố liên tiếp không quá $10^9$ là $g = 282$

----

### Hướng dẫn


Xét tập $S = \{2, 3, \dots, B\}$ và $T = \{B + 1, B + 2, \dots, A\}$$

Mình cần tìm số $x \in T$ lớn nhất sao cho $\nexists\ d \in S$ mà $d\ |\ x$

**Nhận xét 1:** Mọi số nguyên tố $p > B$  đều không có ước trong tập $S$ nên nếu có số nguyên tố trong tập $T$ thì chắc chắn có kết quả. Mà vì khoảng cách $d$ lớn nhất giữa $2$ số $p, q$ không có ước thuộc tập $S$ không thể lớn hơn khoảng cách của $2$ số nguyên tố trong đoạn [B + 1, A]. Mà ta có $A \leq 10^9$ nên $d \leq g = 282$

**Nhận xét 2:** Mọi số nguyên thuộc $y$ thỏa $(\frac{x}{2} < y < x)$ đều không phải là ước của $x$. Nên ta chỉ cần duyệt qua các số $d \leq min(B, \sqrt{A})$

$\Rightarrow$ Từ $2$ nhận xét trên, ta có số trường hợp mình cần thử là $O(g \times min(B, \sqrt{A}))$. Đủ để qua được bài này

---

**Có thể tối ưu thêm:** 
- Hiển nhiên $x$ là số lẻ vì số chẵn chia hết cho $2$ nên mình chỉ cần kiểm tra số lẻ. Điều này giảm số số phải kiểm tra đi $2$ lần. Ta có thể làm thêm với một số ít số nguyên tố tiếp theo để giảm thêm số số phải kiểm tra, nhưng cải thiện không đáng kể.
- Nếu $d\ |\ x$ thì mọi số nguyên tố $p\ |\ d$ đều thỏa $p\ |\ x$ nên mình chỉ cần kiểm tra qua các số nguyên tố. Và số lượng số nguyên tố không quá $n$ là $O(\pi(n)) \approx O(\frac{n}{\ln n})$
- Ta cũng có thể dùng các thuật toán để phân tích thừa số nguyên tố như Pollard-rho để kiểm tra xem nếu $x$ có ước thuộc tập $\{2, 3, \dots, B\}$ hay không.

-----

### Simple Code 

> For constant $g = 282$ is maximal gap of consecutive prime below $10^9$
> **Time:** $O(g \times min(B, \sqrt{A}))$
> **Space:** $O(1)$ 
> **Algo:** Brute-forces, Math
> 

```cpp=
#include <iostream>

using namespace std;

bool check(int b, int a)
{
    for (int d = 2; d <= b && d * d <= a; ++d)
        if (a % d == 0)
            return false;

    return true;
}

int main()
{
    int b, a;
    cin >> b >> a;
    while (b < a && !check(b, a)) --a;

    cout << (a <= b ? -1 : a);
    return 0;
}
```

-----

### Optimized Code 

> For $C = min(B, \sqrt{A})$
> For function $\pi(n) =$ number of primes not greater than $n$
> For constant $g = 282$ is maximal gap of consecutive prime below $10^9$
> **Time:** $O \left (g \times \pi(C) \right ) \approx O \left(g \times \frac{\sqrt{A}}{ln(\sqrt{A})} \right)$
> **Space:** $O \left(\pi(C)\right)$ 
> **Algo:** Sieving, Math

```cpp=
#include <iostream>
#include <vector>
#include <cmath>

using namespace std;

/// SPyofgame Linear Sieve
vector<int> prime;
vector<int> lpf;
void sieve(int n)
{
    prime.assign(1, 2);
    lpf.assign(n + 1, 2);

    for (int x = 3; x <= n; x += 2)
    {
        if (lpf[x] == 2) prime.push_back(lpf[x] = x);
        for (int i = 0; i < prime.size() && prime[i] <= lpf[x] && prime[i] * x <= n; ++i)
            lpf[prime[i] * lpf[x]] = prime[i];
    }
}

const int g = 282;
bool check(int b, int a)
{
    for (int p : prime)
    {
        if (p > b) return true; /// Not found any number in [2, b] is a divisor of (a)
        if (a % p == 0) return false; /// Exist a number in [2, b] is a divisor of (a)
    }
    return true;
}

int main()
{
    /// Input
    int b, a;
    cin >> b >> a;    
    if (b >= a) /// There is no answer
    {
        cout << -1;
        return 0;
    }
    
    sieve(min(b, int(sqrt(a))));
    if (a % 2 == 0) --a; /// 2 is a divisor of (a)
    while (b < a && !check(b, a)) a -= 2; /// Only care for odd (a)
    cout << (a <= b ? -1 : a);
    return 0;
}
```