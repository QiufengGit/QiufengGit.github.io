# Sumercamp


&lt;!--more--&gt;

# 往年题

### 2004 快排

```cpp
void qsort(double a[],int l,int r){
    if(l&gt;=r) return;
    int i=l-1, j=r&#43;1;
    double x=a[l&#43;r&gt;&gt;1];
    while(i&lt;j){
        do i&#43;&#43;; while(a[i]&lt;x);
        do j--; while(a[j]&gt;x);
        if(i&lt;j) swap(a[i],a[j]);
    }
    qsort(a,l,j);
    qsort(a,j&#43;1,r);
}
```

冒泡

```cpp
for(int i=0;i&lt;25-1;i&#43;&#43;){//次数
    for(int j=0;j&lt;25-1-i;j&#43;&#43;){//每次开始结束位置
        if(ps[j].x&gt;ps[j&#43;1].x) //大的放后面
            swap(ps[j],ps[j&#43;1]);
    }
}
```



### 2005卷积

读文件

```cpp
#include&lt;iostream&gt;
ifstream file_in(&#34;source.txt&#34;);
if(file_in.is_open()){
    cout&lt;&lt;&#34;file open&#34;&lt;&lt;endl;
}
file_in.close();

ofstream file_out(&#34;output.txt&#34;);
```

卷积核

```cpp
//边界是否满足--&gt;计算
if(i-1&#43;m&gt;=0&amp;&amp;i-1&#43;m&lt;125&amp;&amp;j-1&#43;n&gt;=0&amp;&amp;j-1&#43;n&lt;80)
    sum&#43;=b_in[i-1&#43;m][j-1&#43;n]*array1[m][n];
```



### 2006随机数

注意读完后 关闭文件

```cpp
#include&lt;cmath&gt;
#include&lt;time.h&gt;
file_out.close();//必须手动关闭，否则读入时数据不完整
```

0~1随机数生成

```cpp
//种子时间
srand((unsigned)time(NULL));
//生成
for(int i=0;i&lt;100;i&#43;&#43;){
    rand_d[i]=rand()*1.0/RAND_MAX;
}
```



### 2006协方差

动态数组创建

```cpp
//动态数组
int **pix_array=new int*[bd_num];
for(int i=0;i&lt;bd_num;i&#43;&#43;){
    pix_array[i]=new int[pix_num];
}
//内存释放
for(int i=0;i&lt;bd_num;i&#43;&#43;){
    delete[] pix_array[i];
}
delete[] pix_array;
```



### 2007映射

读二进制文件

```cpp
 //文件输入
ifstream f_in(&#34;test.raw&#34;,ios::binary);
if(!f_in.is_open()){
    cout&lt;&lt;&#34;error&#34;&lt;&lt;endl;
    return 0;
}
int data_in[256][256] = {0};
int data_a2b[256 * 256] = {0};
// 读取 256x256 个整数（假设每个整数为 4 字节）
for (int i = 0; i &lt; 256; i&#43;&#43;) {
    for (int j = 0; j &lt; 256; j&#43;&#43;) {
        f_in.read(reinterpret_cast&lt;char*&gt;(&amp;data_in[i][j]), sizeof(unsigned char));
        f_in.read(reinterpret_cast&lt;char*&gt;(&amp;data_a2b[i*256&#43;j]), sizeof(unsigned char));
    }
}
f_in.close();

//文件输出
ofstream f_out(&#34;out.raw&#34;,ios::binary);
if (!f_out.is_open()) {
cout &lt;&lt; &#34;Output error.&#34; &lt;&lt; endl;
return 0;
}
for(int i=0;i&lt;256;i&#43;&#43;){
 for(int j=0;j&lt;256;j&#43;&#43;){
     f_out.write(reinterpret_cast&lt;char*&gt;(&amp;data_out[i][j]),sizeof(unsigned char));
 }
}
f_out.close();
```

```cpp
//二进制输出
FILE *fp = fopen(&#34;out.raw&#34;, &#34;wb&#34;);
if (fp == NULL) {
    cout &lt;&lt; &#34;Output error.&#34; &lt;&lt; endl;
    return 0;
}
fwrite(data_out, sizeof(unsigned char), 256*256, fp);
fclose(fp);
fp=NULL;
```



### 2007外包矩形

快排求最大最小-&gt;比较判断内外即可



### 2008均方差

读入数据、正常算



### 2009字符出现次数

&gt; swap() 交换时 结构体变量全交换

字符转换(利用ASCII码)

char-&#39;0&#39;==ASCII

```cpp
while(fin&gt;&gt;s){
    for(int i=0; i&lt;s.size(); i&#43;&#43;){
        if(s[i]-&#39;A&#39;&gt;25 || s[i]-&#39;A&#39;&lt;0)//小写转大写
            s[i]-=&#39;a&#39;-&#39;A&#39;;
        if(s[i]-&#39;A&#39;&gt;=0 &amp;&amp; s[i]-&#39;A&#39;&lt;=25)//判断属于哪个字母
            words_cnt[s[i]-&#39;A&#39;]&#43;=1;
    }
}
```



### 2010中值滤波

与卷积类似，边界通过超限判断进行计算

对连续的数据，考虑%5的余数进行赋值

```cpp
//中值滤波
for (int i=0; i&lt;nhei; i&#43;&#43;){
    for(int j=0; j&lt;nwid; j&#43;&#43;){
        int a[5]={0};
        for(int m=j-2;m&lt;=j&#43;2;m&#43;&#43;){
            if(0&lt;=m &amp;&amp; m&lt;=nwid-1){//超限判断
                a[m%5]=data[i][m];//只需 取出数据排序即可
            }
        }
        qsort(a,0,5-1);
        data_new[i][j]=a[2];//取中值赋值
    }
}
```



### 2010配送时间

迭代计算点位距离即可



### 2011复杂字符串读取

字符串转数字 stoi()

字符转数字 atoi()

转字符 to_string()

```cpp
#include&lt;cstring&gt;
string snum;
getline(fin,snum);
int num=stoi(snum);
```

考虑getline()字符串分隔符

```cpp
#include&lt;sstream&gt;
for(int i=0; i&lt;num; i&#43;&#43;){
    getline(fin,line);
    int line_pix_num=0;
    stringstream lines(line);
    cout&lt;&lt;line&lt;&lt;endl;
    string s;
    while(getline(lines,s,&#39;;&#39;)){
        stringstream  ss(s);
        string x_str, y_str;
        getline(ss,x_str,&#39;,&#39;);
        getline(ss,y_str,&#39;,&#39;);
        int x=stoi(x_str);
        int y=stoi(y_str);
    }
}
fin.close();
```

格式化输出

```cpp
#include&lt;iomanip&gt;
cout&lt;&lt;fixed&lt;&lt;setprecision(2)&lt;&lt;a; //保留两位小数
```



### 2012 大小转换

结构体创建时加struct 使用时不加

```cpp
struct poi{
	int x;int y;
};
poi a;
```



### 2014 重采样

读文件

```cpp
FILE* fp=NULL;
errno_t err;
fopen_s(&amp;fp,&#34;125,raw&#34;,&#34;r&#34;);
unsigned char img10m[125][125]={0};
if (err = fopen_s(&amp;fp, &#34;125.raw&#34;, &#34;r&#34;) != 0){
    cout &lt;&lt; &#34;无法打开125.raw&#34; &lt;&lt; endl;
    return 0;
}
fread(img10m,sizeof(unsigned char),125 * 125,fp);
fclose(fp);
```



### 2015 聚类

中心点初始化--&gt;算类别--&gt;新的中心--&gt;比较变化（）--&gt;赋值--&gt;停止与否

```cpp
//判断聚类中心是否不变
int judge_flag=true;
//聚类中心改变时进行循环
while (judge_flag){
    //点位确定类别
    for (int i=0;i&lt;100;i&#43;&#43;){
        for (int j=0;j&lt;100;j&#43;&#43;){
            int num_cluster=0;
            int distance=100010;//指定max值
            for(int m=0;m&lt;k;m&#43;&#43;){
                int dis=pow(center[m].x-pois[i][j].x,2)&#43;pow(center[m].y-pois[i][j].y,2);//使用距离进行聚类 可以更换
                // int dis=abs(center[m].gray-pois[i][j].gray); //使用灰度差进行聚类
                if (dis&lt;distance){//类别确定
                    distance=dis;
                    num_cluster=m;
                }
            }
            pois[i][j].cluster=num_cluster;//类别赋值
        }
    }
    // for (int i=0;i&lt;100;i&#43;&#43;){
    //     for (int j=0;j&lt;100;j&#43;&#43;){
    //         cout&lt;&lt;pois[i][j].cluster;//类别输出
    //     }
    // }
    //更新后中心点初始化
    for(int m=0;m&lt;k;m&#43;&#43;){
        center_new[m].x=0;
        center_new[m].y=0;
    }
    //更新类簇中心
    for(int m=0;m&lt;k;m&#43;&#43;){
        int px=0,py=0,num_single=0;
        for (int i=0;i&lt;100;i&#43;&#43;){
            for (int j=0;j&lt;100;j&#43;&#43;){
                if (pois[i][j].cluster==m){
                    center_new[m].x&#43;=pois[i][j].x;//累加距离
                    center_new[m].y&#43;=pois[i][j].y;
                    num_single&#43;&#43;;//计数
                }
            }
        }
        if(num_single==0) cout&lt;&lt;&#34;Error!!! &#34;,num_single=1;
        center_new[m].x/=num_single;//取平均值 得到 类簇中心坐标
        center_new[m].y/=num_single;
    }
    //比较中心点位变化判断迭代是否完成
    int judge_flag_num=0;
    for(int m=0;m&lt;k;m&#43;&#43;){
        if (center_new[m].x==center[m].x&amp;&amp;center_new[m].y==center[m].y){//逐中心点比较
            judge_flag_num&#43;&#43;;
        }
    }
    //中心赋值
    for(int m=0;m&lt;k;m&#43;&#43;){
        center[m].x=center_new[m].x;
        center[m].y=center_new[m].y;
    }
    if (judge_flag_num==k) judge_flag=false;//所有点位中心均不变 则 满足要求、停止迭代
}
```



### 2020最大攻击范围

最大最小位置

```cpp
#include&lt;algorithm&gt;
//有效位置范围
int *p_y=new int[n]();
int y_min=*min_element(p_y,p_y&#43;n&#43;1);
int y_max=*max_element(p_y,p_y&#43;n&#43;1);
```



### 2020 L的形状

以字符串格式读取数据

&gt;  字符-&#39;0&#39;即可转为int
&gt;
&gt;  二维vector 可以push_back()一维vector

```cpp
vector&lt;vector&lt;int&gt;&gt; data;
string sline;
while (getline(fin, sline)) {
    vector&lt;int&gt; data_line;
    for (char ch : sline) {
        data_line.push_back(ch - &#39;0&#39;);//转为int
    }
    data.push_back(data_line);
}
```

dfs递归判断

```cpp
int dfs(vector&lt;vector&lt;int&gt;&gt;&amp; a, int line, int col, int max_num_row, int max_num_col) {
    if (0 &lt;= line &amp;&amp; line &lt; max_num_row &amp;&amp; 0 &lt;= col &amp;&amp; col &lt; max_num_col &amp;&amp; a[line][col] == 1) {
        a[line][col] = -1;

        int dx[] = { 0, 1, 0, -1 };
        int dy[] = { 1, 0, -1, 0 };

        int k = 0;
        vector&lt;int&gt; s;
        for (int i = 0; i &lt; 4; i&#43;&#43;) {
            int g= dfs(a, line &#43; dx[i], col &#43; dy[i], max_num_row, max_num_col);
            k &#43;= g;
            if (g == 1) {
                s.push_back(dx[i]);
                s.push_back(dy[i]);
            }
        }
        if (k == 2) {
            if (s[0] != s[2] &amp;&amp; s[1]!=s[3]) {
                if (s[0] == 0) {
                    if (s[1] == 1)
                        cout &lt;&lt; &#34;1&#34;;
                    else
                        cout &lt;&lt; &#34;4&#34;;
                }
                else if (s[0] == 1)
                    cout &lt;&lt; &#34;3&#34;;
                else if (s[0] == -1)
                    cout &lt;&lt; &#34;4&#34;;
            }
        }
        return 1;
    }
    return 0; 
}
```



### 2021 迷魂阵

加入之前判断即可



### 2022 函数

迭代赋值即可



### 2023 水面积

深搜递归

```cpp
void dfs_water(char data[][100],int x,int y, int &amp;area){
    if(x&gt;=0 &amp;&amp; x&lt;100 &amp;&amp; y&gt;=0 &amp;&amp; y&lt;100 &amp;&amp; data[x][y]==&#39;W&#39;){
        data[x][y]=&#39;A&#39;; //标记已经访问过的
        area&#43;=1;
        int dx[8]={-1,-1,-1,0,0,1,1,1};
        int dy[8]={-1,0,1,-1,1,-1,0,1};
        for(int i=0;i&lt;8;i&#43;&#43;){
            dfs_water(data,x&#43;dx[i],y&#43;dy[i],area);
        }
    }
}
```



## 读写文件

头文件

```cpp
#include&lt;iostream&gt;
#include&lt;fstream&gt; //ifstream fin(&#34;a.txt&#34;); if(!fin.is_open()) cout&lt;&lt;&#34;&#34;; fin&gt;&gt;num;
#include&lt;sstream&gt; //stringstream ss(line); getline(ss,s,&#39;;&#39;);
#include&lt;cstring&gt; //string stoi atoi 
#include&lt;iomanip&gt; //&lt;&lt;fixed&lt;&lt;setprecision(2)&lt;&lt;格式化精度
```

读文本

```
ifstream fin(&#34;.txt or .dat&#34;);
fin&gt;&gt;num;
```

读二进制

```cpp
ifstream fin(&#34;.raw&#34;,ios::in | ios::binary)
if(!fin.is_open()){
	cout&lt;&lt;&#34;Error!&#34;;
	return 1;
}
fin.read((char*)(a),sizeof(data));
unsigned char data[256][256]={0};
         //单位置大小             //地址   //总大小
fin.read(reinterpred_cast&lt;char*&gt;(data),sizeof(data))
    
ofstream fout(&#34;.raw&#34;,ios::out | ios::binary);
if(!fout.is_open()){
    cout&lt;&lt;&#34;Error!&#34;;
    return 1;
}
fout.write(reinterpred_cast&lt;char*&gt;(data),sizeof(data))
```

复杂拆分

```cpp
while(getline(fin,line)){
	stringstream lines(line);
	string s;
	while(getline(lines,s,&#39;;&#39;)){
		stringstream ss(s);
		getline(ss,x_str,&#39;,&#39;);
	}
}
```



## Dijkstra or Prim

```cpp
int ini = 0;//起始点
dist[ini] = 0;
s[ini].push_back(ini);//由于下述为&#34;&gt;&#34;故需加入开始位置
for (int i = 0; i &lt; g.size(); i&#43;&#43;) {
    int t = -1;
    for (int j = 0; j &lt; g.size(); j&#43;&#43;) {//找最近点
        if (!st[j] &amp;&amp; (t == -1 || dist[j] &lt; dist[t]))
            t = j;
    }
    st[t] = true;//已找过
    for (int j = 0; j &lt; g.size(); j&#43;&#43;) {
        if (dist[j] &gt; dist[t] &#43; g[t][j]) {    
            dist[j] = dist[t] &#43; g[t][j];//更新最近距离
            s[j] = s[t];//记录路径
            s[j].push_back(j);
        }
    }
}
int dir = 4;
cout &lt;&lt; dist[dir]&lt;&lt;endl;
for (int i = 0; i &lt; s[dir].size(); i&#43;&#43;) {
    cout &lt;&lt; s[dir][i];
}
```

```cpp
#include &lt;iostream&gt;
#include &lt;cstring&gt;
#include &lt;algorithm&gt;
using namespace std;

const int N = 510;
int g[N][N];
int dt[N];//距离
int st[N];
int pre[N];//节点的前节点
int n, m;

void prim()
{
    memset(dt,0x3f, sizeof(dt));//初始化
    int res= 0;
    dt[1] = 0;//节点开始
    for(int i = 0; i &lt; n; i&#43;&#43;)//每次循环选一个点加入
    {
        int t = -1;
        for(int j = 1; j &lt;= n; j&#43;&#43;)
        {
            if(!st[j] &amp;&amp; (t == -1 || dt[j] &lt; dt[t]))
                t = j;
        }
        //孤立点
        if(dt[t] == 0x3f3f3f3f) {
            cout &lt;&lt; &#34;impossible&#34;;
            return;
        }
        st[t] = 1;
        res &#43;= dt[t];
        for(int i = 1; i &lt;= n; i&#43;&#43;)
        {
            if(dt[i] &gt; g[t][i] &amp;&amp; !st[i])
            {
                dt[i] = g[t][i];
                pre[i] = t;//i 的前驱变为 t.
            }
        }
    }

    cout &lt;&lt; res;

}

void getPath()//输出各边
{
    for(int i = n; i &gt; 1; i--)//n 个节点， n-1 条边

    {
        cout &lt;&lt; i &lt;&lt;&#34; &#34; &lt;&lt; pre[i] &lt;&lt; &#34; &#34;&lt;&lt; endl;// i 是节点编号，pre[i] 是 i 的前驱节点
    }
}

int main()
{
    memset(g, 0x3f, sizeof(g));//各个点之间的距离初始化成很大的数
    cin &gt;&gt; n &gt;&gt; m;//输入节点数和边数
    while(m --)
    {
        int a, b, w;
        cin &gt;&gt; a &gt;&gt; b &gt;&gt; w;//输出边的两个顶点和权重
        g[a][b] = g[b][a] = min(g[a][b],w);//存储权重
    }

    prim();//求最下生成树
    //getPath();//输出路径
    return 0;
}
```



## 递归排列

```cpp
#include&lt;iostream&gt;
#include&lt;cstring&gt;
using namespace std;

const int N = 10;//根据题目，最大数字是9
int q[N];//存储的是遍历到的数字
bool st[N];//判断数字是否被遍历
int n;//这里n一定要定义再全局变量里面，因为下面这个函数需要使用

//深度优先搜索遍历
void dfs(int u)
{
    //当遍历到最后一层的时候,先输出，再退出
    if(u == n)
    {
        for(int i = 0 ; i&lt;n ; i&#43;&#43;) cout&lt;&lt;q[i]&lt;&lt;&#34; &#34;;
        cout&lt;&lt;endl;
        return;//退出函数
    }

    for(int i = 1 ; i &lt;=n ; i&#43;&#43;)//按字典序遍历数字
        if(!st[i])//当这个数字不存在于q[]数组中,可以进入，否则继续遍历
        {
            q[u] = i;//存入数字
            st[i] = true;//先将数字进行标记
            dfs(u&#43;1);//进入下一层
            st[i] = false;//当从下一层出来的时候，这个数字也不再被标记
        }
}

int main()
{
    cin&gt;&gt;n;

    dfs(0);

    return 0;
}
```



# 基础

### 1.变量、输入输出、表达式、顺序语句

&gt; #include&lt;fstream&gt; 使用后须关闭，对同一文件，内存读取未关闭时存在调用顺序问题



### 2.判断语句

判断时的括号！！！

&gt; if(){
&gt;
&gt; }

善用大小比较及赋值

&gt; x&lt;y?x:y
&gt;
&gt; true?true:false
&gt;
&gt; false?false:true

平方表示

&gt; i^2&lt;=x
&gt;
&gt; i&lt;=x/i



### 3.循环语句

逗号表达式（只判断最后一个） 

&gt;  while (cin &gt;&gt; n &gt;&gt; m, n &gt; 0 &amp;&amp; m &gt; 0)
&gt;
&gt;  c&#43;&#43;中允许用逗号连接几个表达式，构成一个更大的表达式，逗号运算符的表达式如下：
&gt;  表达式1，表达式2···表达式n
&gt;  各个表达式的运算顺序是从左往右，最终整个表达式的值是“表达式n”的值

定大小输入

&gt; while(n--)

for 循环

&gt; 第二个；； 为条件判断，true or false 决定函数是否运行



### 4.数组

上下左右判断

&gt; i=j --&gt;  i&gt;j,i&lt;j
&gt;
&gt; i&#43;j=m --&gt; i&#43;j&lt;m,i&#43;j&gt;m

数组中心点 

&gt;  (n-1)/2.0
&gt;
&gt;  max(i-(n-1)/2.0,j-(n-1)/2.0)

最大最小值及其位置

&gt; #include&lt;algorithm&gt;
&gt;
&gt; int x[1010]=0;
&gt;
&gt; *min_element(x,x&#43;n); 
&gt;
&gt; min_element(x,x&#43;n)-x;

位运算($2^n$)

&gt; 1&lt;&lt; 除以2

位上是否有有效数据

&gt; if(a[x])



### 5.字符串

&gt; double类型可以 d&#43;&#43;;

string

&gt; #include&lt;cstring&gt;
&gt;
&gt; string s;        s可以当作字符串数组 直接cin会在空格or换行时停止
&gt;
&gt; getline(cin,s);
&gt;
&gt; s.size();
&gt;
&gt; s.insert(i,substr); 在i前插入substr
&gt;
&gt; s.find s.rfind 从前or后查找 第一个字符 出现的位置
&gt;
&gt;
&gt; a&gt;b return true 可直接比较大小
&gt;
&gt; &#39;A&#39;&#43;32=&#39;a&#39;
&gt;
&gt; &#39;a&#39;-0==97   字符转int可以直接减去0
&gt;
&gt; 
&gt;
&gt; string s1, s2;
&gt;
&gt; s1.find(s2);    
&gt; // 在 s1 中查找字符串 s2，找到返回 s2 首字母在字符串中的下标，找不到返回 -1
&gt;
&gt; s1.replace(pos, len, s2);
&gt; // 把 s1 中从下标 pos 开始的长度为 len 的子串替换为 s2
&gt;
&gt; s1.erase(it);    
&gt; // 把 s1 字符串中迭代器 it 处的字符删除
&gt;
&gt; s1.erase(pos, len);
&gt; // 把 s1 中从下标 pos 开始的长度为 len 的子串删除
&gt;
&gt; 
&gt;
&gt; str.substr(0,n); str.substr(n); 从0,n的拷贝，从n往后的拷贝







数组

&gt; strlen(a); 会去掉0值

“数字”的比较可以转作“字符”的比较，比较ascii码

&gt; a&gt;=&#39;0&#39;&amp;&amp;a&lt;=&#39;9&#39;表示a是数字

&gt; 字符 是 单引号！！！

整数相除转为浮点数可以

&gt; *1.0  注意运算顺序！！

排序后去重

&gt; a[i]!=a[i&#43;1] cout&lt;&lt;&#34;1&#34;;



### 6.函数

最大公约数  (最小公倍数=a*b/gcd(a,b))

&gt; int gcd(int a,int b){
&gt;
&gt; if(a%b==0) return b;
&gt;
&gt; else return gcd(b,a%b);
&gt;
&gt; }

斐波那契数列

&gt;int fib(int n){
&gt;
&gt;if(n&lt;=2) return 1;
&gt;
&gt;else return fib(n-1)&#43;fib(n-2);
&gt;
&gt;}

数组元素作为判断条件

&gt; void print(char a[])
&gt; {
&gt;  for (int i = 0; a[i]; i &#43;&#43; )
&gt;      cout &lt;&lt; a[i];
&gt; }

去重

&gt; 排序看相邻
&gt;
&gt; or
&gt;
&gt; 标志数组 judge[a[i]]=0 or 1;

走方格方案数

```
int gonm(int n,int m){
    if(m==0 &amp;&amp; n==0) return 1;
    if(n==0) return gonm(n,m-1);
    if(m==0) return gonm(n-1,m);
    return gonm(n-1,m)&#43;gonm(n,m-1);
}
```

全排列方案数

```
void dfs(int u){
    if(u==n){//结束位置
        for(int i = 0 ; i&lt;n ; i&#43;&#43;) cout&lt;&lt;q[i]&lt;&lt;&#34; &#34;;
        cout&lt;&lt;endl;
        return;//退出函数
    }
    for(int i=0;i&lt;n;i&#43;&#43;){
        if(!st[i]){
            q[u]=i&#43;1;//赋值
            st[i]=true;//已经选过
            dfs(u&#43;1);
            st[i]=false;//摘出
        }
    }
}
```

两个链表中第一个重复节点，双指针总长度

```
ListNode *p1 = headA;
        ListNode *p2 = headB;

        while (p1 != p2) {
            if(p1 != NULL)//p1没有走到结尾
                p1 = p1-&gt;next;//p1指向下一个节点
            else//p1走到结尾
                p1 = headB;//p1指向另一个链表头
            if(p2 != NULL)//p2没有走到结尾
                p2 = p2-&gt;next;//p2指向下一个节点
            else  //p2走到结尾 
                p2 = headA;//p2指向另一个链表头
        }
        return p1;
```



### 7.STL



## 小技巧

1&gt;2&gt;3&gt;4&gt;n&gt;0;

&gt; (i&#43;1)%n

对分隔符为满足要求可以 将所有单个元素加入分隔符，满足**同种格式**

&gt; 单词两边加空格



---

> 作者: [qiu](https://qiufenggit.github.io/)  
> URL: http://localhost:1313/posts/689dedf/  

