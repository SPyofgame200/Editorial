Olympic Sinh Viên 2020 - Không chuyên - Chăn bò
===

[Link](https://oj.vnoi.info/problem/olp_kc20_cows)

-----

### Hướng dẫn

Tổng số hiệu của đàn bò ban đầu: 

$T = 1 + 2 + \dots + (n-1) + n$

$T = n + (n - 1) + \dots + 2 + 1$

$\Leftrightarrow 2T = (1 + n) + (2 + (n-1)) + \dots + ((n - 1) + 2) + (n + 1) = (n + 1) \times n$

$\Leftrightarrow T = \frac{(n + 1) \times n}{2}$

Gọi số hiệu con bò cần tìm là $x$ có $S = T - x \Leftrightarrow x = T - S$ 

-----

### Code 

> **Time:** $O(1)$
> **Space:** $O(1)$ 
> **Algo:** Math, Implementation

```cpp=
#include <iostream>

using namespace std;

int main()
{
    long long n, S;
    cin >> n >> S;
    cout << n * (n + 1) / 2 - S;
    return 0;
}
```