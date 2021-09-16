Bedao Mini Contest 06 - GIRLS
===

[Link](https://oj.vnoi.info/problem/bedao_m06_girls)

-----
### Hướng dẫn

Vì mình cần chọn ra $N$ người có độ chênh lệch giữa người lớn nhất và người bé nhất không quá $K$ nên khi xác định ra được người có giá trị nhỏ nhất $A$ và lớn nhất $B$ thì thêm vô những người có giá trị $X$ nằm giữa $[A, B]$ sẽ không làm thay đổi độ chênh lệch.

Vậy ta chỉ cần sắp xếp lại mảng theo giá trị tăng dần, chọn các đoạn $[l, r]$ gồm $N$ người và kiểm tra xem đoạn này có thỏa điều kiện $max(a[l \dots r]) - min(a[l \dots r]) = a[r] - a[l] \leq K$ hay không

Để tính tổng giá trị của một đoạn $(a[l] + a[l + 1] + ... + a[r])$ thì ta chỉ cần xây dựng mảng cộng dồn và ta có thể tính giá trị một đoạn trong $O(1)$

Vậy bài này có thể làm trong $O(n\ log\ n)$, trong đó

- $O(n\ log\ n)$ để sắp xếp
- $O(n)$ để dựng mảng cộng dồn
- $O(n)$ để duyệt qua các đoạn và cập nhật kết quả

------

### Code

> **Time:** $O(n log n)$
> **Space:** $O(n)$ 
> **Algo:** Sorting, Implementation, Partial Sum, Two Pointers

```cpp=
#include <algorithm>
#include <iostream>

using namespace std;

typedef long long ll;
const int LIM = 1e6 + 16;

ll f[LIM];
int a[LIM];
int main()
{
    /// De nhap du lieu nhanh hon
    ios::sync_with_stdio(NULL);
    cin.tie(NULL);

    /// Input
    int n, d, k;
    cin >> n >> d >> k;
    for (int i = 1; i <= n; ++i)
        cin >> a[i];

    /// Sort gia tri
    sort(a + 1, a + n + 1);

    /// Dung mang cong don
    f[0] = 0;
    for (int i = 1; i <= n; ++i)
        f[i] = f[i - 1] + a[i];

    /// Tinh gia tri
    ll best = -2;
    for (int l = 1, r = d; r <= n; ++l, ++r) /// duyet qua cac doan [l, r] co do dai (d)
    {
        if (a[r] - a[l] <= k) /// thoa dieu kien
        {
            best = max(best, f[r] - f[l - 1]);
        }
    }

    /// Output
    cout << best;
    return 0;
}
```