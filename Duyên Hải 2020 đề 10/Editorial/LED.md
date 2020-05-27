# Yêu cầu đề:
>    **Query1()**: Đếm số vạch LED cần để hiển thị **N**
 
>    **Query2()**: Đếm số lượng số **X** > **N** thỏa các vạch LED **N** bật thì vạch của **X** cũng bật

<details> <summary> Ta chia làm 2 TH để xử lí </summary>
    
```cpp
int main()
{
    if (readInt() == 1)
        solve1();
    else
        solve2();
    return 0;
}
```
</details>

# Với Query1()
> Ta chỉ việc đếm số vạch LED bật ở mỗi số và lưu vào mảng **cntled[0..9]**

> Duyệt qua chữ số của **N** cộng **cntled[digit_thứ_i]** vào kết quả **result**

<details> <summary> Implementation 1 </summary>

```cpp
/// Precalculate numbers of set-led for each number 0 -> 9
vi cntled = {6, 2, 5, 5, 4, 5, 6, 3, 7, 6};
void solve1()
{
    long long n = readLong();
    
    /// Result is sumation of set-led in every digits
    int res = 0; 
    do res += cntled[n % 10]; while (n /= 10);
    cout << res;
}
```
</details>


<details> <summary> Implementation 2 </summary>
    
```cpp
/// Precalculate numbers of set-led for each number 0 -> 9
vi cntled = {6, 2, 5, 5, 4, 5, 6, 3, 7, 6};
void solve1()
{
    /// Represent number as string
    string s = readString();
 
    /// Result is sumation of all set-led in number
    int res = 0;
    for (char c : s)
        res += cntled[c - '0'];
 
    cout << res;
}
```
</details>

# Với Query2()

> 

<details> <summary> _ </summary>
    
_
</details>
