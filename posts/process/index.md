# Decoding Lab: Understanding a Secret Message解答（未完待续）


Decoding Lab: Understanding a Secret Message分析解答

未完待续

&lt;!--more--&gt;

## 分析

### 已给提示

1. 先解key1 key2 后解key3 key4（12解出前34无法解）
2. 如key1 key2解正确  则显示：~From:~ （仅用于检验key1 key2是否正确）
3. process_key12会改变dummy的值（因为statrt、stride值因dummy值而变）
4. 解key3 key4时调用process_key34而非process_key12，在process_key34中实现该过程

### key1&amp;key2

#### 函数主体

~~~c#
	int dummy = 1;
	int start, stride;
	int key1, key2, key3, key4;
	char* msg1, * msg2;

	key3 = key4 = 0;
	if (argc &lt; 3) {
		usage_and_exit(argv[0]);
	}
	key1 = strtol(argv[1], NULL, 0);
	key2 = strtol(argv[2], NULL, 0);
	if (argc &gt; 3) key3 = strtol(argv[3], NULL, 0);
	if (argc &gt; 4) key4 = strtol(argv[4], NULL, 0);

	process_keys12(&amp;key1, &amp;key2);

	start = (int)(*(((char*)&amp;dummy)));
	stride = (int)(*(((char*)&amp;dummy) &#43; 1));

	if (key3 != 0 &amp;&amp; key4 != 0) {
		process_keys34(&amp;key3, &amp;key4);
	}

	msg1 = extract_message1(start, stride);
~~~

#### 分析：

由`msg1 = extract_message1(start, stride);`有：smg1与start与stride相关（与提示相符）

~~~c#
start = (int)(*(((char*)&amp;dummy)));
stride = (int)(*(((char*)&amp;dummy) &#43; 1));
~~~

结合此处，可知dummy数据发生改变

由前文分析可知`process_keys12(&amp;key1, &amp;key2);`改变了dummy地址

~~~c#
void process_keys12(int* key1, int* key2) {

	*((int*)(key1 &#43; *key1)) = *key2;
}
~~~

则`*((int*)(key1 &#43; *key1))`=dummy=*key2

key1=&amp;dummy-&amp;key1

又有： 

~~~
int dummy = 1;
int start, stride;
int key1, key2, key3, key4;
~~~

则应有 **key1=3**

由前文知：key2=dummy

~~~c#
	msg1 = extract_message1(start, stride);

	char* extract_message1(int start, int stride) {
	int i, j, k;
	int done = 0;

	for (i = 0, j = start &#43; 1; !done; j&#43;&#43;) {
		for (k = 1; k &lt; stride; k&#43;&#43;, j&#43;&#43;, i&#43;&#43;) {

			if (*(((char*)data) &#43; j) == &#39;\0&#39;) {
				done = 1;
				break;
			}

			message[i] = *(((char*)data) &#43; j);
		}
	}
	message[i] = &#39;\0&#39;;
	return message;
}

~~~

分析此循环有：从data数组第start&#43;2个字符开始读取，每读取stride-1个字符即跳过一个字符

对

~~~
int data[] = {
	0x63636363, 0x63636363, 0x72464663, 0x6F6D6F72,
		0x466D203A, 0x65693A72, 0x43646E20, 0x6F54540A,
		0x5920453A, 0x54756F0A, 0x6F6F470A, 0x21643A6F,
		0x594E2020, 0x206F776F, 0x79727574, 0x4563200A,
		0x6F786F68, 0x6E696373, 0x6C206765, 0x796C656B,
		0x2C336573, 0x7420346E, 0x20216F74, 0x726F5966,
		0x7565636F, 0x20206120, 0x6C616763, 0x74206C6F,
		0x20206F74, 0x74786565, 0x65617276, 0x32727463,
		0x6E617920, 0x680A6474, 0x6F697661, 0x20646E69,
		0x21687467, 0x63002065, 0x6C6C7861, 0x78742078,
		0x6578206F, 0x72747878, 0x78636178, 0x00783174
};
~~~

数据进行输出分析有：

~~~
cccccccccFFrromo: mFr:ie ndC
TTo:E Y
ouT
Gooo:d!  NYowo tury
 cEhoxoscineg lkelyse3,n4 tto! fYoroceu a  cgalol tto  eextvraectr2 yantd
havioind gth!e caxllx txo xexxtrxacxt1x
~~~

要使得输出为：`From:`

需 从第11个开始 读2个隔1个

则**start=9 &amp; stride=3**

又有

~~~
start = (int)(*(((char*)&amp;dummy)));
stride = (int)(*(((char*)&amp;dummy) &#43; 1));
~~~

则dummy由int转为char类型后 第一个字节：09 第二个字节：03

当后面字节填充为0时有：**dummy=2^8^*3&#43;9=777=key2**


---

> 作者: [qiu](https://qiufenggit.github.io/)  
> URL: http://localhost:1313/posts/process/  

