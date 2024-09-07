# 单链表——闫总版（详细理解）

**解释**：为什么这个需要单独罗列并写出呢？当然这个有他自己的特性，在我们日常对数据结构的使用过程中，我们完全不需要考虑时间复杂度的问题，但是呢？在算法的使用过程中又不可避免的会用到这个，所以便有了一种只用数组进行实现的数据结构，以下是单链表的实现。

## 题目

实现一个单链表，链表初始为空，支持三种操作：

1. 向链表头插入一个数；
2. 删除第 k 个插入的数后面的一个数；
3. 在第 k 个插入的数后插入一个数。

现在要对该链表进行 M 次操作，进行完所有操作后，从头到尾输出整个链表。

注意:题目中第 k
 个插入的数并不是指当前链表的第 k
 个数。例如操作过程中一共插入了 n
 个数，则按照插入的时间顺序，这 n
 个数依次为：第 1
 个插入的数，第 2
 个插入的数，…第 n
 个插入的数。

**输入格式**

第一行包含整数 **M**
，表示操作次数。

接下来 **M**
 行，每行包含一个操作命令，操作命令可能为以下几种：

1. `H x`，表示向链表头插入一个数 x
。
2. `D k`，表示删除第 k
 个插入的数后面的数（当 k
 为 0
 时，表示删除头结点）。
3. `I k x`，表示在第 k
 个插入的数后面插入一个数 x
（此操作中 k
 均大于 0
）。

**输出格式**

共一行，将整个链表从头到尾输出。

**数据范围**

1≤***M***≤100000

**输入样例：**

```
10
H 9
I 1 1
D 1
D 0
H 6
I 3 6
I 4 5
I 4 5
I 3 4
D 6
```
**输出样例：**
```
6 4 6 5
```

## 分析

**核心理解：** 

- 为什么需要用到这种做法？

> **答：**在解题的过程中，我们需要极快的反应速度，而正常的malloc和new在算法中则会大量拖累时间，所以对此我们采取用多数组对应的方式来完成，从某方面来说，我们使用这种方式会浪费一定的空间，但是用空间换时间不失为一种算法策略。

- 这道题需要注意的点是什么？

> **答：**注意:题目中第 k
 个插入的数并不是指当前链表的第 k
 个数。例如操作过程中一共插入了 n
 个数，则按照插入的时间顺序，这 n
 个数依次为：第 1
 个插入的数，第 2
 个插入的数，…第 n
 个插入的数。
小心不要犯经验主义错误。和第k个节点是不同的！！

**拆分题解：**

不解释
```
#include <bits/stdc++.h>
#define N 100010
using namespace std;
```
`head`标注当前链表头的坐标值，`idx`表示当前的可用空间开辟到哪个位置了\
`e[N]`表示对应的链表坐标上的元素都是什么，`ne[N]`表示开辟的空间的下一个位置的坐标
```
int head,idx,e[N],ne[N];
```
这个初始化函数有一个重要的点:\
head初始化为-1的用意是为了使我无论是在头节点前加值还是后续插入值，都保证有一个结尾部分为-1，可以在对整体进行输出的时候有一个结束位置\
idx初始化为0则使为了让数据在ne[0]的位置开始加插入值
```
void init(){
    head=-1;
    idx=0;
}
```
插头前面
```
void InsertHead(int x){
    e[idx]=x;
    ne[idx]=head;
    head=idx++;
}
```
插中间任意位置
```
void Insert(int k,int x){
    e[idx]= x;
    ne[idx]=ne[k];
    ne[k]=idx;
    idx++;
}
```
判断是否删除头节点，若不删除直接向前跳过即可
```
void Delet(int k){
    if(k!=-1)
        ne[k]=ne[ne[k]];
    else {
        head=ne[head];
    }
}
``
int main(){
    int n;
    cin>>n;
    init();

    while(n--){
        char x;
        cin>>x;
        if(x=='H'){
            int k;
            cin>>k;
            InsertHead(k);
        }
        else if(x=='I'){
            int a,b;
            cin>>a>>b;
            Insert(a-1,b);
        }
        else {
            int k;
            cin>>k;
            Delet(k-1);
        }
    }
    int t = head;
    while(t!=-1){
        cout<<e[t]<<" ";
        t=ne[t];
    }
}
```

## 总结
其实说这个的难点在于什么呢？当然不是在他本身，而是在于这个数组思路的转变，如何用一个一维数组来表示他的前进，如何用另一个数组表示他的值，如何理解不删除数值，仅仅跳过即可。难点仅此而已。

**最终代码**

```
#include <bits/stdc++.h>
#define N 100010
using namespace std;

int head,idx,e[N],ne[N];

void init(){
    head=-1;
    idx=0;
}

void InsertHead(int x){
    e[idx]=x;
    ne[idx]=head;
    head=idx++;
}

void Insert(int k,int x){
    e[idx]= x;
    ne[idx]=ne[k];
    ne[k]=idx;
    idx++;
}

void Delet(int k){
    if(k!=-1)
        ne[k]=ne[ne[k]];
    else {
        head=ne[head];
    }
}

int main(){
    int n;
    cin>>n;
    init();

    while(n--){
        char x;
        cin>>x;
        if(x=='H'){
            int k;
            cin>>k;
            InsertHead(k);
        }
        else if(x=='I'){
            int a,b;
            cin>>a>>b;
            Insert(a-1,b);
        }
        else {
            int k;
            cin>>k;
            Delet(k-1);
        }
    }
    int t = head;
    while(t!=-1){
        cout<<e[t]<<" ";
        t=ne[t];
    }
}
```