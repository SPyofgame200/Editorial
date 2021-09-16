Olympic Sinh Viên 2020 - Chuyên tin - VCA
===

[URL](https://oj.vnoi.info/problem/olp_ct20_vca)

-----

### Hướng dẫn

Với mỗi vị trí $[L]$ tìm vị trí $[R]$ nhỏ nhất thỏa mãn số lượng kí tự '**A**', '**V**' và '**C**' trong đoạn $[L, R]$ đều không ít hơn $k$

Vì lúc này $[L, R]$ là một đoạn cố định tương tự như mình xóa $[1, L)$ và $(R, N]$ mà không mất chi phí, nên giờ mình sẽ xóa phần bên trong, sao cho số lượng '**A**', '**V**' và '**C**' đều có đúng $k$.

Gọi $C(L, R, X)$ là số lượng kí tự có giá trị $X$ trên đoạn $[L, R]$. Chi phí sửa đổi của đoạn $[L, R]$ là $(C(L, R,$ '**A**' $) + C(L, R,$ '**C**'$) + C(L, R,$ '**V**'$) - 3k)$. 

Vậy bài này có độ phức tạp là $O(n)$ thời gian và bộ nhớ

- Mình có thể tìm $R$ bằng hai con trỏ thay cho chặt nhị phân
- Mình có thể sử dụng mảng cộng dồn hoặc một số cấu trúc dữ liệu để $O(n)$ tiền xử lí và $O(1)$ truy vấn.

-----

### Code

> **Time:** $O(n)$
> **Space:** $O(n)$ 
> **Algo:** Two Pointers

```cpp=
#inlude <iostream>

using namespace std;

int cnt[256];
int main()
{
    /// Input
    int k;
    string s;
    cin >> k >> s;
    
    /// Calculate
    int res = +INF;
    cnt['A'] = cnt['V'] = cnt['C'] = 0;
    for (int l = 0, r = -1; l < s.size(); --cnt[s[l++]])
    {
        while (r + 1 < s.size() && (cnt['A'] < k || cnt['V'] < k || cnt['C'] < k)) ++cnt[s[++r]];
        if (cnt['A'] < k || cnt['V'] < k || cnt['C'] < k) break;

        int t = 0;
        t += cnt['A'] - k;
        t += cnt['V'] - k;
        t += cnt['C'] - k;
        minimize(res, t);
    }
    
    /// Output
    cout << res;
    return 0;
}
```