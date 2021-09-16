


Olympic Sinh Viên 2020 - Không chuyên - Phân số
===

[https://oj.vnoi.info/problem/olp_kc20_fraction](https://oj.vnoi.info/problem/olp_kc20_fraction)

Co-author: [Nguyễn Nam](https://www.facebook.com/nguyennam.2018)

-----

### Bài toán con

> Xét bài toán kiểm tra xem phân số $\frac{X}{Y}$ có phải là số thập phân tuần hoàn vô hạn hay không

Xét $\frac{A}{B} = \frac{X \div gcd(X, Y)}{Y \div gcd(X, Y)}$ là phân số tối giản của $\frac{X}{Y}$. 

Khi $B = 2^u \times 5^v$ với $u, v \in \mathbb{N}$ thì $\frac{A}{B} = \frac{A \times 2^{k - u} \times 5^{k - v}}{10^k}$ với $k = max(u, v)$. Nên $\frac{A}{B}$ là số thập phân hữu hạn

Ngược lại $\exists$ số nguyên tố $p \notin \{2, 5\}$ thỏa $B\ \vdots\ p$. Ta sẽ chứng minh $\frac{A}{B}$ vô hạn tuần hoàn

Giả sử $\frac{A}{B}$ hữu hạn hay $\frac{A}{B} = \overline{c_1c_2\dots c_u\LARGE,\normalsize d_1d_2 \dots d_v}$ 

$\Rightarrow \frac{A}{B} \times 10^v = \overline{c_1c_2\dots c_ud_1d_2 \dots d_v}$

$\Rightarrow \Large \frac{A \times 10^v}{B} \normalsize \in \mathbb{Z}$

$\Rightarrow A \times 10^v\ \vdots\ B$

$\Rightarrow\ \ \ 10^v\ \ \ \ \vdots\ B\ \ \ \ \ \ \ \ \ \ \ \  \Large($ vì $\frac{A}{B}$ tối giản $\Rightarrow (A, B) = 1\Large)$

$\Rightarrow\ \ \ 10^v\ \ \ \ \vdots\ \ p\ \ \ \ \ \ \ \ \ \ \ \ \Large($ vì $B\ \vdots\ p \Large)$

Mà $gcd(p, 10) = 1 \Rightarrow \Large(\normalsize dpcm\Large)$

**Kết luận:** 

- $\frac{A}{B}$ vô hạn tuần hoàn khi và chỉ khi tồn tại số nguyên tố $p \notin \{2, 5\}$ mà $B\ \vdots\ p$

- $\frac{X}{Y}$ vô hạn tuần hoàn khi và chỉ khi tồn tại số nguyên tố $p \notin \{2, 5\}$ mà $\frac{Y}{gcd(X,Y)}\ \vdots\ p$

-----

### Bài toán chính 

> Kiểm tra $\frac{\overset{n}{\underset{i = 1}{\Large \prod}} \Large( \normalsize a_i \Large)}{\overset{n}{\underset{i = 1}{\Large \prod}} \Large( \normalsize b_i \Large)} = \frac{a_1 \times a_2 \times \dots \times a_n}{b_1 \times b_2 \times \dots \times b_n} = \Large \frac{X}{Y}$ có phải là số thập phân vô hạn tuần hoàn hay không

Đặt $p_1, p_2, \dots, p_k$ là các số nguyên tố phân biệt, khi đó ta có:

- $X = p_1 ^ {d_1} \times p_2 ^ {d_2} \times \dots \times p_k ^ {d_k}$ với $d_i \in \mathbb{N}$
- $Y = p_1 ^ {f_1} \times p_2 ^ {f_2} \times \dots \times p_k ^ {f_k}$ với $f_i \in \mathbb{N}$
- $G = gcd(X, Y) = p_1 ^ {min(d_1, f_1)} \times p_2 ^ {min(d_2, f_2)} \times \dots \times p_k ^ {min(d_k, f_k)}$
- $B = gcd(X, Y) = p_1 ^ {f_1 - min(d_1, f_1)} \times p_2 ^ {min(d_2, f_2)} \times \dots \times p_k ^ {min(d_k, f_k)}$


Áp dụng bài toán con, ta có phân số $\frac{X}{Y}$ là số thập phân vô hạn tuần hoàn khi và chỉ khi tồn tại vị trí $x$ để $p_x \notin \{2, 5\}$ mà $f_x - min(d_x, f_x) > 0 \Leftrightarrow f_x - d_x > 0$

Vậy ta chỉ cần phân tích thừa số nguyên tố các số $a_i, b_i$ và kiểm tra từng số nguyên tố $p \notin \{2, 5\}$ xem có thỏa hay không

-----

### Code 

> Với $v = max\Large(\normalsize max(a_i, b_i) \Large\  \ \normalsize i = 1\dots n \Large)$
> **Time:** $O(q \times n \times log_2(v))$
> **Space:** $O(q \times (n + v))$ 
> **Algo:** Sieving, Factorization, Implementation

```cpp=
#include <iostream>
#include <cstring>
#include <vector>

using namespace std;

const int LIM = 1e6 + 1;

/// SPyofgame linear sieving
vector<int> prime; /// prime list
vector<int> lpf; /// lowest prime factor
void sieve(int n = LIM)
{
    prime.assign(1, 2);
    lpf.assign(n + 1, 2);

    lpf[1] = -2;
    for (int x = 3; x <= n; x += 2)
    {
        if (lpf[x] == 2) prime.push_back(lpf[x] = x);
        for (int i = 0; i < prime.size() && prime[i] <= lpf[x] && prime[i] * x <= n; ++i)
            lpf[prime[i] * x] = prime[i];
    }
}

int G[LIM];
int query()
{
    int n;
    cin >> n;

    bool ok = false;
    memset(G, 0, sizeof(G));
    for (int i = 1; i <= 2 * n; ++i)
    {
        int x;
        cin >> x;

        if (ok) continue;
        while (x > 1)
        {
            int p = lpf[x], f = 0;
            do x /= p, ++f;
            while (lpf[x] == p);
            
            if (p == 2 || p == 5) continue;
            if (i <= n)
                G[p] -= f;
            else 
            {
                G[p] += f;
                if (G[p] > 0) /// f[i] - d[i] > 0
                {
                    ok = true;
                    break;
                }
            }
        }
    }

    cout << (ok ? "repeating\n" : "finite\n");
    return 0;
}

int main()
{
    ios::sync_with_stdio(NULL);
    cin.tie(NULL);

    int q;
    cin >> q;

    sieve();
    while (q-->0)
    {
        query();

    }
    
    return 0;
}
```