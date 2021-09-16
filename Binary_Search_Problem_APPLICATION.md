Binary_Search_Problem_APPLICATION
===

[Link](URL)

-----

### Hướng dẫn

Vào thời điểm $x$, người thứ $k$ làm được $\left \lfloor \frac{x}{T_k} \right \rfloor$ hồ sơ. Nên ta có tổng cộng $f(x) = \overset{n}{\underset{k = 1}{\Large \Sigma}} \left \lfloor \frac{x}{T_k} \right \rfloor$ hồ sơ được làm

Nhiệm vụ của ta là tìm số $x$ nhỏ nhất thỏa $f(x) \geq m$

Nhận thấy vì $T_k > 0\ \forall\ k = 1 \dots n$ nên ta có $f(x) \leq f(x + 1)\ \forall\ x \geq 0$. Hay dãy $f(x)$ tăng đơn điệu.

- Nếu $f(p) \geq m$ thì $f(q) \geq m\ \forall\ q \geq p$
- Nếu $f(p) < m$ thì $f(q) < m\ \forall\ q < p$

Vậy ta dùng chặt nhị phân, bắt đầu với một đoạn $[L, R]$ mỗi bước chọn một vị trí $x$ ở giữa đoạn. 

- Nếu $f(x) \geq m$ thì kết quả hoặc là $x$ hoặc là nhỏ hơn $x$ nhưng chắc chắn không thể lớn hơn $x$. Nên gán $R := x - 1$ và cập nhật kết quả $x$ hiện tại là tốt nhất
- Nếu $f(x) < m$ thì kết quả không thể nhỏ hơn hoặc bằng $x$. Nên gán $L := x + 1$

Trong trường hợp xấu nhất $T_k = 10^9$ với mọi $k$ làm cho hàm $f(x)$ tăng chậm và $m = 10^{18}$ để $x$ cần tìm phải lớn nhất thì kết quả là $10^{18}$. 

Ngược lại trong trường hợp tốt nhất khi $m = 0$ có kết quả nhỏ nhất cần tìm là $0$

Nên ta phải thử đoạn $[L, R] = [0, 10^{18}]$

-----

### Độ phức tạp

Xét đoạn $[L, R]$ bất kì, nếu ta chọn một vị trí $x$ thì ta xét đoạn $[L, x)$ hoặc $(x, R]$ trong bước tiếp theo. Trong trường hợp xấu nhất ta phải chọn đoạn có độ dài lớn hơn

Để tối ưu, ta sẽ chọn vị trí $x$ sao cho nó nằm ở giữa $[L, R]$. Khi này mỗi bước độ dài giảm đi khoảng $1$ nửa. Nên sẽ lặp số lần là $max(k | 2^k \leq R - L + 1) = O(log_2(r - l + 1))$

-----

### Code 

> **Time:** $O(n + log_2(10^{18}))$
> **Space:** $O(n)$ 
> **Algo:** Binary Search, Implementation, Basic Math

```cpp=
#include <iostream>
#include <cstdio>

using namespace std;

void file(const string FILE = "Test")
{
    freopen((FILE + ".INP").c_str(), "r", stdin);
    freopen((FILE + ".OUT").c_str(), "w", stdout);
}

typedef long long ll;
const ll LINF = 0x3f3f3f3f3f3f3f3f;
const int INF = 0x3f3f3f3f;
const int LIM = 1e5 + 15;

int n, m;
int T[LIM];
bool check(ll x)
{
    ll cnt = 0;
    for (int i = 1; i <= n; ++i)
    {
        cnt += x / T[i];
        if (cnt >= m) return true; /// Da lam du ho so
    }

    return false; /// Chua lam du ho so
}

int main()
{
    ios::sync_with_stdio(NULL);
    cin.tie(NULL);
    file("APPLICATION");

    cin >> n >> m;
    for (int i = 1; i <= n; ++i)
        cin >> T[i];

    ll res = -1;
    for (ll l = 0, r = +LINF; l <= r; )
    {
        ll x = (l + r) >> 1;
        if (check(x)) /// Doan [x, +oo) deu thoa
        {
            res = x;
            r = x - 1;
        }
        else /// Doan [0, x] deu khong thoa
        {
            l = x + 1;
        }
    }

    cout << res;
    return 0;
}
```