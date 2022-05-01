[题面](https://ac.nowcoder.com/acm/contest/33550/E)

### 题意

- 给定长度为  <img src="https://www.zhihu.com/equation?tex=n" alt="n" class="ee_img tr_noresize" eeimg="1">  的 01 序列每一位为 1 的概率  <img src="https://www.zhihu.com/equation?tex=p_i" alt="p_i" class="ee_img tr_noresize" eeimg="1"> ，给定  <img src="https://www.zhihu.com/equation?tex=m" alt="m" class="ee_img tr_noresize" eeimg="1"> ，长度为  <img src="https://www.zhihu.com/equation?tex=x" alt="x" class="ee_img tr_noresize" eeimg="1">  的连续一段对答案的贡献是  <img src="https://www.zhihu.com/equation?tex=x^m" alt="x^m" class="ee_img tr_noresize" eeimg="1"> ，求答案在模意义下的期望。
-  <img src="https://www.zhihu.com/equation?tex=1\le n\le 1000, 1\le m\le 1000, 0\le p[i]\le 100" alt="1\le n\le 1000, 1\le m\le 1000, 0\le p[i]\le 100" class="ee_img tr_noresize" eeimg="1"> 

### 思路

1. 暴力  <img src="https://www.zhihu.com/equation?tex=O(n^2)" alt="O(n^2)" class="ee_img tr_noresize" eeimg="1">  求所有连续区间的概率，注意一位为 1 的概率 0 会影响左右两段，注意讨论，求前缀积的时候跳过这样的位。区间连续 1 的概率即为  <img src="https://www.zhihu.com/equation?tex=s_i /s_{j-1}" alt="s_i /s_{j-1}" class="ee_img tr_noresize" eeimg="1"> ，如果  <img src="https://www.zhihu.com/equation?tex=s_{j-1}" alt="s_{j-1}" class="ee_img tr_noresize" eeimg="1">  为 0，说明  <img src="https://www.zhihu.com/equation?tex=s_i" alt="s_i" class="ee_img tr_noresize" eeimg="1">  从第  <img src="https://www.zhihu.com/equation?tex=j" alt="j" class="ee_img tr_noresize" eeimg="1">  位开始前缀积，此时不需要除  <img src="https://www.zhihu.com/equation?tex=s_{j-1}" alt="s_{j-1}" class="ee_img tr_noresize" eeimg="1"> ，否则要除。

2. 第  <img src="https://www.zhihu.com/equation?tex=i-1,j+1" alt="i-1,j+1" class="ee_img tr_noresize" eeimg="1">  位必须要为 0，才是 第  <img src="https://www.zhihu.com/equation?tex=i" alt="i" class="ee_img tr_noresize" eeimg="1">  到  <img src="https://www.zhihu.com/equation?tex=j" alt="j" class="ee_img tr_noresize" eeimg="1">  为连续 1 段的贡献。要乘上这两位为 0 的概率  <img src="https://www.zhihu.com/equation?tex=(1-a_{i-1}),(1-a_{j+1})" alt="(1-a_{i-1}),(1-a_{j+1})" class="ee_img tr_noresize" eeimg="1"> 。如果  <img src="https://www.zhihu.com/equation?tex=i" alt="i" class="ee_img tr_noresize" eeimg="1">  是第一位，前面一位不需要有零，不要乘  <img src="https://www.zhihu.com/equation?tex=(1-a_{i-1})" alt="(1-a_{i-1})" class="ee_img tr_noresize" eeimg="1"> 。同理，如果  <img src="https://www.zhihu.com/equation?tex=i" alt="i" class="ee_img tr_noresize" eeimg="1">  是最后一位，后面一位不需要有零，不要乘  <img src="https://www.zhihu.com/equation?tex=(1-a_{j+1})" alt="(1-a_{j+1})" class="ee_img tr_noresize" eeimg="1"> 。

3. 注意细节：

	```cpp
	if(a[j] == 0) break;
	```

	不然，i~j 的段中间可能会有 0，无法保证 i~j 都是 1

4. mint 为自动取模的整数类：https://paste.ubuntu.com/p/KyGhdcSc6j/

### 代码

```cpp
int n, m;
mint ans, a[N], s[N];

int main() {
    cin >> n >> m;
    a[0] = s[0] = 1;
    rep(i, 1, n) cin >> a[i], a[i] /= 100, s[i] = a[i];
    rep(i, 1, n) {
        if(a[i] == 0 or a[i - 1] == 0) continue;
        s[i] *= s[i - 1];
    }
    rep(i, 1, n) {
        if(a[i] == 0) continue;
        rep(j, i, n) {
            if(a[j] == 0) break;
            mint dx = qpow(j - i + 1, m) * s[j];
            if(a[i - 1] != 0) {
                dx /= s[i - 1];
                if(i != 1) dx *= (1 - a[i - 1]);
            }
            if(a[j + 1] != 0 and j != n) dx *= (1 - a[j + 1]);
            ans += dx;
        }
    }
    cout << ans << endl;
    return 0;
}
```