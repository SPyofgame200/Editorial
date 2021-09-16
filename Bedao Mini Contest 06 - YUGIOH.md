Bedao Mini Contest 06 - YUGIOH
===

-----

### Hướng dẫn 

Để kiểm tra một số bất kì có phải là số nguyên tố không quá $X$ hay không, ta chỉ cần sàng các số nguyên tố trong đoạn $[1, X]$ và có thể kiểm tra trong $O(1)$

Yêu cầu đề bài là chuyển các số $a_i$ nguyên tố $\leq X$ lại gần nhau

$\Rightarrow$ Có thể coi mảng $a[]$ như $n$ số nhị phân, với giá trị $1$ khi $a_i$ nguyên tố $\leq X$ và giá trị $0$ với các số còn lại.

$\Rightarrow$ Gọi $k$ là số số $1$ trong dãy. Lúc này ta chỉ cần đưa $k$ số $1$ kề nhau với ít lần swap nhất

Vì các số $1$ kề nhau nên ta xét các đoạn $[l, r]$ với $(1 \leq l \leq r \leq n)$ và ($r - l + 1 = k)$, ta thử đưa hết các số $1$ vào trong đoạn $[l, r]$

Vì ta cần số lần swap tối thiểu nên ta chỉ swap các số $1$ ở ngoài đoạn với các số $0$ ở trong đoạn.

Nhận thấy rằng số lần swap đó cũng chính bằng số số $0$ ở trong đoạn. Nên ta có thể dựng mảng cộng dồn để đếm số số $0$ với mỗi đoạn $[l, r]$ bất kì trong $O(1)$

----- 

### Độ phức tạp

Vậy độ phức tạp bài này là $O(n + X)$, trong đó:

- Mất $O(X)$ sàng nguyên tố $X$ số đầu
- Mất $O(n)$ để nhận dữ liệu và chuyển về dạng nhị phân
- Mất $O(n)$ để dựng lên mảng tiền tố
- Mất $O(n)$ để duyệt qua các đoạn $[l, r]$ và cập nhật kết quả số lần swap tối thiểu

-----

### Code

```cpp=
#include <algorithm>
#include <iostream>
#include <vector>

using namespace std;

/// Linear SPyofgame Sieve
vector<int> prime; /// prime list
vector<int> lpf;   /// lowest prime factor
void sieve(int n)
{
    if (n <= 1) return ;
    prime.assign(1, 2);
    lpf.assign(n + 1, 2);

    for (int x = 3; x <= n; x += 2)
    {
        if (lpf[x] == 2) prime.push_back(lpf[x] = x);
        for (int i = 0; i < prime.size() && prime[i] <= lpf[x] && prime[i] * x <= n; ++i)
            lpf[prime[i] * x] = prime[i];
    }
}

/// O(1) check - require sieving
bool isPrime(int x)
{
    if (x < 2 || x >= lpf.size()) return false;
    return lpf[x] == x;
}

const int LIM = 1e6 + 16;

int n, k;
int a[LIM];
int f[LIM];
int main()
{
    /// Nhap nhanh
    ios::sync_with_stdio(NULL);
    cin.tie(NULL);

    /// Input
    cin >> n >> k;
    for (int i = 1; i <= n; ++i)
        cin >> a[i];

    /// Sang nguyen to
    sieve(k);
    
    /// Tien xu li
    f[0] = 0;
    for (int i = 1; i <= n; ++i)
    {
        a[i] = (a[i] <= k && isPrime(a[i])); // Bien mang a[] thanh day nhi phan
        f[i] = f[i - 1] + a[i];              // Dung mang cong don
    }
    int cnt_ones = f[n]; // So gia tri (a[i] = 1)

    /// Tinh ket qua
    int best = cnt_ones; // thuc te chi can d - 1
    for (int l = 1, r = cnt_ones; r <= n; ++l, ++r) // duyet qua cac doan [l, r] do dai (d)
    {
        int cnt_zeros = (f[r] - f[l - 1]);      // so so 0 tren doan
        best = min(best, cnt_ones - cnt_zeros); // cap nhat ket qua
    }

    /// Output
    cout << best;
    return 0;
}
```