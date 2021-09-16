Free Contest Testing Round 20 - XORTABLE
===

[Link](https://oj.vnoi.info/problem/fct020_xortable)

-----

### Hướng dẫn

Có $b_{1,j} = a_1 ⊕ a_j$

Mà $b_{i,j}$

$= a_i ⊕ a_j$

$= a_i ⊕ a_j ⊕ 0$

$= a_i ⊕ a_j ⊕ a_1 ⊕ a_1$

$= (a_1 ⊕ a_i) ⊕ (a_1 ⊕ a_j)$

$= b_{1,i} ⊕ b_{1, j}$

Vậy ta dù không cần tính mảng $a_i$ cũng có thể tìm toàn bộ mảng $b_{i, j}$

-----

### Code

> **Time:** $O(n^2)$
> **Space:** $O(n^2)$ 
> **Algo:** Implementation, Constructive

```cpp=
#include <iostream>

using namespace std;

int b1[111];
int main()
{
    int n;
    cin >> n;
    
    for (int i = 1; i <= n; ++i)
        cin >> b1[i];
    
    for (int i = 1; i <= n; ++i)
    {
        for (int j = 1; j <= n; ++j)
        {
            cout << (b1[i] ^ b1[j]) << " ";
        }
        cout << "\n";
    }
    
    return 0;
}
```