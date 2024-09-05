# 区间和

## 题目：
假定有一个无限长的数轴，数轴上每个坐标上的数都是 0

现在，我们首先进行 n
 次操作，每次操作将某一位置 x
 上的数加 c
。

接下来，进行 m
 次询问，每个询问包含两个整数 l
 和 r
，你需要求出在区间 [l,r]
 之间的所有数的和。

**输入格式**

第一行包含两个整数 n
 和 m
。

接下来 n
 行，每行包含两个整数 x
 和 c
。

再接下来 m
 行，每行包含两个整数 l
 和 r
。

输出格式
共 m
 行，每行输出一个询问中所求的区间内数字和。

**输出格式**

共 m
 行，每行输出一个询问中所求的区间内数字和。

**数据范围**

*−109≤x≤109*
,
*1≤n,m≤105*
,
*−109≤l≤r≤109*
,
*−10000≤c≤10000*

**输入样例：**

```
3 3
1 2
3 6
7 5
1 3
4 6
7 8
```
**输出样例：**
```
8
0
5
```

## 分析

**核心理解：** 

- 为什么需要离散化？

> **答：** 本题的数据范围大，但是整体的数据并不多，如果用常规做法也就是暴力循环，一是栈上数组无法开到对应的大小，二是若循环则必然超时，所以在此使用将元素与坐标对应，坐标化元素，元素化数组，用连续的数组来和对应的数值进行处理，最后使用前缀和即可快速完成。

- 离散化的核心是什么？

> **答：** 将坐标化小，散值化密，TLE化单循环，尽可能降低空间开销

**拆分题解：**

头文件不解释

为什么这里开到300010呢？
因为本题需要储存全部的坐标，添加元素的坐标`n`、查询的左右坐标`2m`，每个元素
的大小为1e+5，所以最大共3e+5，防越界+10。
```
#include<bits/stdc++.h>
#define N 300010
using namespace std;
typedef pair<int,int> PII;
```
这里的a是处理后的数组，s是前缀和快速处理查询，alls储存全部下标结果
add和query要使用不止一次，所以先行记录
```
int a[N],s[N];

vector<int> alls;//无论是查询，还是位置元素全部放进来

vector<PII> add,query;
```
以最快速的方式在下标堆里找到位置，方便直接赋值给处理后的数组赋值

`注意！` 为什么在这里需要+1呢？原因非常简单：我希望的我的目标位置从1开始
，但是我的alls本身是从0开始的，所以如果要让整体偏移，便在返回值+1即可全部移
动至下一位。（为什么需要从1开始可以从前缀和的模板来想）
```
int find(int x){
    int l=0,r=alls.size()-1;
    while(l<r){
        int mid = l+r>>1;
        if(alls[mid]>=x)    r=mid;
        else  l=mid+1;        
    }
    return l+1;
}
```
不解释
```
int main(){
    int  n,m;
    cin>>n>>m;
    //添加元素
    for(int i=0;i<n;i++){
        int a,b;
        cin>>a>>b;
        add.push_back({a,b});
        alls.push_back(a);
    }
    //查询
    for(int i=0;i<m;i++){
        int a,b;
        cin>>a>>b;
        query.push_back({a,b});
        alls.push_back(a);
        alls.push_back(b);
    }
```
非常常用！`alls.erase(unique(alls.begin(),alls.end()),alls.end());`

排序是为了后面的去重！
```
    sort(alls.begin(),alls.end());
    alls.erase(unique(alls.begin(),alls.end()),alls.end());
```
构成一个和`vector<int> alls`相同的序列模板，但只给属于``add``的加大小
```
    for(auto it : add){
        a[find(it.first)]+=it.second;
    }
```
构成前缀和不解释，前缀和`s`也从1开始
```
    for(int i=1;i<=alls.size();i++){
        s[i]=s[i-1]+a[i];
    }
```
输出查询位置的前缀和即可
```
    for(auto it : query){
        cout<<s[find(it.second)]-s[find(it.first)-1]<<endl;
    }
}
```

## 总结

个人认为，离散化的思想非常厉害，将大模型化成小模型，将散的收束，并进行完整的归纳整理，适合反复品鉴

最后代码如下：
```
#include<bits/stdc++.h>
#define N 300010
using namespace std;
typedef pair<int,int> PII;

int a[N],s[N];

vector<int> alls;//无论是查询，还是位置元素全部放进来

vector<PII> add,query;

int find(int x){
    int l=0,r=alls.size()-1;
    while(l<r){
        int mid = l+r>>1;
        if(alls[mid]>=x)    r=mid;
        else  l=mid+1;        
    }
    return l+1;
}

int main(){
    int  n,m;
    cin>>n>>m;
    //添加元素
    for(int i=0;i<n;i++){
        int a,b;
        cin>>a>>b;
        add.push_back({a,b});
        alls.push_back(a);
    }
    //查询
    for(int i=0;i<m;i++){
        int a,b;
        cin>>a>>b;
        query.push_back({a,b});
        alls.push_back(a);
        alls.push_back(b);
    }
    sort(alls.begin(),alls.end());
    alls.erase(unique(alls.begin(),alls.end()),alls.end());
    for(auto it : add){
        a[find(it.first)]+=it.second;
    }
    for(int i=1;i<=alls.size();i++){
        s[i]=s[i-1]+a[i];
    }
    for(auto it : query){
        cout<<s[find(it.second)]-s[find(it.first)-1]<<endl;
    }
}
```