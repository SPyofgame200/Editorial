Olympic Sinh Viên 2020 - Không chuyên - Dự trữ nước
===

[Link](https://oj.vnoi.info/problem/olp_kc20_game11)

-----

### Hướng dẫn

Xét tại vị trí $i$, gọi mực nước dâng tới $b_i$

- Nếu như không tồn tại $j < i$ để $a_j > a_i$ hoặc không tồn tại $k > i$ để $a_k > a_i$, thì nước sẽ không đọng lại, nên $b_i$
- Ngược lại mực nước không được dâng quá $L = max(a_1, a_2, \dots, a_i)$ và cũng không quá $R = max(a_i, \dots, a_{n-1}, a_n)$ $a_i$ nên $b_i = min(L, R)$

Xét tại vị trí $i$, mình nâng cột lên vị trí $X$, hỏi phần nước còn lại là bao nhiêu

- Phần nước ở ô $i$ là $water = b_i - a_i$
- Phần bị cô cạn là $drain = max(0, min(x, b[i]) - a[i])$
- Phần nước còn lại là $remain = water - drain$

Vì càng dâng cột lên cao, lượng nước còn lại giảm, nên hàm là hàm đơn điệu

$\Rightarrow$ Chặt nhị phân

-----

### Code 

> **Time:** $O(n\ log\ (max(a) - min(a)))$
> **Space:** $O(n)$ 
> **Algo:** Binary Search, Implementation

```cpp=
#include <algorithm>
#include <iostream>
#include <deque>

using namespace std;

typedef long long ll;

const int LIM = 1e5 + 15;
const int INF = 0x3f3f3f3f;
const ll LINF = 0x3f3f3f3f3f3f3f3f;

int n, M;
int a[LIM];
int b[LIM];
bool check(int x)
{
    int remains = 0;
    for (int i = 1; i <= n; ++i)
    {
        int water = b[i] - a[i];
        int drain = max(0, min(x, b[i]) - a[i]);
        remains += water - drain;
        if (remains >= M) return true;
    }

    return false;
}

int main()
{
    ios::sync_with_stdio(NULL);
    cin.tie(NULL);

    cin >> n >> M;
    for (int i = 1; i <= n; ++i)
        cin >> a[i];

    for (int i = 1, pref = 0; i <= n; ++i)
    {
        if (pref < a[i]) pref = a[i];
        b[i] = pref;
    }

    for (int i = n, suff = 0; i >= 1; --i)
    {
        if (suff < a[i]) suff = a[i];
        b[i] = min(b[i], suff);
    }

    int res = -1;
    int mn = *min_element(a + 1, a + n + 1);
    int mx = *max_element(a + 1, a + n + 1);
    for (int l = mn, r = mx; l <= r; )
    {
        int m = (l + r) >> 1;
        if (check(m))
        {
            res = m;
            l = m + 1;
        }
        else 
        {
            r = m - 1;
        }
    }

    cout << res;
    return 0;
}
```