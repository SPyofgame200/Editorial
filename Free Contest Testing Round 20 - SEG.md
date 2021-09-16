Free Contest Testing Round 20 - SEG
=====

[Link](https://oj.vnoi.info/problem/fct020_seg)

-----

Contributer: [mr_sg](https://codeforces.com/profile/giangcbg)

-----

### Xét bài toán con

Xét một dãy $X =$ "**<<<...<<<>>>...>>>**"

Gọi $L$ là số lần xuất hiện "**<**"

Gọi $R$ là số lần xuất hiện "**>**"

Thì dãy nguyên không âm có tổng nhỏ nhất tạo được là

$0 < 1 < 2 < \dots < L - 1 < max(L, R) > R - 1 > \dots > 2 > 1 > 0$

Với tổng của dãy này là 

$V(X) = (0 + 1 + 2 + \dots + L - 1) + max(L, R) + (R - 1 + \dots + 2 + 1 + 0) = \frac{L(L-1)}{2} + max(L, R) + \frac{R(R-1)}{2}$

------

### Trở về bài toán chính 

Vậy với một xâu $S$ bất kì, ta phải tách về các xâu con để tính

Để thuận tiện cho việc tính toán và tránh phải xử lí cho trường hợp không có "**<**" hay "**>**". Ta sẽ nén mảng thành một tập $A$ chứa các cặp "**{char, int}**" biểu thị có **int** số lượng kí tự **char** (**char** là **<** hoặc **>**).

Vậy nếu bên trái $S$ không có **<** có thể chèn vào đầu $A$ {"**<**", $0$}

Và nếu bên phải $S$ không có **>** có thể chèn vào cuối $A$ {"**>**", $0$}

Khi này chỉ cần tính kết quả giữa các cặp "**<>**" kề nhau

------

### Code

> **Time:** $O(|s|)$
> **Space:** $O(|s|)$ 
> **Algo:** Implementation, String
> 
```cpp=
#include <iostream>
#include <vector>

using namespace std;

/// 0 + 1 + 2 + ... + n - 1
long long natural_sum(int n)
{
    return 1LL * n * (n - 1) / 2;
}

int main()
{
    /// Input
    string s;
    cin >> s;

    /// String compression
    vector<pair<char, int> > T;
    T.reserve(s.size());
    
    if (s.front() != '<') T.push_back(make_pair('<', 0));
    for (char c : s)
    {
        if (T.empty() || T.back().first != c)
            T.push_back(make_pair(c, 1));
        else 
            T.back().second++;
    }
    if (s.back() != '>') T.push_back(make_pair('>', 0));

    /// Calculating
    long long res = 0;
    for (int i = 1; i < T.size(); i += 2)
    {
        /// Peak of sequence 0 < 1 < ... < T[i-1]-1 < peak > T[i-0]-1 > ... > 1 > 0
        int peak = max(T[i - 1].second, T[i - 0].second); 
        res += natural_sum(T[i - 1].second); /// Sum of Left  sequence (not include peak) 0 < 1 < ... < T[i-1] - 1 < [peak]
        res += natural_sum(T[i - 0].second); /// Sum of Right sequence (not include peak) [peak] > T[i-0] - 1 > ... > 1 > 0
        res += peak;
    }

    /// Output
    cout << res;
    return 0;
}
```