# 区间合并

## 题目：
给定 n
 个区间 [li,ri]
，要求合并所有有交集的区间。

注意如果在端点处相交，也算有交集。

输出合并完成后的区间个数。

例如：[1,3]
 和 [2,6]
 可以合并为一个区间 [1,6]
。

**输入格式**

第一行包含整数 n
。

接下来 n
 行，每行包含两个整数 l
 和 r
。

**输出格式**

共一行，包含一个整数，表示合并区间完成后的区间个数。

**数据范围**

*1≤n≤100000*
,
*−109≤li≤ri≤109*

**输入样例：**

```
5
1 2
2 4
5 6
7 8
7 9
```
**输出样例：**
```
3
```

## 分析

**核心理解：** 

- 思路从何而来？

> **答：** 本题考察的是数据的归并能力，如何一遍处理数据同时得出最正确的结果，则该题可以先将区间排序，后即可轻松看出规律

## 总结

本题简单，不过多解释。

代码如下：

```
#include <bits/stdc++.h>
#define N 100010
#define PII pair<int, int>
using namespace std;
vector<PII> adds;
int main()
{
    int n;
    cin >> n;
    for (int i = 0; i < n; i++)
    {
        int l, r;
        cin >> l >> r;
        adds.push_back({l, r});
    }
    sort(adds.begin(), adds.end());
    int ans = 0, maxn = -2e9;
    for (auto it : adds)
    {
        if (it.first > maxn)
        {
            ans++;
            maxn = it.second;
        }
        else
        {
            maxn = max(maxn, it.second);
        }
    }
    cout << ans << endl;
}
```