Bedao Mini Contest 06 - HARD
===

[Link](https://oj.vnoi.info/problem/bedao_m06_hard)

-----

### Giải truy vấn 1:

Nếu truy vấn 1 không đủ nhanh thì TLE trước cả truy vấn 2 nên mình cần tìm một cấu trúc dữ liệu đủ nhanh

Ta có thể sử dụng các cấu trúc dữ liệu cây để tìm, chèn, xóa trong $O(\log\ n)$ hoặc nhanh hơn, ví dụ như trong C++ có std::set, std::map

-----

### Giải truy vấn 2:

Nếu như $x = 0$ thì test đề không có, mình cũng chẳng biết nó in ra $0$ hay $-1$ :(

Nếu như tập $A$ rỗng, hoặc là tập $A$ không bao gồm số $1$, thì không thể phủ được đoạn con $[1, 1]$ trong đoạn $[1, x]$ nên ta xuất $-1$ và thực hiện truy vấn tiếp theo

Ngược lại ta luôn tồn tại các phủ xuất phát từ đoạn con $[1, 1]$. Vấn đề chúng ta bây giờ là làm sao để biết bao nhiêu đoạn nào đã bị phủ, và nên sử dụng thêm số nào ?

**Nhận xét 1:** Nếu đoạn $[1, k]$ đã bị phủ, thì nếu cho thêm giá trị $v > k + 1$ thì sẽ luôn thiếu mất đoạn $[k + 1, k + 1]$, nhưng nếu cho thêm giá trị $v \leq k + 1$ thỉ đoạn bị phủ hiện tại sẽ là $[1, k + v]$ vì luôn tồn tại số $y$ thuộc đoạn $[1, k]$ để phủ thêm lên đoạn $[k + 1, k + v]$. Vì đoạn càng ngày càng được nới rộng dần sang phía $[x]$, nên giá trị $v$ ta chọn sẽ càng lớn dần

**Nhận xét 2:** Nếu cứ phải xét một nhóm k đoạn $[l_1, r_1], [l_2, r_2], \dots, [l_k, r_k]$ ($1 \leq l_1 \leq r_1 < l_2 \leq r_2 < \dots < l_k \leq r_k)$ đã bị phủ thì sẽ phải chèn thêm các số vào giữa để lấp lại các đoạn bị sót. Nhưng như thế cũng có nghĩa là tồn tại một cách sắp xếp thứ tự chèn các số để sao cho các đoạn bị phủ sẽ phủ lần lượt $[1, r_1] \rightarrow [1, r_2] \rightarrow \dots \rightarrow [1, r_k]$, mà từ nhận xét $1$, ta cũng chỉ cần chọn các số theo thứ tự từ nhỏ đến lớn mà thôi. Nên cách này vừa cài khó, vừa khó tính toán và tối ưu hơn cách làm theo nhận xét $1$, hoặc thậm chí không tồn tại thuật toán để giải

Vì mình muốn phủ bằng càng ít số càng tốt và không được chọn số lớn hơn $k + 1$ hay không có trong tập. Nên mình sẽ chọn số $v_0 = \underset{u \in A}{max}(u | u \leq k + 1)$. Mình có thể chặt nhị phân trên cây trong $O(\log^2(n))$ hoặc tìm kiếm trực tiếp trên cây trong $O(\log\ n)$

Nhưng giờ mình không thể cứ thêm từng số được, mà nhận thấy rằng để tìm được số $v$ tiếp theo thỏa mãn, thì đó sẽ là $v_1 = \underset{u \in A}{max}(u | u \leq k + v + 1)$, và cho đến lúc đó mình chỉ có thêm số $v_0$.

**Nhận xét 3:** Tồn tại $p$ là vị trí mà khi đoạn bị phủ chứa cả $[1, p]$ mình sẽ thấy $v_0$ sẽ không còn tối ưu để thêm nữa. Rõ ràng $p = min(v_1, x) \geq k$. Cho tới lúc đó thì mình cần thêm $v_0$ với số lần là $\lceil \frac{p - k}{v_0} \rceil = \lfloor \frac{p - k + v_0 - 1}{v_0} \rfloor$

------

### Độ phức tạp

Mỗi bước, có vẻ như $v_1$ có thể thay thế $v_0$ với $O(|A|)$ lần trong trường hợp xấu nhất. Nhưng điều này chỉ khả thi khi giới hạn của các số $v \in A$ đủ lớn.

Sau mỗi bước đưa đoạn $[1, k]$ nới rộng lên trên đoạn $[1, p]$ thì độ dài tăng ít nhất $2$ lần trong 1 bước, cụ thể là $1 \leq \frac{k}{2} \leq v_0 \leq k + 1 \leq 2 \times v_0 \leq v_1$.

Có $q$ truy vấn tất cả. Số bước phủ dãy là $O(min(|A|, \log_2(max(x \in A))))$ . Mà mỗi bước, mình cần tìm kiếm trên cây mất $O(\log_2\ |A|)$. 

Vậy ta có độ phức tạp là $O(q \times \log_2 n \times min(n, \log_2(max(A))))$ với $n = |A|$

------

### Code

> For $n = |A|$
> **Time:** $O(q \times \log_2 n \times min(n, \log_2(max(A))))$
> **Space:** $O(q + n)$ 
> **Algo:** Math, Binary Search, Data Structure ( Tree ), Online Query
> 
```cpp=
#include <iostream>
#include <cstdio>
#include <set>

using namespace std;

typedef long long ll;

int main()
{
    /// De nhap nhanh
    ios::sync_with_stdio(NULL);
    cin.tie(NULL);
    
    /// tap A hien tai
    set<ll> A;
    A.insert(2e18); /// de tien tinh toan

    /// Input
    int q;
    cin >> q;

    /// Duyet qua cac truy van
    while (q-->0)
    {
        /// Phep truy van
        int operation;
        cin >> operation;

        if (operation == 1) /// Tuong tac voi gia tri
        {
            ll val;
            cin >> val;

            if (A.count(val))  /// neu so da ton tai
                A.erase(val);  /// -> xoa
            else               /// nguoc lai chua ton tai
                A.insert(val); /// -> them
        }
        else if (operation == 2) /// Yeu cau phu doan [1, pos]
        {
            ll pos;
            cin >> pos;

            if (A.size() <= 1 || !A.count(1)) /// TH 1: Khong the phu duoc doan [1, 1]
            {
                cout << -1 << "\n";
            } 
            else /// TH 2: Luon phu duoc doan [1, x]
            {
                ll res = 1; /// so phan tu trong tap hien tai
                ll cur = 1; /// doan da phu duoc toi hien tai [1, cur]
                while (cur < pos) /// Chay toi khi nao phu duoc doan [1, pos]
                {
                    set<ll>::iterator it = A.upper_bound(cur + 1);
                    ll nxt = min(pos, *it - 1); /// can phu qua doan [1, v_1]
                    ll dis = *(--it);           /// gia tri v_0 o hien tai
                    ll cnt = (nxt - cur + dis - 1) / dis; /// so buoc can thuc hien v_0
                    cur += cnt * dis; /// di chuyen (cnt) buoc, moi buoc phu them (dis) don vi
                    res += cnt;       /// them (cnt) phan tu co gia tri (v_0)
                }

                cout << res << "\n"; /// xuat ket qua
            }
        }
    }

    return 0;
}
```