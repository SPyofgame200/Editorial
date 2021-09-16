Free Contest 130 - MERGE
===

[https://oj.vnoi.info/problem/fc130_merge](https://oj.vnoi.info/problem/fc130_merge)

-----

**Contributor:** [@jalsol](https://codeforces.com/profile/jalsol) [@volongskl2003](https://codeforces.com/profile/volongskl2003)

-----

### Hướng dẫn

Xét các tập:
- $A = \{1, 2, \dots, n\}$
- $B = \{n + 1, n + 2, \dots, n + m\}$
- $C = \{n + m + 1, n + m + 2, \dots, n + m + k\}$

**Nhận xét 1:** Đề yêu cầu số tìm cách chọn một tập con $S$ từ 3 tập $A, B, C$ sao cho các phần tử cùng một tập được chọn phải sắp xếp tăng dần. Nên việc chọn các số giữa các tập là độc lập nhau.

**Nhận xét 2:** Số cách để chọn một tập con $S$ bất kì là $x = (n + m + k)!$
- Trong số đó, chỉ có $\frac{x}{n!}$ tập có các phần tử trong tập $A$ sắp xếp tăng dần.
- Trong số đó, chỉ có $\frac{x}{m!}$ tập có các phần tử trong tập $B$ sắp xếp tăng dần.
- Trong số đó, chỉ có $\frac{x}{k!}$ tập có các phần tử trong tập $C$ sắp xếp tăng dần.

Vì việc chọn các tập là độc lập nhau (nhận xét 1), ta có số tập con $S$ thỏa cả 3 tính chất trên là $\frac{(n + m + k)!}{n! \times m! \times k!}$

Vậy kết quả bài toán là $\frac{(n + m + k)!}{n! \times m! \times k!} \bmod (10^9 + 7)$

---

Để tính $x^n$ nhanh ta tính $x^n$ dựa trên $x^{n/2}$ hoặc dùng thuật toán lũy thừa nhị phân.

Để tính $\frac{1}{a!}$ dưới một modulo nguyên tố $b$ mà không cần xử lí số lớn ta áp dụng định lí nhỏ fermat ta có $\frac{1}{a!} = a!^{-1} \large \equiv \normalsize  a!^{b - 2} \pmod b$

-----

### Code 

> For constant modulo $MOD = 10^9 + 7$
> **Time:** $O(n + m + k + 3\log_2(MOD))$
> **Space:** $O(n + m + k)$ 
> **Algo:** Combinatorics, Math, Binary Exponentiation

```cpp=
#include <iostream>

using namespace std;

const int LIM = 3e6 + 36;
const int MOD = 1e9 + 7;

/// x^n % MOD in O(log n)
int powMOD(int x, int n) 
{
    int res = 1;
    for (; n > 0; n >>= 1)
    {
        if (n & 1) res = (1LL * res * x) % MOD;
        x = (1LL * x * x) % MOD;
    }

    return res;
}

/// fact[n] = n!
int fact[LIM];
int main()
{
    int n, m, k;
    cin >> n >> m >> k;

    fact[0] = 1;
    for (int i = 1; i <= n + m + k; ++i)
        fact[i] = (1LL * fact[i - 1] * i) % MOD; /// n! = (n - 1)! * n

    int res = fact[n + m + k]; /// res := (n + m + k)!
    res = (1LL * res * powMOD(fact[n], MOD - 2)) % MOD; /// res := res / n!
    res = (1LL * res * powMOD(fact[m], MOD - 2)) % MOD; /// res := res / m!
    res = (1LL * res * powMOD(fact[k], MOD - 2)) % MOD; /// res := res / k!
    cout << res;
    return 0;
}
```