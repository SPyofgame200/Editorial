Free Contest Testing Round 20 - SEQ51
===

[Link](https://oj.vnoi.info/problem/fct020_seq51)

-----

### Hướng dẫn

Ta muốn tìm một đoạn $[L, R]$ thỏa điều kiện $a_L \geq 1, a_{L+1} \geq 2, \dots, a_r \geq (R-L+1)$ **[1]** 

Xét dãy $b_p = a_p - p$ với $p = 1 \dots n$

Ta có **[1]** $\Leftrightarrow b_L + L \geq 1, b_{L+1} \geq 1, \dots, b_R \geq 1 \Leftrightarrow min(b_L, b_{L+1}, \dots, b_R) \geq 1$

Để cập nhật kết quả ta có thể dùng
- Chặt nhị phân
- Hai con trỏ 

Để tính min đoạn (RMQ) ta có thể dùng
- Segment Tree: $O(n)$ dựng - $O(log\ n)$ truy vấn
- Sprase Table: $O(n\ log\ n)$ dựng - $O(1)$ truy vấn
- SQRT Tree: $O(n\ log\ log\ n)$ dựng - $O(1)$ truy vấn
- Sliding Window Deque // Cartesian Tree: $O(1)$ cập nhật - $O(1)$ truy vấn

-----

### Code 0

> **Time:** $O(n^2)$
> **Space:** $O(n)$
> **Algo:** Brute-foces

```cpp=
#include <iostream>

using namespace std;

const int LIM = 1e5 + 15;

int n;
int a[LIM];
int main()
{
    ios::sync_with_stdio(NULL);
    cin.tie(NULL);

    cin >> n;
    for (int i = 1; i <= n; ++i)
        cin >> a[i];
    
    int res = 0;
    for (int i = 1; i <= n; ++i)
        for (int j = i, p = 1; j <= n && a[j] >= p; ++j, ++p)
            if (res < p)
                res = p;
    
    cout << res;
    return 0;
}
```

-----
### Code 1


> **Time:** $O(n\ log\ n)$ precal + $O(n \times log\ n)$ cal
> **Space:** $O(n\ log\ n)$
> **Algo:** Sprase Table + Binary Search

```cpp=
#include <iostream>
#include <cmath>

using namespace std;

const int LIM = 1e5 + 15;
const int LOG_LIM = ceil(log(LIM) / log(2)) + 1;

int n;
int a[LIM];
int l2[LIM];
int up[LIM][LOG_LIM];
void construct_sprase_table()
{
    l2[0] = l2[1] = 0;
    for (int i = 2; i <= n; ++i)
        l2[i] = l2[i >> 1] + 1;

    for (int i = 1; i <= n; ++i)
        up[i][0] = a[i];
    
    for (int k = 1; (1 << k) <= n; ++k)
        for (int i = 1; i + (1 << k) - 1 <= n; ++i)
            up[i][k] = min(up[i][k - 1], up[i + (1 << (k - 1))][k - 1]);

}

int query(int l, int r)
{
    int k = l2[r - l + 1];
    return min(up[l][k], up[r - (1 << k) + 1][k]);
}

int main()
{
    ios::sync_with_stdio(NULL);
    cin.tie(NULL);

    cin >> n;
    for (int i = 1; i <= n; ++i)
        cin >> a[i];
    
    for (int i = 1; i <= n; ++i)
        a[i] -= i;

    construct_sprase_table();
    int res = 0;
    for (int i = 1; i + res <= n; ++i)
    {
        int j = -1;
        for (int l = i + res, r = n; l <= r; )
        {
            int m = (l + r) >> 1;  
            if (query(i, m) + i >= 1)
            {
                j = m;
                l = m + 1;
            }
            else 
            {
                r = m - 1;
            }
        }

        if (j != -1) res = j - i + 1;
    }

    cout << res;
    return 0;
}
```

-----

### Code 2

> **Time:** $O(n\ log\ n)$ precal + $O(n)$ cal
> **Space:** $O(n\ log\ n)$
> **Algo:** Sprase Table + Two Pointers

```cpp=
#include <iostream>
#include <cmath>

using namespace std;

const int LIM = 1e5 + 15;
const int LOG_LIM = ceil(log(LIM) / log(2)) + 1;

int n;
int a[LIM];
int l2[LIM];
int st[LIM][LOG_LIM];

void construct_sparse_table()
{
    l2[0] = l2[1] = 0;
    for (int i = 2; i <= n; ++i)
        l2[i] = l2[i >> 1] + 1;

    for (int i = 1; i <= n; ++i)
        st[i][0] = a[i];
    
    for (int k = 1; (1 << k) <= n; ++k)
        for (int i = 1; i + (1 << k) - 1 <= n; ++i)
            st[i][k] = min(st[i][k - 1], st[i + (1 << (k - 1))][k - 1]);
}

int query(int l, int r)
{
    int k = l2[r - l + 1];
    return min(st[l][k], st[r - (1 << k) + 1][k]);
}

int main()
{
    ios::sync_with_stdio(NULL);
    cin.tie(NULL);

    cin >> n;
    for (int i = 1; i <= n; ++i)
    {
        cin >> a[i];
        a[i] -= i;
    }

    construct_sparse_table();
    int res = 0;
    for (int l = 1, r = 1; l <= n; ++l)
    {
        if (r < l) r = l;
        while (r + 1 <= n && query(l, r + 1) + l >= 1) ++r;
        if (res < r - l + 1)
            res = r - l + 1;
    }

    cout << res;
    return 0;
}
```


-----

### Code 3

> **Time:** $O(n)$
> **Space:** $O(n)$
> **Algo:** Sliding Window Deque

```cpp=
#include <iostream>
#include <deque>

using namespace std;

const int LIM = 1e5 + 15;

int n;
int a[LIM];
int main()
{
    ios::sync_with_stdio(NULL);
    cin.tie(NULL);

    cin >> n;
    for (int i = 1; i <= n; ++i)
    {
        cin >> a[i];
        a[i] -= i;
    }

    deque<int> S;
    int res = 0;
    for (int l = 1, r = 1; l <= n; ++l)
    {
        while (S.size() && S.front() < l) S.pop_front();
        for (r = max(l, r); r <= n; ++r)
        {
            while (S.size() && a[S.back()] >= a[r]) S.pop_back();
            S.push_back(r);
            if (a[S.front()] + l < 1) break;
            res = max(res, r - l + 1);
        }
    }

    cout << res;
    return 0;
}
```