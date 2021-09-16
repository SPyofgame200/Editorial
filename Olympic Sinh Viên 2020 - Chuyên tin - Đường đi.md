
Olympic Sinh Viên 2020 - Chuyên tin - Đường đi
===

[https://oj.vnoi.info/problem/olp_ct20_route](https://oj.vnoi.info/problem/olp_ct20_route)

-----

**Credit:** [@darkkcyan](https://codeforces.com/profile/darkkcyan)

-----

### Hướng dẫn tính giá trị

Ta định nghĩa

- $P$ là giá trị tích các số trên đường đi hiện tại
- $V$ là số số $0$ ở cuối $P$
- $c_x$ là số thừa số $x$ trong $P$
- $m_x$ là số lượng tối thiểu lấy được số $x$ trên đường đi bất kì

Nếu trên đường đi có $c_0 > 0$ thì $V = 1$

Nếu trên đường đi có $c_0 = 0$ thì $P = a \times 10^{c_{10}} = b \times 2^{c_2} \times 5^{c_5} \Rightarrow V = c_{10} = min(c_2, c_5)$ với $a, b$ là giá trị b 

- Không mất tính tổng quát, giả sử $m_2 \leq m_5$. Nếu ta chọn $c_2 > m_2$ thì đường đi hiện tại là không tối ưu vì $min(c_2, c_5) = c_5 > m_2 = min(m_2, m_5)$
- Chứng minh tương tự với $m_2 \geq m_5$, nếu chọn $c_5 > m_5$ thì đường đi này không tối ưu
- Vậy đường đi có $V$ nhỏ nhất ở trong ít nhất 1 trong hai đường thỏa mãn $c_2 = m_2$ hoặc $c_5 = v_5$
 
Vậy ta chỉ cần xét 3 đường đi là có thể tính giá trị nhỏ nhất
- Đường có có thứ tự từ điển nhỏ nhất lấy ít thừa số $2$ nhất ($c_0 = 0$ và $c_2 = m_2$)
- Đường có có thứ tự từ điển nhỏ nhất lấy ít thừa số $5$ nhất ($c_0 = 0$ và $c_5 = m_5$)
- Đường có có thứ tự từ điển nhỏ nhất lấy ít nhất một số $0$ ($c_0 > 0$)

Bằng quy hoạch động và BFS, việc tính giá trị rất đơn giản

---

### Hướng dẫn truy vết

Để chọn đường đi có $c_x = m_x$, di chuyển từ $(1, 1)$ đến $(n, m)$ mà có thứ tự từ điển nhỏ nhất, ta làm như sau
- Không mất tính tổng quát coi vị trí hiện tại là $(x, y)$
- Gọi $f(x, y)$ là giá trị nhỏ nhất có thể lấy được khi đi từ $(x, y)$ đến $(n, m)$
- Khi $(x = n)$ thì ta chỉ có thể di chuyển trên hàng, nên đi "**L**"
- Khi $(y = m)$ thì ta chỉ có thể di chuyển trên cột, nên đi "**D**"
- Nếu $f(x + 1, y) = f(x, y + 1)$ thì đi kiểu gì cũng tồn tại đường thỏa $c_x = m_x$ nên ta đi theo đường có thứ tự từ điển nhỏ hơn là "**D**"
- Nếu $f(x + 1, y) > f(x, y + 1)$ thì đường đi theo cột không tối ưu, nên ta đi "**L**"
- Nếu $f(x + 1, y) < f(x, y + 1)$ thì đường đi theo hàng không tối ưu, nên ta đi "**D**"

Để chọn đường đi có $c_x > k$, di chuyển từ $(1, 1)$ đến $(n, m)$ mà có thứ tự từ điển nhỏ nhất, ta làm như sau
- Không mất tính tổng quát coi vị trí hiện tại là $(x, y)$
- Số lượng phần thừa số $x$ lấy được nhiều nhất từ $(1, 1)$ đến $(x, y)$ là $v$
- Gọi $f(x, y)$ là giá trị lớn nhất có thể lấy được khi đi từ $(x, y)$ đến $(n, m)$
- Nếu $f(1, 1) \leq k$ thì không tồn tại đường đi 
- Khi $(x = n)$ thì ta chỉ có thể di chuyển trên hàng, nên đi "**L**"
- Khi $(y = m)$ thì ta chỉ có thể di chuyển trên cột, nên đi "**D**"
- Nếu $f(x + 1, y) = f(x, y + 1)$ thì đi kiểu gì cũng tồn tại đường thỏa $c_x = m_x$ nên ta đi theo đường có thứ tự từ điển nhỏ hơn là "**D**"
- Nếu $f(x + 1, y) + v \leq k$ thì đi "**D**" không thì đi "**L**"

-----

### Code 

**Bonus:** [Code with checker](https://ideone.com/Wyx6rx)

> **Time:** $O(n \times m \times (log_2(a) + log_5(a) + 1))$
> **Space:** $O(n \times m)$ 
> **Algo:** DP, Constructive, Medium Implementation, Lexicographical Order

```cpp=
#include <iostream>
#include <cstring>
#include <vector>
#include <deque>

using namespace std;

const int mx[] = {+0, +1};
const int my[] = {+1, +0};

struct Info
{
    int x, y, d;
    Info (int x = 0, int y = 0, int d = 0)
    : x(x), y(y), d(d) {}
};

struct Pack
{
    int res;
    string way;
    Pack (int res = 0, string way = "")
    : res(res), way(way) {}
};

Pack min(const Pack &a, const Pack &b)
{
    if (a.res < b.res) return a; /// Left is smaller path
    if (a.res > b.res) return b; /// Right is smaller path
    return (a.way < b.way) ? a : b; /// Choose smaller lexicographical order
}

const int INF = 0x3f3f3f3f;
const int LIM = 1e3 + 13;

int n, m;
int a[LIM][LIM];
int f[LIM][LIM];
int n2[LIM][LIM];
int n5[LIM][LIM];

Pack solve_0()
{
    memset(f, -1, sizeof(f));
    f[n][m] = !a[n][m];
    
    deque<Info> S;
    S.push_back(Info(n, m, f[n][m]));
    while (S.size())
    {
        /// Take current path
        int x = S.front().x;
        int y = S.front().y;
        int d = S.front().d;
        S.pop_front();

        if (d < f[x][y]) continue; /// Not optimal
        for (int k = 0; k < 2; ++k)
        {
            int u = x - mx[k];
            int v = y - my[k];
            if (u < 1 || u > n) continue; /// Out of row
            if (v < 1 || v > m) continue; /// Out of col

            int g = f[x][y] || !a[u][v];
            if (f[u][v] < g) /// Take part with a[i][j] = 0
            {
                f[u][v] = g;
                S.push_back(Info(u, v, f[u][v]));
            }
        }
    }
    
    /// Trace back [1][1] -> [n][m]
    bool ok = false;
    int res = f[1][1];
    string way(n + m - 2, 'z');
    if (res == 0) return Pack(+INF, way);
    
    for (int x = 1, y = 1, p = 0; p < way.size(); ++p)
    {
        if (x == n) /// Top row
        {
            while (p < way.size()) way[p++] = 'L';
            break;
        }

        if (y == m) /// Top column
        {
            while (p < way.size()) way[p++] = 'D';
            break;
        }

        /// Priority contain a[i][j] = 0 -> Priority f[][] contain a[i][j] = 0 -> 'D'
        ok |= !a[x][y];
        if (ok || f[x + 1][y] >= f[x][y + 1])
        {
            way[p] = 'D';
            ++x;
        }
        else 
        {
            way[p] = 'L';
            ++y;
        }
    }

    return Pack(res, way);
}

Pack solve(int a[LIM][LIM])
{
    /// Init DP
    memset(f, +INF, sizeof(f));
    f[n][m] = a[n][m];

    deque<Info> S;
    S.push_back(Info(n, m, f[n][m]));
    while (S.size())
    {
        /// Take current path
        int x = S.front().x;
        int y = S.front().y;
        int d = S.front().d;
        S.pop_front();

        if (d > f[x][y]) continue; /// Not optimal
        for (int k = 0; k < 2; ++k)
        {
            int u = x - mx[k];
            int v = y - my[k];
            if (u < 1 || u > n) continue; /// Out of row
            if (v < 1 || v > m) continue; /// Out of col

            int g = f[x][y] + a[u][v];
            if (f[u][v] > g) /// Take smaller path
            {
                f[u][v] = g;
                S.push_back(Info(u, v, f[u][v]));
            }
        }
    }

    /// Trace back [1][1] -> [n][m]
    int res = f[1][1];
    string way(n + m - 2, 'z');
    for (int x = 1, y = 1, p = 0; p < way.size(); ++p)
    {
        if (x == n) /// Top row
        {
            while (p < way.size()) way[p++] = 'L';
            break;
        }

        if (y == m) /// Top column
        {
            while (p < way.size()) way[p++] = 'D';
            break;
        }
        
        /// Priority Smaller f[][] -> Priority 'D'
        if (f[x + 1][y] <= f[x][y + 1])
        {
            way[p] = 'D';
            ++x;
        }
        else 
        {
            way[p] = 'L';
            ++y;
        }
    }

    return Pack(res, way);
}

int main()
{
    /// FASTIO
    ios::sync_with_stdio(NULL);
    cin.tie(NULL);

    /// Input
    cin >> n >> m;
    for (int i = 1; i <= n; ++i)
        for (int j = 1; j <= m; ++j)
            cin >> a[i][j];
    
    /// Construct n2[][], n5[][]
    memset(n2, 0, sizeof(n2));
    memset(n5, 0, sizeof(n5));
    for (int i = 1; i <= n; ++i)
    {
        for (int j = 1; j <= m; ++j)
        {
            cin >> a[i][j];
            if (a[i][j] == 0)
            {
                /// We ban the way with zero
                n2[i][j] = +INF;
                n5[i][j] = +INF;
                continue;
            }

            /// Count factors
            for (int t = a[i][j]; t % 2 == 0; t /= 2) ++n2[i][j];
            for (int t = a[i][j]; t % 5 == 0; t /= 5) ++n5[i][j];
        }
    }

    Pack res;
    res = min(solve_0(), min(solve(n2), solve(n5)));

    /// Output
    cout << res.res << "\n" << res.way;
    return 0;
}
```
