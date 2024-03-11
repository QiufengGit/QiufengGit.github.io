# 蓝桥杯笔记2


蓝桥杯算法基础课第三章笔记

&lt;!--more--&gt;



# 搜索与图论

## DFS与BFS

深度优先搜索 DFS ：stack  O(n)   不具有最短路径

宽度优先搜索 BFS ： queue  O($2^n$)   最短路（由于每层开始搜索，故搜到点的最高层）

树与图的存储

树与图的深度优先遍历

树与图的宽度优先遍历

拓扑排序



## DFS

回溯、剪枝



## 例：排列数字

```c&#43;&#43;
#include&lt;iostream&gt;
using namespace std;

const int N=10;

int n;
int path[N];
bool st[N];

void dfs (int u){ //u 表示层数
    if (u==n){
        for (int i=0;i&lt;n;i&#43;&#43;)
        cout&lt;&lt;path[i]&lt;&lt;&#34; &#34;;
        
        cout&lt;&lt;endl;
    }
    for (int i=1;i&lt;=n;i&#43;&#43;){//循环：从左到右遍历  //注意：i开始位置
        if(!st[i]){
            path[u]=i;
            st[i]=true;
            dfs(u&#43;1); //递归：从上到下遍历
            // path[u]=0;//无用、一定会被覆盖 可删除
            
            st[i]=false;//注意恢复修改的内容
            
        }
        
    }
}

int main(){
    cin&gt;&gt;n;
    
    dfs(0);
}
```

## 例：N-皇后   问题

每行一个 按行枚举

```c&#43;&#43;
#include&lt;iostream&gt;
using namespace std;

const int N=20;//对角线个数2n-1

int n;
char g[N][N];
bool col[N],dg[N],udg[N]; //以行为索引，仅需要开辟: 列数组及正、反对角线数组

void dfs (int u){ //u 表示层数
    if (u==n){
        for (int i=0;i&lt;n;i&#43;&#43;)
        cout&lt;&lt;g[i]&lt;&lt;endl;
        cout&lt;&lt;endl;
        
    }
    for (int i=0;i&lt;n;i&#43;&#43;){//循环：从左到右遍历(列遍历)  //注意：i开始位置
        if(!col[i] &amp;&amp; !dg[u&#43;i] &amp;&amp; !udg[i-u&#43;n]){
            g[u][i]=&#39;Q&#39;;
            col[i] = dg[u&#43;i] = udg[i-u&#43;n] = true;
            dfs(u&#43;1); //递归：从上到下遍历（行遍历）
            // path[u]=0;//无用、一定会被覆盖 可删除
            col[i] = dg[u&#43;i] = udg[i-u&#43;n] = false;//注意恢复修改的内容
            g[u][i]=&#39;.&#39;;
        }
        
    }
}

int main(){
    cin&gt;&gt;n;
    
    for (int i=0;i&lt;n;i&#43;&#43;)
     for (int j=0;j&lt;n;j&#43;&#43;)
        g[i][j]=&#39;.&#39;;
    
    dfs(0);
}
```

按格枚举

```c&#43;&#43;
#include&lt;iostream&gt;
using namespace std;

const int N=20;

int n;
char g[N][N];
bool row[N],col[N],dg[N],udg[N]; 

void dfs (int x, int y, int s){
    
    if (y==n) y=0,x&#43;&#43;;
    if (x==n){
        if (s==n){
            for (int i=0;i&lt;n;i&#43;&#43;)
            cout&lt;&lt;g[i]&lt;&lt;endl;
            cout&lt;&lt;endl;
        }
        return; //无所谓？
    }
    
    dfs(x,y&#43;1,s);
    
    if(!row[x] &amp;&amp; !col[y] &amp;&amp; !dg[x&#43;y] &amp;&amp; !udg[x-y&#43;n]){
        g[x][y]=&#39;Q&#39;;
        row[x] = col[y] = dg[x&#43;y] = udg[x-y&#43;n] = true;
        dfs(x,y&#43;1,s&#43;1); //递归：从上到下遍历（行遍历）
        // path[u]=0;//无用、一定会被覆盖 可删除
        row[x] = col[y] = dg[x&#43;y] = udg[x-y&#43;n] = false;//注意恢复修改的内容
        g[x][y]=&#39;.&#39;;
    }


}

int main(){
    cin&gt;&gt;n;
    
    for (int i=0;i&lt;n;i&#43;&#43;)
     for (int j=0;j&lt;n;j&#43;&#43;)
        g[i][j]=&#39;.&#39;;
    
    dfs(0,0,0);
}
```



![image-20240303234751676](https://qiu-media.oss-cn-wuhan-lr.aliyuncs.com/img/image-20240303234751676.png)



## 例：走迷宫

```c&#43;&#43;
#include&lt;iostream&gt;
#include&lt;cstring&gt;

using namespace std;

typedef pair&lt;int, int&gt; PII;

const int N=110;

int n,m;

int g[N][N];//迷宫
int d[N][N];//距离
PII q[N*N];//带扩展点位置(模拟队列)

int bfs()
{
    int hh=0,tt=0;
    q[0]={0,0};
    
    memset(d,-1,sizeof d);
    d[0][0]=0;
    
    int dx[4]={-1, 0, 1, 0}, dy[4]={0, 1, 0, -1};
    
    while(hh&lt;=tt)
    {
        auto t=q[hh&#43;&#43;];
        
        for(int i=0; i&lt;4; i&#43;&#43;)
        {
            int x=t.first &#43; dx[i];
            int y=t.second &#43; dy[i];
            
            if(x &gt;= 0 &amp;&amp; x &lt; n &amp;&amp; y &gt;=0 &amp;&amp; y &lt;m &amp;&amp; g[x][y]==0 &amp;&amp; d[x][y]==-1) //扩展条件判断
            {
                d[x][y]=d[t.first][t.second]&#43;1;
                q[&#43;&#43;tt]={x,y}; //待扩展目标添加
            }
        }
        
    }
    
    return d[n - 1][m - 1];
    
}

int main(){
    cin&gt;&gt;n&gt;&gt;m;
    for(int i=0;i&lt;n;i&#43;&#43;)
        for (int j=0;j&lt;m;j&#43;&#43;)
         cin&gt;&gt;g[i][j];
    
    cout&lt;&lt;bfs()&lt;&lt;endl;
    
    return 0;
}
```





## 树与图的遍历：拓扑排序

有向图

邻接矩阵：时间复杂度 n^2

邻接表：



## 例：树的重心

```c&#43;&#43;
 #inlcude &lt;iostream&gt;
const int N = 100010,M = N * 2;
int h[N], e[N], ne[N], idx;
bool st[N];//只遍历一次

int ans=N;

int n;

void add(int a, int b)
{
    e[idx]=b;
    ne[idx]=h[a];
    h[a]=idx&#43;&#43;;
  
}

int dfs(int u)
{
    st[u]=true; //标记
    
    int sum=1, res=0;
    for (int i = h[u]; i!=-1; i=ne[i])
    {
        int j = e[i];
        if(!st[j])
        {
            int s = dfs[j];
            res = max(res, s);
            sum &#43;= s; 
        }
    }
    
    res = max(res, n-sum);
    
    ans = min(ans, res);
    
    return sum;
}

int main()
{
    cin&gt;&gt;n;
    memset(h,-1, sizeof(h))
    
    for (int i=0; i&lt;n-1; i&#43;&#43;)\
    {
        int a,b;
        cin &gt;&gt;a &gt;&gt;b;
        add(a,b), add(b,a);
    }
        
    dfs(1);    
    
    cout &lt;&lt; ans &lt;&lt; endl;
    
    return 0;
}

```



## 例：图中点的层次



## 拓扑序列（前后顺序 指向性 不变）



**有向无环图：拓扑图**

**入度：几边 进**

**出度：几边 出**



入度为0：起点

![image-20240304231841923](https://qiu-media.oss-cn-wuhan-lr.aliyuncs.com/img/image-20240304231841923.png)


---

> 作者: [qiu](https://qiufenggit.github.io/)  
> URL: http://localhost:1313/posts/466bc94/  
