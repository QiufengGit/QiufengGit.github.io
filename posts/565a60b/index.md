# 蓝桥杯笔记


&lt;!--more--&gt;

蓝桥杯算法基础课第一章、第二章笔记（部分内容参考自qly同学）

# 算法基础课

## 排序

思路：分治

1. 分为子序列
2. 更分子序列
3. 子序列处理合并

### 快排

1. 取中间值  分割区间 排序
2. 再取中间值 再分割子区间 在排序
3. 区间合并

~~~c&#43;&#43;
void quick_sort(int q[],int l, int r){
    if(l&gt;=r) return;
    int i=l-1,j=r&#43;1; //初始位置
    
    int x=q[l&#43;r&gt;&gt;1]; //中间值位置 选择j时不变，选择i时l&#43;r&#43;1 （边界问题）
    
    while(i&lt;j){ //逐次处理
        do i&#43;&#43;; while(q[i]&lt;x); //注意起点位置
        do j--;while(q[j]&gt;x);
        if(i&lt;j) swap(q[i],q[j]); //单次交换
    }
    
    quick_sort(q,l,j); //递归处理子区间
    quick_sort(q,j&#43;1,r);
}
~~~

&gt;注：
&gt;
&gt;1. while(i&lt;j)不能写成 i&lt;=j,这样会多做一次循环，导致跳出循环时i&gt;j。跳出循环时i应该和j相等。
&gt;2. 注意i、j模板与初始分界点x的关系。
&gt;3. 如果初始分界点x取左端点或者右端点，在所有数都一样的情况下，时间复杂度会退化到 O($n^2$)，原为$n*log_2(n)$



### 归并

思路：分治

1. 找出分界点，分界点取数组的中间位置（与快速排序不同，分界点取中间位置的数值）。
2. 分界后对两边做递归排序。
3. 归并，合二为一。（从小到大则是取较小值放到tmp中，从大到小则是取较大值放到tmp中）
4. 时间复杂度：$n*log_2(n)$。（n对2取对数）

~~~c&#43;&#43;
void merge_sort(int q[],int l,int r){
   if(l&gt;=r) return;
   int mid=l&#43;r&gt;&gt;1; //边界点确定
    merge_sort(q,l,mid); //递归
    merge_sort(q,mid&#43;1,r);
    int k=0,i=l,j=mid&#43;1; //两边初值
    while(i&lt;=mid&amp;&amp;j&lt;=r){
        if(q[i]&lt;q[j]) //取小值赋值
            tmp[k&#43;&#43;]=q[i&#43;&#43;];
        else
            tmp[k&#43;&#43;]=q[j&#43;&#43;];
    }
    while(i&lt;=mid) //取剩余
        tmp[k&#43;&#43;]=q[i&#43;&#43;];
    while(j&lt;=r)
        tmp[k&#43;&#43;]=q[j&#43;&#43;];
    for (i=l,j=0;i&lt;=r;i&#43;&#43;,j&#43;&#43;) q[i]=tmp[j]; //排序后赋值 从l到r
}
~~~

&gt;注：
&gt;
&gt;1.  while(i&lt;=mid&amp;&amp;j&lt;=r)  两边循环到底
&gt;2.  注意i、j模板与初始分界点mid的关系。
&gt;3.  时间复杂度稳定为$n*log_2(n)$
&gt;4.  与快排不同，需要空间存储，故数据量小时适合归并



## 二分

思路：分治

1. 二分出分界点
2. 数据含一定规律：从符合规则到不符合规则
3. 整数二分存在边界问题（$=$ 号位置）

~~~c&#43;&#43;
bool check(int x) {/* ... */} // 检查x是否满足某种性质

// 区间[l, r]被划分成[l, mid]和[mid &#43; 1, r]时使用：
int bsearch_1(int l, int r)
{
    while (l &lt; r)
    {
        int mid = l &#43; r &gt;&gt; 1;
        if (check(mid)) r = mid;    // check()判断mid是否满足性质
        else l = mid &#43; 1;
    }
    return l;
}
// 区间[l, r]被划分成[l, mid - 1]和[mid, r]时使用：
int bsearch_2(int l, int r)
{
    while (l &lt; r)
    {
        int mid = l &#43; r &#43; 1 &gt;&gt; 1;
        if (check(mid)) l = mid;
        else r = mid - 1;
    }
    return l;
}
~~~

~~~c&#43;&#43;
bool check(double x) {/* ... */} // 检查x是否满足某种性质

double bsearch_3(double l, double r)
{
    const double eps = 1e-6;   // eps 表示精度，取决于题目对精度的要求
    while (r - l &gt; eps)
    {
        double mid = (l &#43; r) / 2;
        if (check(mid)) r = mid;
        else l = mid;
    }
    return l;
}
~~~



&gt;左右边界：
&gt;
&gt;看mid点位置
&gt;
&gt;不满足-满足  左边界
&gt;
&gt;满足-不满足  右边界



## 高精度计算

思路：模仿手算

- 加法 后一位补
- 减法 后一位借
- 乘法 后一位补
- 除法 后一位借

### 加法

```c&#43;&#43;
// C = A &#43; B, A &gt;= 0, B &gt;= 0
vector&lt;int&gt; add(vector&lt;int&gt; &amp;A, vector&lt;int&gt; &amp;B)
{
    if (A.size() &lt; B.size()) return add(B, A);

    vector&lt;int&gt; C;
    int t = 0;
    for (int i = 0; i &lt; A.size(); i &#43;&#43; )
    {
        t &#43;= A[i];
        if (i &lt; B.size()) t &#43;= B[i];
        C.push_back(t % 10);
        t /= 10;
    }

    if (t) C.push_back(t);
    return C;
}
```

### 减法

```c&#43;&#43;
// C = A - B, 满足A &gt;= B, A &gt;= 0, B &gt;= 0
vector&lt;int&gt; sub(vector&lt;int&gt; &amp;A, vector&lt;int&gt; &amp;B)
{
    vector&lt;int&gt; C;
    for (int i = 0, t = 0; i &lt; A.size(); i &#43;&#43; )
    {
        t = A[i] - t;
        if (i &lt; B.size()) t -= B[i];
        C.push_back((t &#43; 10) % 10);
        if (t &lt; 0) t = 1;
        else t = 0;
    }

    while (C.size() &gt; 1 &amp;&amp; C.back() == 0) C.pop_back();
    return C;
}
```

### 乘法

~~~c&#43;&#43;
// C = A * b, A &gt;= 0, b &gt;= 0
vector&lt;int&gt; mul(vector&lt;int&gt; &amp;A, int b)
{
    vector&lt;int&gt; C;

    int t = 0;
    for (int i = 0; i &lt; A.size() || t; i &#43;&#43; )
    {
        if (i &lt; A.size()) t &#43;= A[i] * b;
        C.push_back(t % 10);
        t /= 10;
    }

    while (C.size() &gt; 1 &amp;&amp; C.back() == 0) C.pop_back();

    return C;
}
~~~

### 除法

```c&#43;&#43;
// A / b = C ... r, A &gt;= 0, b &gt; 0
vector&lt;int&gt; div(vector&lt;int&gt; &amp;A, int b, int &amp;r)
{
    vector&lt;int&gt; C;
    r = 0;
    for (int i = A.size() - 1; i &gt;= 0; i -- )
    {
        r = r * 10 &#43; A[i];
        C.push_back(r / b);
        r %= b;
    }
    reverse(C.begin(), C.end());
    while (C.size() &gt; 1 &amp;&amp; C.back() == 0) C.pop_back();
    return C;
}
```



## 前缀和与差分

### 前缀和

（累加生成序列）

#### 一维

类比数列

$S(n)=\sum_{i=0}^{n}a(i) $

则有

$S[i] = a[1] &#43; a[2] &#43; ... a[i]$
$a[l] &#43; ... &#43; a[r] = S[r] - S[l - 1]$



```C&#43;&#43;
#include&lt;iostream&gt;
using namespace std;

const int N=1e5&#43;10;
int a[N],sum[N];//全局变量默认初始值为0

int main(){
    int m,n,x;
    cin&gt;&gt;n&gt;&gt;m;
    for(int i=1;i&lt;=n;i&#43;&#43;){
        cin&gt;&gt;x;
        sum[i]=x&#43;sum[i-1];//sum[i]表示序列中前i个数之和
    }
    int l,r;
    for(int i=0;i&lt;m;i&#43;&#43;){
        cin&gt;&gt;l&gt;&gt;r;
        //注意是l-1，因为l到r之间的数之和包括第l个数
        printf(&#34;%d\n&#34;,sum[r]-sum[l-1]);
    }
}
```



#### 二维

$S(m,n)=\sum_{j=0}^{n} \sum_{i=0}^{m} a(i,j) $

$S[x1--x2,y1--y2]=S[x2, y2] - S[x1 - 1, y2] - S[x2, y1 - 1] &#43; S[x1 - 1, y1 - 1]$

&gt;x-1 ：点阵



```C&#43;&#43;
#include&lt;iostream&gt;
using namespace std;
const int N=1010;
int S[N][N],a[N][N];//a用于存放矩阵，s是矩阵的前缀和矩阵

int main(){
    //1.读入数据
    int n,m,q;
    cin&gt;&gt;n&gt;&gt;m&gt;&gt;q;
    for(int i=1;i&lt;=n;i&#43;&#43;){
        for(int j=1;j&lt;=m;j&#43;&#43;){
            cin&gt;&gt;a[i][j];
        }
    }
    //2.初始化和矩阵
    for(int i=1;i&lt;=n;i&#43;&#43;){
        for(int j=1;j&lt;=m;j&#43;&#43;){
            S[i][j]=S[i-1][j]&#43;S[i][j-1]-S[i-1][j-1]&#43;a[i][j];
        }
    }
    //3.计算(x1,y1)和(x2,y2)之间的数之和
    int x1,y1,x2,y2;
    while(q--){
        cin&gt;&gt;x1&gt;&gt;y1&gt;&gt;x2&gt;&gt;y2;
        cout&lt;&lt;S[x2][y2]-S[x2][y1-1]-S[x1-1][y2]&#43;S[x1-1][y1-1]&lt;&lt;endl;
    }
}
```



### 差分

思路：

1. 前缀和的逆运算

2. a[i]=b[1]&#43;b[2]&#43;b[3]&#43;···&#43;b[i]，则称b为a的差分数组，a为b的前缀和，且b满足：

​		b[1]=a[1];

​		b[2]=a[2]-a[1];

​		b[3]=a[3]-a[2];

​		b[n]=a[n]-a[n-1]; 

3. a[i]&#43;c,对b数组的影响是b[i]&#43;c，并且b[i&#43;1]-c。*(从a[i]=b[1]&#43;b[2]&#43;b[3]&#43;···&#43;b[i]中可以得到）*

​		a数组的第l到r个数＋c，对b数组的影响是b[l]&#43;c并且b[r&#43;1]-c。



步骤：

1. 确定插入方式
2. 构造原始序列（0序列满足差分及前缀和）
3. 插入获取目标序列（**同时生成相应差分序列**）



#### 一维

给区间[l, r]中的每个数加上c：B[l] &#43;= c, B[r &#43; 1] -= c

```c&#43;&#43;
#include&lt;iostream&gt;
using namespace std;
const int N=100010;
int n,m,a[N],b[N];

void insert(int l,int r,int c){
    b[l]&#43;=c;
    b[r&#43;1]-=c;
}

int main(){
    cin&gt;&gt;n&gt;&gt;m;
    for(int i=1;i&lt;=n;i&#43;&#43;) cin&gt;&gt;a[i];
    for(int i=1;i&lt;=n;i&#43;&#43;) insert(i,i,a[i]);
    int l,r,c;
    while(m--){
        cin&gt;&gt;l&gt;&gt;r&gt;&gt;c;
        insert(l,r,c);
    }
    //a是b的前缀和，求完之后b的值也就变成自身的前缀和了，也就是a
    for(int i=1;i&lt;=n;i&#43;&#43;) b[i]&#43;=b[i-1],cout&lt;&lt;b[i]&lt;&lt;&#39; &#39;;
    
}
```



#### 二维

给以(x1, y1)为左上角，(x2, y2)为右下角的子矩阵中的所有元素加上c：

- B[x1, y1] &#43;= c
- B[x2 &#43; 1, y1] -= c
- B[x1, y2 &#43; 1] -= c
- B[x2 &#43; 1, y2 &#43; 1] &#43;= c

```c&#43;&#43;
#include &lt;iostream&gt;
using namespace std;

const int N=1010;
int a[N][N],b[N][N];

void insert(int x1,int y1,int x2,int y2,int c){
    //1.前缀和a矩阵在(x1,y1)到(x2,y2)的范围整体加c，对b的影响
    b[x1][y1]&#43;=c;
    b[x1][y2&#43;1]-=c;
    b[x2&#43;1][y1]-=c;
    b[x2&#43;1][y2&#43;1]&#43;=c;
}
int main(){
    int n,m,q;
    scanf(&#34;%d%d%d&#34;,&amp;n,&amp;m,&amp;q);
    //1.读入数据
    for(int i=1;i&lt;=n;i&#43;&#43;){
        for(int j=1;j&lt;=m;j&#43;&#43;){
            scanf(&#34;%d&#34;,&amp;a[i][j]);
        }
    }
    //2.初始化差分数组
    for(int i=1;i&lt;=n;i&#43;&#43;){
        for(int j=1;j&lt;=m;j&#43;&#43;){
            insert(i,j,i,j,a[i][j]);
        }
    }
    //3.循环处理
    while(q--){
        int x1,y1,x2,y2,c;
        cin&gt;&gt;x1&gt;&gt;y1&gt;&gt;x2&gt;&gt;y2&gt;&gt;c;
        insert(x1,y1,x2,y2,c);
    }
    //4.得到b的前缀和a
    for(int i=1;i&lt;=n;i&#43;&#43;){
        for(int j=1;j&lt;=m;j&#43;&#43;){
            a[i][j]=b[i][j]&#43;a[i-1][j]&#43;a[i][j-1]-a[i-1][j-1];
            cout&lt;&lt;a[i][j]&lt;&lt;&#39; &#39;;
        }
        cout&lt;&lt;endl;
    }
    return 0;
}
```



## 双指针

思路：

1. 原始做法
2. 双指针简化流程（思考是否具备相关过程）



```c&#43;&#43;
for (int i = 0, j = 0; i &lt; n; i &#43;&#43; )
{
    while (j &lt; i &amp;&amp; check(i, j)) j &#43;&#43; ;

    // 具体问题的逻辑
}
```



**常见问题分类：**
    (1) 对于一个序列，用两个指针维护一段区间
    (2) 对于两个序列，维护某种次序，比如归并排序中合并两个有序序列的操作



**数组元素的目标和**

- 给定两个升序排序的有序数组 A和 B，以及一个目标值 x。

- 数组下标从 0 开始。

- 请你求出满足 A[i]&#43;B[j]=x的数对 (i,j)。

- 数据保证有唯一解。

```c&#43;&#43;
#include&lt;iostream&gt;
using namespace std;
const int N=100010;
int a[N],b[N];

int main(){
    //1.读入数据
    int n,m,x;
    scanf(&#34;%d%d%d&#34;,&amp;n,&amp;m,&amp;x);
    for(int i=0;i&lt;n;i&#43;&#43;) scanf(&#34;%d&#34;,&amp;a[i]);
    for(int i=0;i&lt;m;i&#43;&#43;) scanf(&#34;%d&#34;,&amp;b[i]);
    //2.双指针相向移动 找出i j
    for(int i=0,j=m-1;i&lt;n;i&#43;&#43;){
        while(a[i]&#43;b[j]&gt;x &amp;&amp; j&gt;=0){
            j--;
        }
        if(a[i]&#43;b[j]==x) {
            printf(&#34;%d %d\n&#34;,i,j);
            break;
        }
    }
}
```



## 位运算

**求n的第k位数字**

1. n是一个十进制数，要求n的二进制表达里的第k位数字 **n&gt;&gt;k&amp;1**

   以下代码会输出n=10的二进制表达

```C&#43;&#43;
int n=10;
for(int i=31;i&gt;=0;i--) cout &lt;&lt; (x &gt;&gt; i &amp; 1);
```

2. 求十进制数n的二进制表达中，最后一位1，比如10的二进制表达是1010，最后一位1是10，10100的最后一位1是100。

lowbit函数实现如下：

```C&#43;&#43;
int lowbit(int x){
    return x &amp; (-x);
}
```

[关于lowbit的那些事 - 凌云_void - 博客园 (cnblogs.com)](https://www.cnblogs.com/lingyunvoid/p/15165866.html)

3.例题：

统计一个十进制数的二进制表达中1的个数

```C&#43;&#43;
#include&lt;iostream&gt;
using namespace std;

int lowbit(int x){
    return x &amp; (-x);
}

int main(){
    //1.有n个数据
    int n;
    cin&gt;&gt;n;
    while(n--){
    //2.每次对一个数据做计算处理
        int number;
        cin&gt;&gt;number;
        int num=0;
    //3.当数据不为0时说明数据的二进制表示里还有1
        while(number){
        number-=lowbit(number);
        num&#43;&#43;;
        }
        cout&lt;&lt;num&lt;&lt;&#39; &#39;;
    }
}
```



## 离散化

思路：

1. 定规则（满足二分条件）
2. 二分离散

```c&#43;&#43;
vector&lt;int&gt; alls; // 存储所有待离散化的值
sort(alls.begin(), alls.end()); // 将所有值排序
alls.erase(unique(alls.begin(), alls.end()), alls.end());   // 去掉重复元素

// 二分求出x对应的离散化的值
int find(int x) // 找到第一个大于等于x的位置
{
    int l = 0, r = alls.size() - 1;
    while (l &lt; r)
    {
        int mid = l &#43; r &gt;&gt; 1;
        if (alls[mid] &gt;= x) r = mid;
        else l = mid &#43; 1;
    }
    return r &#43; 1; // 映射到1, 2, ...n
}
```



**例题**

1. ​	假定有一个无限长的数轴，数轴上每个坐标上的数都是 00。

2. ​	现在，我们首先进行 n次操作，每次操作将某一位置 x上的数加 c。

3. ​	接下来，进行 m 次询问，每个询问包含两个整数 l和 r，你需要求出在区间 [l,r]之间的所有数的和。

```c&#43;&#43;
#include&lt;iostream&gt;
#include&lt;algorithm&gt;
#include&lt;vector&gt;

using namespace std;

typedef pair&lt;int,int&gt; PII;
const int N=300010;
int a[N],s[N];//存放所有下标的值和前缀和

vector&lt;PII&gt; add,query;//add存储插入的数对x,c query存储查询的数对l,r
vector&lt;int&gt; alls;//存放所有出现的下标，包括插入的x和查询的左右边界l r

int find(int x){
    int l=0,r=alls.size()-1;
    while(l&lt;r){
        int mid = l &#43; r &gt;&gt; 1;
        if(alls[mid]&gt;=x) r=mid;
        else l=mid&#43;1;
    }
    return r&#43;1;//代表在区间1，2，3···到alls.size()映射
}

int main(){
    int n,m;
    cin&gt;&gt;n&gt;&gt;m;
    //1.读取n个要插入的数据
    for(int i=1;i&lt;=n;i&#43;&#43;){
        int x,c;
        cin&gt;&gt;x&gt;&gt;c;
        add.push_back({x,c});
        alls.push_back(x);
    }
    //2.读取m对要查询的左右边界
    for(int i=1;i&lt;=m;i&#43;&#43;){
        int l,r;
        cin&gt;&gt;l&gt;&gt;r;
        query.push_back({l,r});
        alls.push_back(l);
        alls.push_back(r);//记得把左右边界的下标也一并记录
    }
    //3.对下标进行排序并去重
    sort(alls.begin(),alls.end());
    alls.erase(unique(alls.begin(),alls.end()),alls.end());
    //4.离散化
    //离散化就是把大而分散的一段段使用到的稀疏区间，整合映射到连续的一段较小的稠密区间里
    for(auto item:add){
        int x=find(item.first);
        a[x]&#43;=item.second;
    }
    //5.预处理前缀和
    for(int i=1;i&lt;=alls.size();i&#43;&#43;) s[i]=s[i-1]&#43;a[i];
    //6.开始查询
    for(auto item:query){
        int l=find(item.first),r=find(item.second);//先找边界映射的下标
        int sum = s[r] - s[l-1];
        cout&lt;&lt;sum&lt;&lt;endl;
    }
}
```

## 区间合并

思路：贪心

目标：合并具有交集的区间

条件：

1. 有序规则
2. 交集

```c&#43;&#43;
// 将所有存在交集的区间合并
void merge(vector&lt;PII&gt; &amp;segs)
{
    vector&lt;PII&gt; res;

    sort(segs.begin(), segs.end());//排序

    int st = -2e9, ed = -2e9; //起始区间（前一个区间）
    for (auto seg : segs)
        if (ed &lt; seg.first) //区间没有交集则 独立并剔出 前一个区间
        {
            if (st != -2e9) res.push_back({st, ed});
            st = seg.first, ed = seg.second;
        }
        else ed = max(ed, seg.second); //存在交集则 扩大前一个区间 但不剔出

    if (st != -2e9) res.push_back({st, ed}); //加入最后一个区间

    segs = res;
}
```



```c&#43;&#43;
#include&lt;iostream&gt;
#include&lt;vector&gt;
#include&lt;algorithm&gt;

using namespace std;

typedef pair&lt;int,int&gt; PII;
vector&lt;PII&gt; segs;

void merge(vector&lt;PII&gt; &amp;segs){
    vector&lt;PII&gt; res;//用于存储合并后的区间
    //1.将区间按照左端点大小升序排序（默认按照segs.first排序）
    sort(segs.begin(),segs.end());
    //2.开始合并
    int st = -2e9,ed=-2e9;//st ed存储上一个区间的左右端点
    for(auto seg :segs){
        //如果当前区间的左端点大于上一个区间的右端点 说明上一个区间独立了
        if(seg.first &gt; ed){ 
            if(st!=-2e9) res.push_back({st,ed});//第一次循环不执行
            //把当前区间作为要维护的下一个区间
            st = seg.first, ed = seg.second; 
        }
        //如果当前区间的左端点不大于上一个区间的右端点 那么合并当前和上一个区间
        else ed = max(ed,seg.second);
    }
    if(st!=-2e9) res.push_back({st,ed});//记得把最后一个区间的结果加进去
    segs = res;
}

int main(){
    int n;
    cin&gt;&gt;n;
    while(n--){
        int l,r;
        cin&gt;&gt;l&gt;&gt;r;
        segs.push_back({l,r});
    }
    merge(segs);
    cout&lt;&lt;segs.size()&lt;&lt;endl;
}
```



# 数据结构

核心：数组模拟-数据结构

原因：new（）动态分派操作非常慢

## 链表与邻接表

### 单链表

- 单链表：邻接表（存储 图、树）（n个单链表）

&gt;存在内存浪费，但算法题中不考虑

![image-20240227145038811](https://qiu-media.oss-cn-wuhan-lr.aliyuncs.com/img/image-20240227145038811.png)

~~~c&#43;&#43;
#include&lt;iostream&gt;
using namespace std;

const int N = 100010;
//head 表示头结点是插入的第几个数据
//e[N]表示节点的值
//ne[N]表示节点下一个指向第几个插入的数据
//idx表示插入到第几个数据
int head,e[N],ne[N],idx;

//初始化
void init(){
    head = -1;
    idx = 0;
}
//向链表头插入一个数x
void add_to_head(int x){
    idx&#43;&#43;;
    e[idx] = x;
    ne[idx] = head;
    head = idx;
}
//表示在第 k个插入的数后面插入一个数 x
void add(int k,int x){
    idx&#43;&#43;;
    e[idx] = x;
    ne[idx] = ne[k];
    ne[k] = idx;
}
//表示删除第k个插入的数后面的数 记得考虑k=0的情况，即删除头结点
void remove(int k){
    if(!k) head = ne[head];
    else ne[k] = ne[ne[k]];
}

int main(){
    init();
    int m;
    cin&gt;&gt;m;
    while(m--){
        char op;
        cin&gt;&gt;op;
        if(op==&#39;H&#39;){
            int x;
            cin&gt;&gt;x;
            add_to_head(x);
        }
        if(op==&#39;D&#39;){
            int k;
            cin&gt;&gt;k;
            remove(k);
        }
        if(op==&#39;I&#39;){
            int k,x;
            cin&gt;&gt;k&gt;&gt;x;
            add(k,x);
        }
    }
    for(int i=head;i!=-1;i=ne[i]){
        cout&lt;&lt;e[i]&lt;&lt;&#39; &#39;;
    }
}
~~~

第二种处理方式，注意idx&#43;&#43;位置引起的区别

~~~c&#43;&#43; 
#include&lt;iostream&gt;
using namespace std;

const int N=100010;
int head,e[N],ne[N],idx;

void init(){
    head=-1;
    idx=0;
}

void add_to_head(int x){
    e[idx]=x;
    ne[idx]=head;
    head=idx;
    idx&#43;&#43;;
}

void add(int k, int x){
    e[idx]=x;
    ne[idx]=ne[k];
    ne[k]=idx;
    idx&#43;&#43;;
}

void remove(int k){
    ne[k]=ne[ne[k]]; //不能为ne[k&#43;&#43;]
}

int main(){
     init();
    int m;
    cin&gt;&gt;m;
    while(m--){
        char op;
        cin&gt;&gt;op;
        if(op==&#39;H&#39;){
            int x;
            cin&gt;&gt;x;
            add_to_head(x);
        }
        if(op==&#39;D&#39;){
            int k;
            cin&gt;&gt;k;
            if(!k) head = ne[head];
            remove(k-1);
        }
        if(op==&#39;I&#39;){
            int k,x;
            cin&gt;&gt;k&gt;&gt;x;
            add(k-1,x);
        }
    }
    for(int i=head;i!=-1;i=ne[i]){
        cout&lt;&lt;e[i]&lt;&lt;&#39; &#39;;
    }
}
~~~

### 双链表

- 双链表：优化某些问题

```c&#43;&#43;
#include&lt;iostream&gt;
using namespace std;

const int N=100010;
int e[N],l[N],r[N],idx;

//初始化
void init(){
    r[o]=1;
    l[1]=0;
    idx=2;//前两个节点已经使用
}

//k点右插为主所写， k点左插=add(l[k],x)
void add(int k,int x){
	e[idx]=x;
    
    r[idx]=r[k];//1
    l[idx]=k;//2
    
    l[r[k]]=idx;//3
    r[k]=idx; //4
    //l[r[idx]]=idx; 可和3交换
}
//删除第k个点
void remove(int k){
    r[l[k]]=r[k];
    l[r[k]]=l[k];
}

```

![image-20240227153430787](https://qiu-media.oss-cn-wuhan-lr.aliyuncs.com/img/image-20240227153430787.png)

![image-20240227153930210](https://qiu-media.oss-cn-wuhan-lr.aliyuncs.com/img/image-20240227153930210.png)

## 栈与队列

### 栈

```c&#43;&#43;
// tt表示栈顶
int stk[N], tt = 0;

// 向栈顶插入一个数
stk[ &#43;&#43; tt] = x;

// 从栈顶弹出一个数
tt -- ;

// 栈顶的值
stk[tt];

// 判断栈是否为空，如果 tt &gt; 0，则表示不为空
if (tt &gt; 0)
{

}
```

### 队列

普通队列

```c&#43;&#43;
// hh 表示队头，tt表示队尾
int q[N], hh = 0, tt = -1;

// 向队尾插入一个数
q[ &#43;&#43; tt] = x;

// 从队头弹出一个数
hh &#43;&#43; ;

// 队头的值
q[hh];

// 判断队列是否为空，如果 hh &lt;= tt，则表示不为空
if (hh &lt;= tt)
{

}
```

循环队列

```c&#43;&#43;
// hh 表示队头，tt表示队尾的后一个位置
int q[N], hh = 0, tt = 0;

// 向队尾插入一个数
q[tt &#43;&#43; ] = x;
if (tt == N) tt = 0;

// 从队头弹出一个数
hh &#43;&#43; ;
if (hh == N) hh = 0;

// 队头的值
q[hh];

// 判断队列是否为空，如果hh != tt，则表示不为空
if (hh != tt)
{

}
```



### 单调栈与单调队列（抽象但题型较少）

**朴素做法-&gt;优化无效值-&gt;观察单调性-&gt;优化**

### 单调栈

严格单调

强调目标：最近 且 较小

&gt; 相当于“最近”指标权重更大   故逆序对or相等时 较远的元素无优势
&gt;
&gt; 故严格单调

~~~c&#43;&#43;
常见模型：找出每个数左边离它最近的比它大/小的数
int tt = 0;
for (int i = 1; i &lt;= n; i &#43;&#43; )
{
    while (tt &amp;&amp; check(stk[tt], i)) tt -- ;
    stk[ &#43;&#43; tt] = i;//必须压入元素 此处&#43;&#43;tt表示栈顶&#43;1位置
}
~~~

### 单调队列（滑动窗口）

```c&#43;&#43;
常见模型：找出滑动窗口中的最大值/最小值
int hh = 0, tt = -1;
for (int i = 0; i &lt; n; i &#43;&#43; )
{
    while (hh &lt;= tt &amp;&amp; check_out(q[hh])) hh &#43;&#43; ;  // 判断队头是否滑出窗口
    while (hh &lt;= tt &amp;&amp; check(q[tt], i)) tt -- ;
    q[ &#43;&#43; tt] = i;
}
```

## KMP

核心：快速移动 · 所需匹配模板字符串 ，从而降低时间复杂度

![91908708eaef29561643b407e0c7a71d](https://qiu-media.oss-cn-wuhan-lr.aliyuncs.com/img/91908708eaef29561643b407e0c7a71d.png)

![9dcbe8b7a2e43b1f39b8e676b562d2ff](https://qiu-media.oss-cn-wuhan-lr.aliyuncs.com/img/9dcbe8b7a2e43b1f39b8e676b562d2ff.png)

**步骤：**

1、原始做法逐个字母开始 匹配

2、思考如何简化，发现如果存在最大的前缀和后缀相等，那么可以直接进行移动，此举大大加快了速率



**移动的前提：**

 知道最大的前缀和后缀相等时是多少，即知道next数组



**求取next数组：**

考虑对模板字符串进行自匹配，需要求取 ne[1]、ne[2]乃至ne[N]

故采用循环的方式，逐步从2~m  以此扩大所匹配的字符串，匹配用模板前缀匹配模板后缀，同时进行赋值计算



&gt;注意到，当目标字符串为k时
&gt;
&gt;匹配的模板字符串最大为k-1
&gt;
&gt;故赋值时可以赋值到ne[k-1]，所以模板匹配时无需担心ne[i], i&lt;k不存在



**参考解释：**

[AcWing 831. KMP字符串 - AcWing](https://www.acwing.com/solution/content/14666/)



```c&#43;&#43;
// s[]是长文本，p[]是模式串，n是s的长度，m是p的长度
求模式串的Next数组：
for (int i = 2, j = 0; i &lt;= m; i &#43;&#43; )
{
    while (j &amp;&amp; p[i] != p[j &#43; 1]) j = ne[j];
    if (p[i] == p[j &#43; 1]) j &#43;&#43; ;
    ne[i] = j;
}

// 匹配
for (int i = 1, j = 0; i &lt;= n; i &#43;&#43; )
{
    while (j &amp;&amp; s[i] != p[j &#43; 1]) j = ne[j];
    if (s[i] == p[j &#43; 1]) j &#43;&#43; ;
    if (j == m)
    {
        j = ne[j];
        // 匹配成功后的逻辑
    }
}
```



## Tier

高效地存储和查找字符串

![image-20240228221951820](https://qiu-media.oss-cn-wuhan-lr.aliyuncs.com/img/image-20240228221951820.png)



## 最大异或对

化简暴力搜索异或对的内层循环

$C_N^2$

可以：边插入边查找（）

~~~c&#43;&#43;
#include &lt;iostream&gt;

uisng namespace std;
const int N=100010,M=31*N;//M为节点个数
int n,a[N],son[M][2],idx;

void insert(int x){
    int p=0;
    for (int i=30;i&gt;=0;i--){
        int u=x&gt;&gt; i &amp;1;
        if(!son[p][u]) son[p][u]=&#43;&#43;idx;
        p=son[p][o];
    }
}

void query(int x){
    int p=0, res=0;
    for(int i=30;i&gt;=0;i--){
        int u=x&gt;&gt;i&amp;1;
        if(son[p][!u]) {
            p=son[p][!u];
            res=2*res&#43;!u;
        }
        else {
            p=son[p][u];
            res=2*res&#43;u;
        }
    }
    return res;//返回值
}

int main(){
    cin&gt;&gt;n;
    for (int i=0;i&lt;n;i&#43;&#43;) cin&gt;&gt;a[i];
    int res=0;
    for(int i=0; i&lt; n; i&#43;&#43;){
        insert(a[i]);
        int t=auery(a[i]);
        res=max(res,a[i]^t);
    }
    cout&lt;&lt;res;
    
    return 0;
}
~~~





## 并查集

**集合合并**

集合为基本元素



1、将两个集合合并

2、询问两个元素是否在一个集合中



近乎o(1)的时间复杂度内完成相应操作



基本原理：

集合用树表示

根节点p[x]=x



问题1：如何判断树根： p[x]==x

问题2：如何求x的集合编号： while(p[x]!=x) x=p[x];

问题3：如何合并两个集合：px，py两个集合编号 合并：p[x]=y即可



路径压缩优化（按秩合并优化未讲--用的少）



![image-20240229113513976](https://qiu-media.oss-cn-wuhan-lr.aliyuncs.com/img/image-20240229113513976.png)





scanf(&#34;s%&#34;) 扫描字符串会过滤回车，但是c%不会，因此对单个字符也可采用字符串进行处理（过滤空格和回车）（血的教训ovo）

~~~c&#43;&#43;
int find(int x) //返回x的祖宗节点&#43;路径压缩
{
    if(p[x]!=x)
        p[x]=find[p[x]];
    return p[x];
}
~~~



```c&#43;&#43;
#include&lt;iostream&gt;
using namespace std;
const int N=100010;
int p[N],sz[N],n,m;

int find(int x){//找到编号为x的数的集合编号 &#43; 路径压缩
    if(p[x]!=x) p[x]=find(p[x])//如果该节点不是根节点则让其父节点等于根节点（递归实现路径压缩）
    return p[x];
}

int main(){
    scanf(&#34;%d%d&#34;,&amp;n,&amp;m);
    for (int i=0;i&lt;n;i&#43;&#43;){
        p[i]=i;
        sz[i]=1;
    }
    while(m--){
        char op[5];
        int a,b;
        scanf(&#34;%s&#34;,op);
        if(op[0]==&#39;C&#39;){
            scanf(&#34;%d%d&#34;,&amp;a,&amp;b);
            if find(b)==find(a) continue;
            sz[find(b)]&#43;=sz[find(a)];
            p[find(a)]=find(b);//注意要在根节点的size相加之后再合并，两句话不能颠倒
        }
        else if(op[1] == &#39;1&#39;){
            scanf(&#34;%d%d&#34;,&amp;a,&amp;b);
            if(find(a)==find(b))
                puts(&#34;Yes&#34;);
            else
                puts(&#34;No&#34;);
        }
        else{
            scanf(&#34;%d&#34;,&amp;a);
            printf(&#34;%d\n&#34;,sz[find(a)]);
        }
    }
    return 0;
}
```



## 食物链

![image-20240301220648118](https://qiu-media.oss-cn-wuhan-lr.aliyuncs.com/img/image-20240301220648118.png)

![image-20240301220713269](https://qiu-media.oss-cn-wuhan-lr.aliyuncs.com/img/image-20240301220713269.png)

![image-20240301220731077](https://qiu-media.oss-cn-wuhan-lr.aliyuncs.com/img/image-20240301220731077.png)

同于集合内 知道节点与根节点之间的距离 即知道 两节点间的相互关系（由于仅有 三种变量 故 中间变量可以传递）

![image-20240301204904852](https://qiu-media.oss-cn-wuhan-lr.aliyuncs.com/img/image-20240301204904852.png)





&gt;什么时间用什么数据结构
&gt;
&gt;寻找最小最大值：堆
&gt;
&gt;维护有序列表：平衡树 site
&gt;
&gt;区间最大值，区间和：树状数组 线段树
&gt;
&gt;并查集题目：不直观、需训练



## 堆

堆：完全二叉树

小根堆：每个点小于等于左右子节点（根节点为最小值）（左右叶子节点无大小关系）

如何手写一个堆

1、插入一个数

2、求集合当中的最小值

3、删除最小值

stl无法实现的内容：

4、删除任意元素

5、修改任意元素

![image-20240229123347506](https://qiu-media.oss-cn-wuhan-lr.aliyuncs.com/img/image-20240229123347506.png)



存储：一维数组

根节点：1 左子节点：2x 右子节点2x&#43;1

两个操作： 节点上下移动：down(x) up(x) 



### 堆排序

### 模拟堆

建堆

~~~c&#43;&#43;
for (int i=n/2;i;i--){
    down(i);
}
~~~

```c&#43;&#43;
void down(int u){
	int t=u;//t:最小值位置
    if(2*u&lt;size &amp;&amp; p[2*u] &lt; p[t])
        t=2*u;
    if(2*u&#43;1&lt;size &amp;&amp; p[2*u&#43;1] &lt; p[t])
        t=2*u&#43;1;
    if(u!=t){
        swap(h[u],h[t]);//根节点不是最小则交换子节点，并从子节点向下down
        down(t);
    }
}
```

~~~c&#43;&#43;
void up(int u){
	while(u/2&amp;&amp;h[u/2] &gt; h[u]){
        swap(h[u/2],h[u]);
    	u/=2;
    }
}
~~~

```c&#43;&#43;
void heat_swap(int a,int b){
	swap(ph[hp[a]],ph[hp[b]]);//hp:堆到第k个插入的数 ph:第k个插入的数到堆 pointer-heap=ph
    swap(hp[a],hp[b]);
    swap(h[a],h[b]);
}
```





## 哈希表与STL简介

队列、优先队列



### 哈希表

- 存储结构

  ​	开放寻址

  ​	拉链法

- 字符串的哈希方式



作用：大数据映射小范围



考虑问题：

1、一般可取模 缩小

2、映射应该一对一

3、按照处理冲突（一对多映射）方式，可以分为两种方法：开放寻址发与拉链法



#### 拉链法

哈希表算法题中一般只有：插入 查询两种操作，删除操作（假删除，开一个标记，标记其无用即可）

![image-20240302112931814](https://qiu-media.oss-cn-wuhan-lr.aliyuncs.com/img/image-20240302112931814.png)



![image-20240302114904162](https://qiu-media.oss-cn-wuhan-lr.aliyuncs.com/img/image-20240302114904162.png)

样本空间缩小，碰撞概率上升

```c&#43;&#43;
// C&#43;&#43; Version
const int N = 1e5 &#43; 10;
int h[N], e[N], ne[N], idx;

void insert(int x) {
    int k = (x % N &#43; N) % N;  // 把负数也映射为正数。—— c&#43;&#43; 中负数 mod 正数还是负数
    e[idx] = x, ne[idx] = h[k], h[k] = idx&#43;&#43;;  // 链表头插
}

bool find(int x) {
    int k = (x % N &#43; N) % N;
    for(int i = h[k]; ~i; i = ne[i])  // 链表查询遍历
        if(e[i] == x) return true;
    return false;
}
```





#### 开放寻址法

开数组长度为数据的2-3倍（经验值，冲突概率较低）

```c&#43;&#43;
// C&#43;&#43; Version
const int N = 3e5 &#43; 10;
const int null = 0x3f3f3f3f; // 正无穷表示不存在目标数。—— memset(h, 0x3f, sizeof h);
int h[N];

int find(int x) {
    int k = (x % N &#43; N) % N;
    while (h[k] != null &amp;&amp; h[k] != x) { // 说明当前位置有值，但是不是目标值。
        k &#43;&#43; ;
        if (k == N) k = 0;  // 如果走到末尾，则从头再找
        //  该过程一定会停止，因为总共的空位比总的数的个数多。
    }
    return k;	//返回值为：1. 插入位置  2. 查找到x的对应位置
}
```

memset: 按字节set





### 字符串哈希方式

字符串前缀哈希法

![image-20240302123017598](https://qiu-media.oss-cn-wuhan-lr.aliyuncs.com/img/image-20240302123017598.png)

99.99%的概率下，不会发生冲突。

![image-20240302123228965](https://qiu-media.oss-cn-wuhan-lr.aliyuncs.com/img/image-20240302123228965.png)



```c&#43;&#43;
#include &lt;iostream&gt;
using namespace std;

typedef unsigned long long ULL;

const int N=100010,p=131;

int n.m;
char str[N];
ULL h[N],p[N];

//l-r区间内字符串
ULL get(int l,int r){
    return h[r]-h[l-1]*p[r-l&#43;1];
}

int main(){
    sacnf(&#34;%d%d%s&#34;,&amp;n,&amp;m,str&#43;1);
    
    p[0]=1;
    for(int i=1;i&lt;=n;i&#43;&#43;){
        p[i]=p*p[i - 1];
        h[i]=h[i - 1]*p&#43;str[i];
    }
    
}



```





## STL

![image-20240302125018765](https://qiu-media.oss-cn-wuhan-lr.aliyuncs.com/img/image-20240302125018765.png)







---

> 作者: [qiu](https://qiufenggit.github.io/)  
> URL: http://localhost:1313/posts/565a60b/  

