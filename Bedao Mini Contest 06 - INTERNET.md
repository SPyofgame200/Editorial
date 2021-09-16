Bedao Mini Contest 06 - INTERNET
===

[Link](https://oj.vnoi.info/problem/bedao_m06_internet)

-----

### Trường hợp $s = 1$

Ta chỉ cần in số $0$ vì số đó là số hệ tứ phân đối xứng nhỏ nhất

-----

### Trường hợp $s > 1$:

Vì số không được có số $0$ vô nghĩa nên ta sẽ chọn chữ số ở đầu là $1, 2$ hoặc $3$ nếu có đủ và ưu tiên số nhỏ hơn, vì số đối xứng nên chữ số ở cuối cũng phải là chữ số ở đầu. Lưu ý rằng cần ít nhất $2$ chữ số để đủ. Nếu không tồn tại thì in **"Bedao!"** và dừng chương trình

Sau khi điền ở đầu, ta điền từ ở giữa ra, ta ưu tiền điền các chữ số lớn nhất vì lúc này các số càng gần về phía trái (tức hệ số lớn hơn) sẽ có chữ số nhỏ hơn. Lưu ý những phần chưa được điền số ta gán hết bằng $0$ và không điền ra tới ngoài biên

Nếu như $s$ lẻ ta sẽ chon chữ số lớn nhất mà số lượng của nó còn lại lẻ lần vì những vị trí khác phải điền theo cặp. Lưu ý nếu ta không tìm được chữ số hợp lệ thì ta điền số $0$

Nếu sau quá trình điền vẫn còn dư $A$ hoặc $B$ hoặc $C$ không tồn tại thì in **"Bedao!"** và dừng chương trình

Ngược lại xuất ta cần tính giá trị của số vừa được xây in modulo. Để làm điều đó, ta khởi tạo $v = 0$ và duyệt qua từng chữ số $x$ hệ tứ phân theo trái qua phải, gán $v := (v * 4 + x)\mod\ 727355608$

Vậy độ phức tạp bài này là $O(s)$

-----

### Code

> **Time:** $O(s)$
> **Space:** $O(s)$ 
> **Algo:** Medium Implementation, Constructive, Base-4

```cpp=
#include <iostream>
#include <vector>

using namespace std;

const int MOD = 727355608;
int main()
{
    /// De nhap nhanh hon
    ios::sync_with_stdio(NULL);
    cin.tie(NULL);

    /// Input
    int s, a, b, c;
    cin >> s >> a >> b >> c;
    
    /// De code gon hon
    int cnt[4];
    cnt[1] = a;
    cnt[2] = b;
    cnt[3] = c;

    /// TH dac biet
    if (s == 1)
    {
        cout << 0;
        return 0;
    }
    
    /// So he tu phan
    vector<int> x(s, 0);
    for (int v = 1; v <= 4; ++v)
    {
        /// Neu du so luong 
        if (cnt[v] >= 2)
        {
            cnt[v] -= 2;
            x.front() = v; 
            x.back() = v;
            break;
        }
    }
    
    /// Khong tim duoc cach chon so dau tien thoa man
    if (x.front() == 0) 
    {
        cout << "Bedao!";
        return 0;
    }
    
    /// Xay tung chu so tu giua ra
    for (int l = (s - 1) / 2, r = s / 2; 0 < l && r < s; --l, ++r)
    {
        if (l == r)
        {
            for (int v = 3; v >= 1; --v)
            {
                if (cnt[v] % 2 == 1) /// Chi uu tien chon so bi le ra
                {
                    --cnt[v];
                    x[l] = v;
                    break;
                }
            }

            continue;
        }
        
        /// Uu tien xay tu giua ra ngoai bien nen se uu tien gia tri cang to cang tot
        for (int v = 3; v >= 1; --v)
        {
            if (cnt[v] >= 2)
            {
                cnt[v] -= 2;
                x[l] = x[r] = v;
                break;
            }
        }
    }   

    /// Khong xay dung duoc so thoa man
    if (cnt[1] || cnt[2] || cnt[3])
    {
        cout << "Bedao!";
        return 0;
    }

    /// Tinh ket qua modulo
    int res = 0;
    for (int v : x) res = (4LL * res | v) % MOD;
    
    /// Output
    cout << res;
    return 0;
}
```