Free Contest Testing Round 20 - BALLOON
===

[Link](https://oj.vnoi.info/problem/fct020_balloon)

-----

### Hướng dẫn

Gọi $f[i][c]$ là tổng điểm số cao nhất xét đến $[i]$ và đã lấy **đúng** $[c]$ phần tử có vị trí không cách nhau quá $M$ đơn vị

Hiển nhiên $f[i][1] = a[i]$

Từ đề, ta có $f[i][c] = max(f[j][c - 1]\ |\ max(1, i - d) \leq j \leq i) + c \times a_i$

Trong trường hợp không tồn tại số $j$ thỏa mãn thì $f[i][c] = -\infty$

Đê tìm max trên một đoạn cách nhau không quá $M$ đơn vị, ta có nhiều cách
- Segment Tree: $O(n)$ dựng - $O(log\ n)$ truy vấn
- Sprase Table: $O(n\ log\ n)$ dựng - $O(1)$ truy vấn
- SQRT Tree: $O(n\ log\ log\ n)$ dựng - $O(1)$ truy vấn
- Sliding Window Deque // Cartesian Tree: $O(1)$ cập nhật - $O(1)$ truy vấn

Nhận thấy rằng $f[i][c]$ chỉ phụ thuộc vào $f[j][c - 1]$ nên ta có thể giảm chiều $O(n \times k) \rightarrow O(n \times 2)$

-----

### Code 

> **Time:** $O(n \times k)$
> **Space:** $O(n)$ 
> **Algo:** DP, Sliding Window Deque, Space Optimized DP

```cpp=
#include <algorithm>
#include <iostream>
#include <cstring>
#include <cstdio>
#include <deque>

using namespace std;

typedef long long ll;

const int LIM = 200200;
const int INF = 0x3f3f3f3f;
const ll LINF = 0x3f3f3f3f3f3f3f3f;

template<typename T> void maximize(T &res, const T &val) { if (res < val) res = val; }
template<typename T> void minimize(T &res, const T &val) { if (res > val) res = val; }

int n, d, k;
int a[LIM];
ll f[LIM][2];
int main()
{
    ios::sync_with_stdio(NULL);
    cin.tie(NULL);
    
    cin >> n >> d >> k;
    for (int i = 1; i <= n; ++i)
        cin >> a[i];
    
    ll res = *max_element(a + 1, a + n + 1);
    memset(f, 0, sizeof(f));

    for (int i = 1; i <= n; ++i) f[i][1] = a[i];
    for (int t = 2; t <= k; ++t)
    {
        bool cur = t & 1;
        bool pre = !cur;

        deque<int> S;
        for (int i = 1; i <= n; ++i)
        {
            while (S.size() && S.front() < i - d) S.pop_front();
            ll v = (S.empty()) ? -LINF : f[S.front()][pre];
            f[i][cur] = v + 1LL * t * a[i];
            maximize(res, f[i][cur]);
            while (S.size() && f[S.back()][pre] <= f[i][pre]) S.pop_back();
            S.push_back(i);
        }
    }

    cout << res;
    return 0;
}
```