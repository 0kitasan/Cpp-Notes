## 2-程序的基本组成
这部分以前写的，随便看看就行

#### 补码

在硬件层面，加法器比较容易实现。因此考虑“补码”操作，将减法运算通过某些比较容易实现的方式，转变为加法运算。

example：

```
62   #十进制
0 0111110  #62的二进制
1 1000001  #取反码
1 1000010  #加上1，得到-62的补码   
```

```
    0 0111110  (+62)
+   1 1000010  (-62) #符号位也参与运算
= 1 0 0000000        #最高位的1应被舍弃，因为是8位的表示，溢出了

### -61的补码为1 1000011
```

注意，4Bytes 表示一个 int（1 位为符号位），由于补码操作，其表示范围为 -2^31 ~ 2^31-1

#### 数据类型

##### 整型

|      类型      |        类型名        | 占用空间 |    表示范围    |
| :------------: | :------------------: | :------: | :------------: |
|      整型      |         int          |  4Bytes  | -2^31 ~ 2^31-1 |
|     短整型     |     short [int]      |  2Bytes  | -2^15 ~ 2^15-1 |
|     长整型     |      long [int]      |  4Bytes  | -2^31 ~ 2^31-1 |
| 无符号基本整型 |    unsigned [int]    |  4Bytes  |   0 ~ 2^32-1   |
|  无符号短整型  | unsigned short [int] |  2Bytes  |   0 ~ 2^16-1   |
|  无符号长整型  | unsigned long [int]  |  4Bytes  |   0 ~ 2^32-1   |

##### 实型

> a*2^b , //a 为尾数，b 为指数

|        类型        | C++ 标准 (有效数字) | 占用空间 (尾数/指数，均有符号位) | 大致表示范围（二进制） | 大致表示范围（十进制） | 精度（二进制/十进制） |
| :----------------: | :-----------------: | :------------------------------: | :--------------------: | :--------------------: | :-------------------: |
|    float/单精度    |    至少保证 6 位    |         4Bytes(24/8 位)          |    -2^-128 ~ 2^128     |    -10^-38 ~ 10^38     |        23/7 位        |
|   double/双精度    |   至少保证 10 位    |         8Bytes(53/11 位)         |   -2^-1024 ~ 2^1024    |   -10^-308 ~ 10^308    |       52/16 位        |
| long double/双精度 |   至少保证 10 位    |        16Bytes or 12Bytes        |           略           |           略           |          略           |

##### 字符型

```
char example='A';
```

| 转义序列 |                   功能                    |
| :------: | :---------------------------------------: |
|    \a    |               报警声 (响铃)               |
|    \0    |      空字符 (ASCII 代码为 0 的字符)       |
|   \\\    |               字符**\**本身               |
|   \\'    |  字符**'**(仅在**字符**常量中需要反斜杠)  |
|   \\"    | 字符**"**(仅在**字符串**常量中需要反斜杠) |

##### 枚举类型

```
enum 枚举类型名 {元素表};
```

#### 数据的输入/输出

```
cin.get(字符变量);
字符变量=cin.get();
cin.getline();
```

#### 算数运算

##### 强制类型转换

```
9.0/4
9/4.0
9.0/4.0
```


## 5-数组

### 一维数组

暂时先不写

### 排序与查找


#### 顺序查找

数组元素已按某一顺序排序

比较简单，先不写了

#### 二分查找

数组元素已按某一顺序排序

``` cpp
##include <iostream>
using namespace std;

int main() {
    int lh, rh, mid, x;
    int array[] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};

    cout << "请输入要查找的数据: "; 
    cin >> x;

    lh = 0;  // 左边界
    rh = 9;  // 右边界

    while (lh <= rh) {
        mid = (lh + rh) / 2;  // 计算中间位置
        if (x == array[mid]) {
            cout << x << " 的位置是: " << mid << endl;
            break;  // 找到元素，退出循环
        }
        if (x < array[mid]) {
            rh = mid - 1;  // 缩小右边界
        } else {
            lh = mid + 1;  // 增大左边界
        }
    }

    if (lh > rh) {
        cout << "没有找到" << endl;  // 如果 lh > rh，说明没有找到元素
    }

    return 0;
}
```


#### 选择排序

- 在所有元素中找到最小的元素放在数组的第 0 个位置
- 在剩余元素中找出最小的放在第一个位置。以此类推，直到所有元素都放在适当的位置

``` cpp
##include <iostream>
using namespace std;
int main()
{
    int lh, rh, k, tmp;
    int array[] = {2, 5, 1, 9, 10, 0, 4, 8, 7, 6};

    for (lh = 0; lh < 10; lh++)
    {
        rh = lh;
        for (k = lh; k < 10; ++k)
            if (array[k] < array[rh]) rh = k;
        tmp = array[lh];
        array[lh] = array[rh];
        array[rh] = tmp;
    }

    for (lh = 0; lh < 10; ++lh)
        cout << array[lh] << ' ';
    return 0;
}
```


#### 冒泡排序

- 将待冒泡的数据从头到尾依次处理：比较相邻的两个元素，如果大的在前小的在后，就交换这两个元素。这样经过从头到尾的检查，最大的一个就被交换到最后了。
- 如果在一次起泡中没有发生交换，则表示数据都已排好序，不需要再进行起泡。

``` cpp
##include <iostream>
using namespace std;

int main() {
  int a[] = {0, 3, 5, 1, 8, 7, 9, 4, 2, 10, 6};
  int i, j, tmp, n = 11;
  bool flag;
  for (i = 1; i < n; ++i) {
    flag = false;
    for (j = 0; j < n - i; ++j)
      if (a[j + 1] < a[j]) {
        tmp = a[j];
        a[j] = a[j + 1];
        a[j + 1] = tmp;
        flag = true;
      }
    if (!flag)
      break; // 一趟冒泡中没有发生交换，排序结束
  }
  cout << endl;
  for (i = 0; i < n; ++i)
    cout << a[i] << ' ';
  return 0;
}
```

#### 数组合并实例

- 把两个升序排列的整型数组 a 和 b 组合为一个升序数组 c。
- 设计合适的算法，以得到较高的运行效率。


``` cpp
##include <iostream>
using namespace std;

int main() {
  int a[4] = {1, 2, 5, 7};
  int b[8] = {3, 4, 8, 8, 9, 10, 11, 12};
  int c[12];
  int i, j, k;
  i = j = k = 0;

  while (i < 4 && j < 8) {
    if (a[i] > b[j]) // 当a[i]>b[j]，把b[i]写入数组c
    {
      c[k++] = b[j++];
    } else // 当a[i] <= b[j]，把a[i]写入数组c
    {
      c[k++] = a[i++];
    }
  }

  while (i < 4) {
    // 把数组a的剩余元素写入数组c
    c[k++] = a[i++];
  }
  while (j < 8) {
    // 把数组b的剩余元素写入数组c
    c[k++] = b[j++];
  }

  for (int m = 0; m < 12; m++)
    cout << c[m] << " ";
  cout << endl;

  return 0;
}
```



### 二维数组



### 字符串


#### 字符串的存储及初始化


``` cpp
char ch1[] = {'H', 'e', 'l', 'l', 'o', ',', ' ', 'w', 'o', 'r', 'l', 'd', '\0'};
char ch2[] = {"Hello, world"};
char ch3[] = "Hello, world";
```

注意：`'a'` 与 `"a"` 的区别（字符与数组的区别）

#### 字符串的输入输出

##### `cin` 和 `cout`

- `cin` 输入是以**空格、回车或 Tab 键**作为结束符。因此无法输入包含空白字符的字符串。
- 在用 `cin` 输入时，要注意**输入的字符串的长度不能超过数组的长度**。因此，最好在输出的提示信息中告知允许的最长字符串长度。

```cpp
char a[20];
cin >> a;
cout << a << endl;
// 输入：abc def (回车)
// 输出 ：abc
```


##### 函数 `getline`

如果要读取包含空格的一句话

```cpp
cin.getline(字符数组, 数组长度, 结束标记);
```

- 读取包含任意字符的字符串
- 直到**达到结束标记**或者**字符串读满**（包含 `'\0'`）

```cpp
char name[256];
cout << "Please input your name: ";
cin.getline(name, 256);
```


``` cpp
cin.getline(ch1, 80, '.');
cin.getline(ch2, 80);
// 输入：aaa bbb ccc.ddd eee fff ggg
// ch1的值：aaa bbb ccc
// ch2的值：ddd eee fff ggg
```

``` cpp
char m[20];
cin.getline(m, 5);
cout << m << endl;

// 输入 ：abcabcabc 输出 ：abca
// 输出 ：abca
// 接受5个字符到m中，其中最后一个为'\0'
// 所以只看到4个字符输出 
```

#### 字符串处理函数

C++ 的函数库中提供了一些用来处理字符串的函数。这些函数在库 cstring 中
（C++ 还提供了一个 string 类来处理字符串，貌似不考）

| 函数                   | 作用                                                    |
| -------------------- | ----------------------------------------------------- |
| strcpy(dst, src)     | 将字符从 src 拷贝到 dst。函数的返回值是 dst 的地址                      |
| strncpy(dst, src, n) | 至多从 src 拷贝 n 个字符到 dst。函数的返回值是 dst 的地址                 |
| strcat(dst, src)     | 将 src 接到 dst 后。函数的返回值是 dst 的地址                        |
| strncat(dst, src, n) | 从 src 至多取 n 个字符接到 dst 后。函数的返回值是 dst 的地址               |
| strlen(s)            | 返回 s 的长度，注意与 sizeof( ) 的区别                            |
| strcmp(s1, s2)       | 比较 s1 和 s2。如 s1 > s2 返回值为正数，s1=s1 返回值为 0，s1<s2 返回值为负数 |
| strncmp(s1, s2, n)   | 如 strcmp，但至多比较 n 个字符                                  |
| strchr(s, ch)        | 返回一个指向 s 中第一次出现 ch 的地址                                |
| strrchr(s, ch)       | 返回一个指向 s 中最后一次出现 ch 的地址                               |
| strstr(s1, s2)       | 返回一个指向 s1 中第一次出现 s2 的地址                               |

#### 字符串应用

输入一行文字，统计有多少个单词。单词和单词之间用空格分开。
- 解题关键：单词的数目可以由单词间的空格决定
- 解题思路：
  - 设置一个计数器 `num` 表示单词个数。开始时，`num=0`。
  - 从头到尾扫描字符串。如果发现：当前字符为非空格，而当前字符以前的字符是空格，则表示找到了一个新的单词，`num` 加 1。
  - 当整个字符串扫描结束后，`num` 中的值就是单词数。

```cpp
int main() {
    const int LEN = 80;
    // 细节：LEN + 1
    char sentence[LEN + 1], prev = ' ';  
    // prev 表示当前字符的前一个字符
    // 注意，prev 初始化时为空格
    int i, num = 0;

    cin.getline(sentence, LEN + 1);

    for (i = 0; sentence[i] != '\0'; ++i) {
        if (prev == ' ' && sentence[i] != ' ') ++num;
        prev = sentence[i];
    }

    cout << "单词个数为: " << num << endl;

    return 0;
}
```



## 6-函数
### 函数的用途与定义

- C++ 的函数只能有 **一个** 返回值，可以有 **多条** 返回语句。
- C++ 的返回值 **不能是数组**！
- 函数名是一个 **标识符**，符合标识符命名规范。命名规则：**首含关长大**。
- 函数名要有意义，函数名一般是一个 **动词短语**，表示函数的行为。

举例：判断闰年

``` cpp
bool IsLeapYear(int year)
{
    bool leapyear;
    leapyear = ((year % 4 == 0) && (year % 100 != 0)) || (year % 400 == 0);
    return leapyear;
}
```

### 函数的使用

#### 函数的声明

**所有函数在使用前必须被声明**，以便让编译器知道用户的用法是否正确。

函数声明包括三个内容 (与函数头相似)：
* 函数名
* 参数的个数和类型
* 函数的返回类型

函数的声明被称为**函数的原型**，它的形式为：

```
返回类型 函数名（参数表）；
```

参数表中的每个参数说明可以是类型，也可以是类型后面再接一个参数名，如：

```cpp
int max(int, int);
int max(int a, int b);
```

#### 函数的调用

形参/实参

值传递/引用传递

函数调用执行过程：
* 在主程序中计算每个实际参数值；
* 用实际参数值初始化形式参数，如类型不一致则自动转换；
* 依次执行函数体的每个语句，直到遇见 return 语句或函数体结束；
* 计算 return 后面的表达式的值，如类型不一致则自动转换；
* 回到调用函数，用返回值置换函数调用，继续主程序的执行。

**帧 (frame)**：**栈 (stack)** 中用来存储函数的形式参数和函数内定义的变量。

执行函数调用：
- 现场保护，并为函数内的局部变量（包括形参）分配内存
- 把实参值复制一份给形参，单向传值（实参→形参）
- 实参与形参的数目、类型和顺序要一致

从函数退出时：
- 返回到本次函数调用的地方
- 把函数值返回给主调函数，同时把控制权还给调用者
- 收回分配给函数内所有变量（包括形参）的内存


### 变量的作用域

一个变量能被**存取**的程序部分，称为**变量的作用域**。

局部变量（Local Variable）
- 在语句块内（函数、复合语句）定义的变量
- `main()` 函数中说明的变量也是局部变量

全局变量（Global Variable）
- 在所有函数之外定义的变量
- 作用范围：从定义位置到文件结束
- 作用：方便函数间的数据传递
- 如在作用范围外的函数要使用此变量，用关键词 `extern` 在函数内说明此全局变量。

补充：
- 在块中说明的变量是**局部的**，仅能在本块中和内部的块中存取。
- 当内部块与外部块有**同名变量**时，在内部块中屏蔽外部块的同名变量。
- 在一个函数中，我们**不能存取主调函数**的变量，即使知道该变量的名字。
- 函数**参数**对该函数也是**局部的**，可以将它看成在块内，即函数体内说明的变量。

#### 局部变量的作用域

仅能在定义它的语句块（包括其下级语句块）内访问：

``` cpp
int main()
{
    int sum = 0, m;
    // int i 的作用域--开始
    for (int i = 0; i < 5; i++)
    {
        cout << "input a number:";
        cin >> m;
        sum = sum + m;
    }
    // int i 的作用域--结束
    cout << "sum = " << sum << endl;
    return 0;
}
```

#### 全局变量的作用域

全局变量的作用范围： 从定义变量的位置开始，到本程序结束.

#### 假如变量同名

局部变量与全局变量同名：局部变量隐藏全局变量，互不干扰

``` cpp
void Function();
int x = 1; int y = 2;

int main()
{
    cout <<"x="<<x<<"  y=" <<y<<endl;
    Function();
    return 0;
}

void Function()
{
    int x = 2, y = 1;
    cout <<"x="<<x<<"  y=" <<y<<endl;
}

/*
输出：
x=1  y=2
x=2  y=1
*/

```

形参与全局变量同名：局部变量隐藏全局变量，互不干扰

实际参数与形式参数可以同名：形参值改变不影响与其同名的实参值

形式参数不可以和局部变量同名

#### 局部变量和全局变量的实例

``` cpp
##include <iostream>
using namespace std;

int p = 1, q = 5, r = 3;
void f1();
void f2();
void f3();

int main() {
  f3();
  cout << "after f3: p,q,r=" << p << " " << q << " " << r << endl; // 1 5 6
  f1();
  cout << "after f1: p,q,r=" << p << " " << q << " " << r << endl; // 1 10 6
  f2();
  cout << "after f2: p,q,r=" << p << " " << q << " " << r << endl; // 17 10 6
  return 0;
}

void f1() {
  int p = 3, r = 2;
  q = p + q + r;                                             // q=10
  cout << "f1: p,q,r=" << p << " " << q << " " << r << endl; // 3 10 2
}

void f2() {
  p = p + q + r;                                             // p=17
  cout << "f2: p,q,r=" << p << " " << q << " " << r << endl; // 17 10 6
}

void f3() {
  int q;
  r = 2 * r;                                                 // r=6
  q = r + p;                                                 // q(局部)=7
  cout << "f3: p,q,r=" << p << " " << q << " " << r << endl; // 1 7 6
}
```

### 变量的存储类别

- 在 C++ 语言中，每个变量有两个**属性**：
    - **数据类型**：变量所存储的数据类型
    - **存储类别**：变量的存储位置和期限
- 标准的变量定义：
    - `存储类别 数据类型 变量名;`
- 存储类别：
    - 自动变量：`auto` （动态存储）
    - 静态变量：`static` （静态存储）
    - 寄存器变量：`register` （动态存储）
    - 外部变量：`extern` （静态存储）

##### auto

C++11 中 auto 被用于自动推断被定义变量类型， 自动变量无需再用 auto 说明。

##### static

- **静态变量**: 在整个程序的运行期间都存在的变量
    - `static int i;`
- **特点**:
    - 从程序运行起占据内存，程序退出时释放内存
    - 离开函数，值仍保留
- **两类静态变量**:
    - 静态的局部变量
    - 静态的全局变量
	- 生存期相同，作用域不同（语句块内，文件内）

``` cpp
##include <iostream>
using namespace std;

long Func(int n) {
  static long p = 1;
  // 用于声明和初始化静态变量p，只会运行一次
  // p只能在该函数中被调用，无法在main中直接调用
  p = p * n;
  return p;
}

int main() {
  int i, n;
  cout << "Input n:";
  cin >> n;
  for (i = 1; i <= n; i++) {
    cout << "Func(" << i << ")=" << Func(i) << endl;
  }
  return 0;
}
```

- **未被程序员初始化的静态变量都由系统初始化为 0**。
- **局部静态变量**:
    - 初始值是编译时赋的。当运行时重复调用此函数时，不重复赋初值。
    - 虽然局部静态变量在函数调用结束后仍然存在，但其他函数不能引用它。
    - 在程序执行结束时消亡。
- **静态的全局变量**:
    - 用 `static` 说明的全局变量其他源文件不能用 `extern` 引用它。
    - 用 `static` 还可以用在函数定义或说明中。该函数只能被用在本源文件中，其他源文件不能调用此函数。

##### register

不太考，先不写了

##### extern

声明一个不在本模块作用范围内的全局变量，例如：

``` cpp
extern int num;
// num 为一个全局变量。
```

**在某函数中引用了一个声明在本函数后的全局变量时**，需要在函数内用 `extern` 声明此全局变量。

``` cpp
##include <iostream>
using namespace std;

void a() {
  int x = 25; // 局部变量定义
  cout << "x in a is " << x << endl;
}

void b() {
  static int x = 50; // 静态局部变量定义
  cout << "x in b is " << x++ << endl;
}

void c() {
  extern int x; // 引用全局变量 x
  x = 10;
  cout << "x in c is " << x << endl;
}

int x = 2; // 全局变量定义

void d(int x) { cout << "x in d is " << ++x << endl; }

int main() {
  int x = 6; // 局部变量定义
  cout << "x in main is " << x << endl;

  a();b();c();d(x);x++;

  a();b();c();d(x);

  cout << "x in main is " << x << endl;
  return 0;
}
```

**当一个程序有多个源文件组成时**，用 `extern` 可引用另一文件中的全局变量。

``` cpp
// file1.cpp
##include <iostream>
using namespace std;
// f函数的声明
void f();
// 声明外部变量x，表示x在其他地方被定义。
extern int x; 

int main() {
  f();
  cout << "in main(): x= " << x << endl;
  return 0;
}
```

``` cpp
// file2.cpp
##include <iostream>
using namespace std;

int x; // 全局变量的定义
// f函数的定义
void f() {
  cout << "in f(): x= " << x << endl;
}
```

```
g++ file1.cpp file2.cpp -o myProgram
```


### 函数参数 -- 数组和默认值

#### 数组作为函数的参数

#### 默认值



### 内联、重载、模板、递归

#### 内联

函数调用存在开销，需要创建函数的**帧**。

在调用内联函数时，编译器直接用内联函数的代码替换函数调用，于是省去了函数调用的开销。

内联函数的定义：在函数头部前加保留词 inline

``` cpp
##include <iostream>
using namespace std;

inline float cube(float s) {
    return s * s * s;
}

int main() {
    float side;
    cin >> side;
    cout << cube(side) << endl;
    return 0;
}

```


#### 重载

- 使**参数个数不同、参数类型不同或两者兼而有之**的两个以上的函数取相同的函数名。
- 函数重载的目的是提高函数的易用性。

``` cpp
int max(int a1, int a2);
int max(int a1, int a2, int a3);
int max(int a1, int a2, int a3, int a4);
int max(int a1, int a2, int a3, int a4, int a5);
```

函数重载的实现
- 由编译器确定某一次函数调用到底是调用了哪一个具体的函数。这个过程称之为**绑定**（binding，又称为**联编**或**捆绑**）。

如何绑定?
- 编译器首先会为这一组重载函数中的每个函数取一个**不同的内部名字**。
- 当发生函数调用时，编译根据实际参数和形式参数的**匹配情况**确定具体调用的是那个函数，将这个函数的内部函数名取代重载的函数名。

#### 模板

在函数调用时，编译器根据实际参数的类型确定模板参数的值，生成不同的**模板函数**。

``` cpp
##include <iostream>
using namespace std;

template<class T>
T custom_max(T a, T b)
{
	// std中含有max，必须选择合适的名称，不然会引起冲突
    return a > b ? a : b;
}

int main() {
    // 整数类型调用
    int intResult = custom_max(3, 7);
    cout << "Max of 3 and 7 is: " << intResult << endl;

    // 浮点数类型调用
    double doubleResult = custom_max(3.5, 4.6);
    cout << "Max of 3.5 and 4.6 is: " << doubleResult << endl;

    // 字符类型调用
    char charResult = custom_max('z', 'a');
    cout << "Max of 'z' and 'a' is: " << charResult << endl;

    return 0;
}
```

一个细节：`class` 和 `typename`

``` cpp
template<class T>
void function(T param) {
    // ...
}

template<typename T>
void function(T param) {
    // ...
}
```

函数模板的**实例化**：
- 根据实际参数确定模板参数的值；
- 将模板参数的值代入函数模板，形成一个真正的函数。

#### 递归

##### 求解 n!

``` cpp
int p(int n)
{
    if (n == 0)
        return 1;
    else
        return n * p(n - 1);
}
```

##### Hannoi 塔

```
A (从大到小放置盘子)
B (目标位置)
C (辅助杆)
```

该问题只能用递归来解决。递归的步骤如下：

1. 将 `n-1` 个盘子从 A 移动到 C，使用 B 作为辅助柱。
2. 将第 `n` 个盘子（即最下面的盘子）从 A 移动到 B。
3. 将 `n-1` 个盘子从 C 移动到 B，使用 A 作为辅助柱。

``` cpp
##include <iostream>
using namespace std;

// 递归函数实现汉诺塔问题
void Hanoi(int n, char start, char finish, char temp) {
  // 基本情况：只有一个盘子，直接移动
  if (n == 1) {
    cout << start << " -> " << finish << '\t';
  }
  // 递归情况：多个盘子
  else {
    // 将n-1个盘子从start移动到temp，以finish作为辅助柱
    Hanoi(n - 1, start, temp, finish);
    // 将第n个盘子从start移动到finish
    cout << start << " -> " << finish << '\t';
    // 将n-1个盘子从temp移动到finish，以start作为辅助柱
    Hanoi(n - 1, temp, finish, start);
  }
}

// 主函数
int main() {
  int n = 20; // 盘子的数量：64个根本运行不完，30个都要运行好久
  Hanoi(n, 'A', 'B', 'C');
  cout << endl;
  return 0;
}
```



## 7-指针
### 指针概念及指针变量的定义

#### 指针运算

#### 指针与数组

#### 动态内存分配

#### 指针运算

#### 字符串与指针

字符串的常用表示是指向字符的指针，用法：

1、字符串常量的内存地址赋值给指针：

```cpp
char *String;
String = "abcde";
```

1. **字符串常量存储在内存中的数据区（只读）**：
   - 字符串常量（例如 `"abcde"`）在程序执行时存储在数据区或代码区，这些区域通常是只读的。
2. **将存储字符串 `"abcde"` 的内存的首地址赋给指针变量 `String`**：
   - 语句 `String = "abcde";` 将字符串 `"abcde"` 的首地址赋值给指针变量 `String`。因此，`String` 指向只读的字符串常量。
3. **内存布局**：
   - 图示展示了字符串常量 `"abcde"` 存储在数据区或代码区，并且 `String` 指针指向该字符串的首地址。
4. **注意事项**：
   - **不能将 `String` 作为 `strcpy` 或 `strcat` 的第一个参数**：
     - 因为 `String` 指向的是一个只读的字符串常量区，试图修改它会导致未定义行为（通常会导致程序崩溃）。
   - **可以通过下标读取字符串中的字符**：
     - 例如，`String[3]` 的值是 `'d'`，这是合法的读取操作。
   - **不可以通过下标变量赋值**：
     - 例如，`String[3] = 'w'` 是错误的，因为它试图修改只读内存区域的内容。

| 操作系统 |        |
| -------- | ------ |
| 程序代码 |        |
| "abcde"  | 数据段 |
| String   | 栈     |
|          | 堆     |

2、将一个字符数组名赋给一个指针，字符数组中存储的是一个字符串：

```cpp
char *String, ss[] = "ABCD";
String = ss;
```

将字符数组 ss 的起始地址存入 String

| 操作系统                  |        |
| :------------------------ | ------ |
| 程序代码                  |        |
|                           | 数据段 |
| String<br />ss[]="ABCD\0" | 栈     |
|                           | 堆     |

3、申请一个动态的字符数组赋给一个指向字符的指针，字符串存储在动态数组中

```cpp
char *String;
String = new char[10];
strcpy(String, "abc");
```

- string 在栈工作区
- 字符串存储在堆工作区
- string 记录了字符串”aaa”的内存的首地址

| 操作系统 |        |
| -------- | ------ |
| 程序代码 |        |
|          | 数据段 |
| String   | 栈     |
| "aaa\0"  | 堆     |

#### const 与指针

```cpp
// 不能通过 p 修改它指向的单元的内容，但 p 可以指向不同的变量。
const int *p;

// 可以通过 p 修改指向的对象，但 p 的值不能变。
// 即 p 永远只能指向 x ，p 定义时必须给出初值。
int *const p = &x;

// p 的值不能修改，*p 的值也不能修改
const int *const p = &x;
```

### 指针与函数

#### 指针作为函数参数

#### 返回指针的函数

### 引用

##### 引用概念

##### 返回引用的函数

函数调用是 return 后面的变量的别名，用途：

- 将函数用于赋值运算符的左边，即作为**左值**。
- 减少函数返回时的开销。

```cpp
int a[] = {1, 3, 5, 7, 9};

// 声明返回引用的函数
int& index(int);

void main() {
    index(2) = 25; // 将 a[2] 重新赋值为 25
    cout << index(2);
    // 其实质与 int& index(2)=a[2] 类似
}

int& index(int j) {
    return a[j];  // 函数是 a[j] 的一个引用
}
```

##### const 与引用

定义格式：`const 类型 &变量名 = 变量名；`

```cpp
int i;
const int &j = i;
```

常量引用的意义：**控制变量的修改**，不管被引用对象是常量还是变量，用此名字是不能赋值的

```cpp
int i = 5;    // 合法，直接修改变量 i
const int &j = i;
j = 5;       // 非法，通过常量引用 j 尝试修改变量 i
```

 `const` 引用的初值可以是常量或表达式。如：

```cpp
const int &y = 2 + 5;
```

返回常量引用的函数：仅减少函数返回时的开销，保证返回变量不被修改

```cpp
int a[] = {1, 3, 5, 7, 9};

// 声明返回引用的函数
const int& index(int);

void main() {
    index(2) = 25; // 错误！！！
    cout << index(2);
    // 正确，根据索引返回数组的对应元素
}

const int& index(int j) {
    return a[j];  // 函数是 a[j] 的一个引用
}
```

### 高级指针

#### 指针数组

每个元素均为指针的数组

##### 指针数组的应用

##### main 函数参数实例

#### 多级指针

指向指针的指针

```cpp
##include <iostream>
using namespace std;

int main()
{
    char *city[] = {"aaa", "bbb", "ccc", "ddd", "eee"};
    char **p;

    for (p = city; p < city + 5; ++p)
        cout << *p << endl;

    return 0;
}
```

输出结果

```
aaa
bbb
ccc
ddd
eee
```

#### 动态二维数组

C++ 不能直接申请动态二维数组，如 `new int[5][9]` 是非法的表达式

解决方案：利用数组和指针的关系

具体过程：

- 数组名 `a` 用二级指针表示
- `a` 数组是一个指针数组
- 每个 `a[i]` 指向一个一维数组
- 可以用 `a[i][j]` 访问

| a-> | a[0] | 1   | 2   | 3   | 4   |
| --- | ---- | --- | --- | --- | --- |
|     | a[1] | 5   | 6   | 7   | 8   |
|     | a[2] | 9   | 10  | 11  | 12  |

```cpp
int main()
{
    int **a, i, j, k = 0;

    a = new int*[3];                // 申请指针数组
    for (i = 0; i < 3; ++i)
        a[i] = new int[4];          // 申请每一行空间
    for (i = 0; i < 3; ++i)         // 二维数组赋值
        for (j = 0; j < 4; ++j)
            a[i][j] = k++;
    for (i = 0; i < 3; ++i) {       // 二维数组输出
        cout << endl;
        for (j = 0; j < 4; ++j)
            cout << a[i][j] << '\t';
    }
    for (i = 0; i < 3; ++i)         // 二维数组空间释放
        delete[] a[i];
    delete[] a;
    return 0;
}
```

#### 函数指针

```cpp
##include <iostream>
using namespace std;

void add(), erase(), modify(), printSalary(), printReport();

int main() {
  int select;
  void (*func[6])() = {NULL, add, erase, modify, printSalary, printReport};

  while (1) {
    cout << "1--add\n";
    cout << "2--delete\n";
    cout << "3--modify\n";
    cout << "4--print salary\n";
    cout << "5--print report\n";
    cout << "0--quit\n";
    cin >> select;

    if (select == 0)
      return 0;
    if (select > 5)
      cout << "input error\n";
    else
      func[select]();
  }
}
```


## 8-结构体

### 结构体的概述


### 结构体类型的定义


### 结构体类型的变量


### 结构体数组



### 结构体的应用：链表


``` cpp
ListNode *reverseList(ListNode *head) {
  ListNode *prev = nullptr; // 初始化前一个节点为 null
  ListNode *curr = head;    // 当前节点初始化为头节点

  while (curr != nullptr) {
    ListNode *next = curr->next; // 暂存当前节点的下一个节点
    curr->next = prev; // 当前节点的下一个节点指向前一个节点
    prev = curr;       // 将前一个节点移到当前节点
    curr = next;       // 当前节点移到下一个节点
  }

  return prev; // 最终 prev 是新的头节点
}
```



## 9-模块化开发






## 10-类
### 新的数据类型

改进后的 Array 库的头文件：封装在结构体中

``` cpp
##ifndef _array_h
##define _array_h

struct DoubleArray {
    int low;
    int high;
    double *storage;

    bool initialize(int lh, int rh);
    bool insert(int index, double value);
    bool fetch(int index, double &value);
    void cleanup();
};

// 函数名前要加限定
bool DoubleArray::initialize(int lh, int rh)
{
    low = lh;
    high = rh;
    storage = new double[high - low + 1];
    if (storage == NULL) return false;
    else return true;
}

##endif
```

### 类（类型）的定义

私有成员 (private)：只能由类的成员函数调用
公有成员 (public)：外部函数可以调用，类对外的接口
私有成员被封装在一个类中，类的用户是看不见的

如不加访问权限，class 默认私有； struct 默认公有

### 对象的使用

对象赋值语句
同类的对象之间可以互相赋值。例如，假设有两个有理数对象 `r1` 和 `r2`，如果它们属于同一个类，则可以执行以下赋值操作：

``` cpp
r1 = r2;
```

当一个对象赋值给另一个对象时，所有的数据成员都会**逐位拷贝**。上述赋值操作相当于逐个成员变量进行赋值。例如，有理数对象 `r1` 和 `r2` 有成员变量 `num` 和 `den`，那么赋值操作相当于：

``` cpp
r1.num = r2.num;
r1.den = r2.den;
```

逐位拷贝：
``` cpp
class A {
public:
    int a;
    int b[5];
};

A x, y;

// 设置对象 x 的成员变量
x.a = 10;
for (int i = 0; i < 5; i++) {
    x.b[i] = i;
}

// 将对象 x 赋值给对象 y
y = x;
```


### 对象的构造与析构

``` cpp
class DoubleArray {
private:
    int low;
    int high;
    double* storage;
	
public:
    // 构造函数
    DoubleArray(int lh, int rh) : low(lh), high(rh) {
        storage = new double[high - low + 1];
    }
	// 设置数组元素的值
    bool set(int index, double value);
    // 取数据元素的值，把结果存储在value中
    bool fetch(int index, double &value);
    // 析构函数
    ~DoubleArray() {
        delete[] storage; // 释放动态分配的内存
    }
};
```

#### 对象的构造

一旦有自定义构造函数，系统缺省构造函数消失

动态变量初始化在类型后面用圆括号指出实参表，例如：
``` cpp
p = new DoubleArray(20, 30);
```

初始化列表方法：
``` cpp
DoubleArray::DoubleArray(int lh, int rh) : low(lh), high(rh)
{
    storage = new double[high - low + 1];
}
```

构造函数可以重载，导致对象可以有多种方式构造

#### 对象的析构

并非每个类都必须要有析构函数，如 Rational 类就不需要，取决于是否有善后工作。

一般在构造函数中有动态申请内存的，必须有析构函数， 由它释放内存。

#### 拷贝构造函数

- 创建对象时，可用一个同类的对象对其初始化。
- 需用一个特殊的构造函数：**拷贝（复制）构造函数**。
- 拷贝构造函数以**同类对象引用作为参数**：

``` cpp
ClassName (const ClassName &)；
```

缺省的拷贝构造函数：如果用户未定义拷贝构造函数，系统会定义一个缺省的拷贝构造函数。该函数将已存在的对象原式原样地复制给新成员

```cpp
class point
{
    int x, y;
public:
    point(int a, int b) { x = a; y = b; }
    point(const point &p) { x = 2 * p.x; y = 2 * p.y; }
}
```


#### 对象的生命周期


``` cpp
class Time {
  //...
};

Time gTime;
int main() {
  Time lTime1;
  static Time sTime;
  Time lTime2;
}
```

创建顺序：遇到变量定义时调用构造函数

消失顺序：
- 局部变量先消失，然后是静态局部变量,，最后是全局变量
- 后创建的先消失

实例：

``` cpp
##include <iostream>
using namespace std;

class CreateAndDestroy {
public:
  CreateAndDestroy(int ID) {
    objectID = ID;
    cout << "Object " << objectID << " constructor runs " << endl;
  }
  ~CreateAndDestroy() {
    cout << "Object " << objectID << " destructor runs " << endl;
  };

private:
  int objectID;
};

void create(void) {
  cout << "\nCREATE FUNCTION: EXECUTION BEGINS " << endl;
  CreateAndDestroy fifth(5);
  static CreateAndDestroy sixth(6);
  CreateAndDestroy seventh(7);
  cout << "\nCREATE FUNCTION: EXECUTION ENDS " << endl;
}

CreateAndDestroy first(1);
int main() {
  cout << "\nMAIN FUNCTION: EXECUTION BEGINS" << endl;
  CreateAndDestroy second(2);
  static CreateAndDestroy third(3);
  create();
  cout << "\nMAIN FUNCTION: EXECUTION RESUMES" << endl;
  CreateAndDestroy fourth(4);
  cout << "\nMAIN FUNCTION: EXECUTION ENDS" << endl;
  return 0;
}
```

运行结果：

```
Object 1 constructor runs

MAIN FUNCTION: EXECUTION BEGINS
Object 2 constructor runs
Object 3 constructor runs

CREATE FUNCTION: EXECUTION BEGINS
Object 5 constructor runs
Object 6 constructor runs
Object 7 constructor runs

CREATE FUNCTION: EXECUTION ENDS
Object 7 destructor runs
Object 5 destructor runs

MAIN FUNCTION: EXECUTION RESUMES
Object 4 constructor runs

MAIN FUNCTION: EXECUTION ENDS
Object 4 destructor runs
Object 2 destructor runs
Object 6 destructor runs
Object 3 destructor runs
Object 1 destructor runs
```

### 常量对象与 const 成员函数

#### 常量对象

类比固有数据类型的常量 `const double PI = 3.14`，也可以有常量对象，即 const 对象。

const 对象的定义：

``` cpp
const MyClass obj (参数表) ; // 初值哪里来？
```

如何保证数据成员不被修改：

- 数据成员保护起来了，一般都由成员函数修改。
- 常量对象不能调用一般的成员函数。
- C++ 规定：对 `const` 对象只能调用 `const` 成员函数。

**提问：成员函数参数表后加 `const` 的原则？**

#### const 成员函数

- 不修改数据成员的函数都应声明为 `const` 类型。
- 在编写 `const` 成员函数时，如果修改了**数据成员**，或者调用了**其他非 `const` 成员函数**，编译器将指出错误，提高程序的健壮性。

``` cpp
class A {
    int x;
public:
    A(int i) { x = i; }
    int getx() const { return x; } // 声明与定义
    void disp() { cout << x << endl; }
};
```

``` cpp
class A {
    int x;
public:
    A(int i) { x = i; }
    int getx() const ; // 仅声明
    void disp() { cout << x << endl; }
};
int A::getx() const {return x;} // 必须加const，否则视为不同函数
```

就算成员函数不修改数据类型，只要是没有声明为 `const` 类型，const 对象就无法调用该函数

``` cpp
void main() {
    A a(10); 
    const A b(20);

    cout << a.getx() << "<" << b.getx() << endl;
    a.disp();
    b.disp();  // 错！！！

}
```

### 常量数据成员

- `const` 数据成员只在某个对象生存期内是常量，对于整个类而言是可变的。
- 同一类的不同的对象其 `const` 数据成员的值可以不同。又称 “对象一级的常量”。
- 常量成员的声明：在该成员声明前加上 `const`，如：

``` cpp
class abc {
	const int x;
	...
};
```

- 不能在类声明中初始化 `const` 数据成员。**原因：对象一级的常量！**

``` cpp
class A {
    // 错误，企图在类声明中初始化const数据成员
    const int SIZE = 200;
    int array[SIZE];  // 错误，未知的SIZE
};
```

- `const` 数据成员的初始化只能在类构造函数的**初始化表**中进行，**不能在构造函数中对它们赋值。**

``` cpp
class A {
    const int SIZE;
public:
    A(int size) : SIZE(size) {} // 构造函数的初始化表
    ...
};

A a(100); // 对象 a 的 SIZE 的值为 100
A b(200); // 对象 b 的 SIZE 的值为 200

// b 和 a 中 SIZE 不同
```

原因：**初始化列表是在构造函数体执行之前被处理的**，这是唯一能够确保 `const` 数据成员在对象创建时被正确初始化的地方。如果尝试在构造函数体内赋值，将违反 `const` 数据成员不可变的规则。

### 静态数据成员与静态成员函数

#### 静态数据成员

``` cpp
class SavingAccount {
private:
    char m_name[20];   // 存户姓名
    char m_addr[60];   // 存户地址
    double m_total;    // 存款额
    static double m_rate; // 利率
```

- 静态成员变量属于类，而不是对象。
- 静态成员变量的初始化不能放在构造函数中，必须在类定义外进行。
- 类的定义并不分配内存，只有在创建对象实例时才分配内存。
- 静态成员变量在类定义外初始化，不依赖于对象的创建。

``` cpp
// 静态成员变量的定义和初始化，此时才分配空间
double SavingAccount::m_rate = 0.05; // 初始化为 5%
```


#### 静态成员函数

- 静态成员函数可定义为内嵌的，也可在类外定义。**在类外定义时，不用 static。**
- 静态成员函数的访问：可以通过类作用域限定符或通过对象访问

``` cpp
class goods {
private:
  int weight;              // 单个货物的重量
  static int total_weight; // 所有货物的总重量

public:
  // 构造函数
  goods(int w) {
    weight = w;
    total_weight += w;
  }

  // 析构函数
  ~goods() { total_weight -= weight; }

  // 返回单个货物的重量
  int getWeight() const { return weight; }

  // 返回所有货物的总重量
  static int getTotalWeight() { return total_weight; }
};

// 静态成员变量的定义和初始化
int goods::total_weight = 0;

// 注意：如果写成 int total_weight = 0 是会链接错误的，ppt上疑似打错了

```

#### 完整实例

``` cpp
##include <cstring>
##include <iostream>
using namespace std;

class SavingAccount {
private:
  char m_name[20]; // 存户姓名
  char m_addr[60]; // 存户地址
  double m_total;  // 存款额

public:
  static double m_rate; // 利率

  // 构造函数，其他成员函数省略...
  SavingAccount(const char *name, const char *addr, double total) {
    strncpy(m_name, name, 20);
    strncpy(m_addr, addr, 60);
    m_total = total;
  }

  // 静态成员函数来设置和获取利率
  static void setRate(double rate) { m_rate = rate; }

  static double getRate() { return m_rate; }

  // 成员函数来显示账户信息
  void display() const {
    cout << "Name: " << m_name << endl;
    cout << "Address: " << m_addr << endl;
    cout << "Total: " << m_total << endl;
    cout << "Rate: " << m_rate << endl;
  }
};

// 静态成员变量的定义和初始化
double SavingAccount::m_rate = 0.05; // 初始化为 5%

int main() {
  // 通过类直接访问静态成员变量
  // 注意：如果在private中，则不能访问
  cout << "Initial rate: " << SavingAccount::m_rate << endl;

  // 创建对象
  SavingAccount account1("Alice", "123 Main St", 1000.0);
  SavingAccount account2("Bob", "456 Elm St", 2000.0);

  // 通过对象访问静态成员变量
  cout << "Account1 initial rate: " << account1.getRate() << endl;
  cout << "Account2 initial rate: " << account2.getRate() << endl;

  // 修改利率
  SavingAccount::setRate(0.03); // 修改为 3%

  // 通过静态成员函数访问静态成员变量
  cout << "Modified rate: " << SavingAccount::getRate() << endl;

  // 再次通过对象访问静态成员变量
  cout << "Account1 modified rate: " << account1.getRate() << endl;
  cout << "Account2 modified rate: " << account2.getRate() << endl;

  return 0;
}
```


#### 静态的常量成员

- 静态的常量成员：整个类的所有对象的共享常量——“类一级的常量”
- 静态的常量成员的声明：`static const 类型 数据成员名 = 常量表达式;`
- 注意 const 数据成员和 static const 数据成员的区别——“对象一级的常量” vs “类一级的常量”

- 例如，在某个类中需要用到一个数组，该数组的大小对所有对象都相同。
- 在类中可指定一个数组规模，并创建一个该规模的数组。如下所示：

``` cpp
class sample 
{
    static const int SIZE = 10;
    int storage[SIZE];
    // 其他声明内容...
};
```

不能用静态常量成员变量来设置数组的大小，除非这个静态常量成员变量在类定义内部有一个初始值。

``` cpp
class sample 
{
    static const int SIZE;
    int storage[SIZE];
    // 其他声明内容...
};
// 这是错误的！
const int sample::SIZE = 10;
```


### 友元

- 私有成员只能让它的成员函数来访问。
- 友元函数是一扇通往私有成员的**后门**。
- 友元可以是：
  - 外部函数（友元函数）
  - 另一个类的成员函数（友元成员）
  - 整个类（友元类）


- 友元关系是**授予**，**非索取**。
  - 如果函数 `f` 要成为类 `A` 的友元，类 `A` 必须显式声明函数 `f` 是它的友元，而不是函数 `f` 自称是类 `A` 的友元。

- 友元关系**不是对称关系**。
  - 如果类 `A` 声明了类 `B` 是它的友元，并不意味着类 `A` 也是类 `B` 的友元。

- 友元关系**不是传递关系**。
  - 如果类 `A` 是类 `B` 的友元，类 `B` 是类 `C` 的友元，并不意味着类 `A` 是类 `C` 的友元。

定义在类内
``` cpp
class girl {
  char name[10];
  int age;

public:
  girl(char *n, int d) {
    strcpy(name, n);
    age = d;
  }
  // 定义在类内
  friend void disp(girl &x) { cout << x.name << " " << x.age << endl; }
};

int main() {
  girl e("abc", 15);
  disp(e);
  return 0;
}
```

定义在类外
``` cpp
class girl {
  char name[10];
  int age;

public:
  girl(char *n, int d) {
    strcpy(name, n);
    age = d;
  }
  // 声明在类内
  friend void disp(girl &x);
};

// 定义在类外
void disp(girl &x) { 
  cout << x.name << " " << x.age << endl; 
}

int main() {
  girl e("abc", 15);
  disp(e);
  return 0;
}
```


 

## 11-运算符重载-不在考试范围

### 运算符重载

在 C++ 中，运算符重载允许你定义或修改运算符（如 `+`, `-`, `*`, `<<`, `>>` 等）对用户自定义类型（如类和结构体）的行为。这使得自定义类型可以像内置类型一样使用运算符，增强了代码的可读性和可维护性。

运算符重载的语法类似于函数定义。以下是运算符重载的一些常见示例和使用方法：

#### 示例：重载加法运算符 (`+`)

假设我们有一个表示二维点的类 `Point`，我们希望能够使用 `+` 运算符来相加两个点：

``` cpp
##include <iostream>
using namespace std;

class Point {
private:
    int x, y;
public:
    Point(int x = 0, int y = 0) : x(x), y(y) {}

    // 重载加法运算符
    Point operator+(const Point &p) const {
        return Point(x + p.x, y + p.y);
    }

    void display() const {
        cout << "(" << x << ", " << y << ")" << endl;
    }
};

int main() {
    Point p1(1, 2), p2(3, 4);
    Point p3 = p1 + p2; // 使用重载的 + 运算符
    p3.display(); // 输出: (4, 6)
    return 0;
}
```

在这个示例中，我们定义了一个 `Point` 类，并重载了 `+` 运算符。`operator+` 函数返回一个新的 `Point` 对象，其 x 和 y 坐标分别是对应坐标的和。

#### 示例：重载插入运算符 (`<<`)

为了方便打印自定义类型的对象，我们可以重载插入运算符 `<<`：

``` cpp
##include <iostream>
using namespace std;

class Point {
private:
    int x, y;
public:
    Point(int x = 0, int y = 0) : x(x), y(y) {}

    // 重载插入运算符
    friend ostream& operator<<(ostream &os, const Point &p) {
		// 返回传入的 `ostream` 对象 `os` 的引用。
		// 这是一种常见的做法，以便支持链式输出操作。
        os << "(" << p.x << ", " << p.y << ")";
        return os;
    }
};

int main() {
    Point p1(1, 2), p2(3, 4);
    cout << p1 << endl; // 输出: (1, 2)
    cout << p2 << endl; // 输出: (3, 4)
    return 0;
}
```

在这个示例中，我们重载了 `<<` 运算符，使得 `Point` 对象可以直接使用 `cout` 输出。注意，我们将 `operator<<` 函数声明为友元函数，以便它可以访问 `Point` 类的私有成员。

#### 示例：重载赋值运算符 (`=`)

重载赋值运算符通常用于自定义类的深拷贝：

``` cpp
##include <iostream>
using namespace std;

class Point {
private:
    int x, y;
public:
    Point(int x = 0, int y = 0) : x(x), y(y) {}

    // 重载赋值运算符
    Point& operator=(const Point &p) {
        if (this != &p) { // 防止自赋值
            x = p.x;
            y = p.y;
        }
        return *this;
    }

    void display() const {
        cout << "(" << x << ", " << y << ")" << endl;
    }
};

int main() {
    Point p1(1, 2), p2;
    p2 = p1; // 使用重载的 = 运算符
    p2.display(); // 输出: (1, 2)
    return 0;
}
```

在这个示例中，我们重载了赋值运算符 `=`，并在操作前检查自赋值情况。

#### 总结

运算符重载允许你定义或修改运算符对自定义类型的行为，从而使自定义类型可以像内置类型一样使用运算符。运算符重载常用于增强代码的可读性和可维护性，但需谨慎使用，以避免混淆和误用。


### 编译器如何解释重载

当你写 `obj1 == obj2` 时，编译器将其解释为 `obj1.operator==(obj2)`：

- **成员函数 `fun`**：调用 `obj1.fun(obj2)`。
- **重载的 `==` 运算符**：调用 `obj1.operator==(obj2)`。

两者在逻辑上是等价的，只是语法上有所不同。使用运算符重载使得代码更直观和自然，尤其是对于常见操作如比较、加减乘除等。



可以将 `operator==` 当作一个特殊的成员函数，其作用是重载 `==` 运算符，以便在对象之间进行比较时实现自定义的逻辑。

#### 类比与普通成员函数

假设我们有一个普通的成员函数 `fun`，它接受一个参数并返回一个布尔值。我们可以这样定义和使用它：

``` cpp
##include <iostream>
using namespace std;

class X {
private:
    int value;
public:
    // 构造函数
    X(int v = 0) : value(v) {}

    // 普通成员函数
    bool fun(const X& other) const {
        return value == other.value;
    }
};

int main() {
    X obj1(10);
    X obj2(10);
    X obj3(20);

    if (obj1.fun(obj2)) {
        cout << "obj1 is equal to obj2" << endl; // 输出: obj1 is equal to obj2
    } else {
        cout << "obj1 is not equal to obj2" << endl;
    }

    if (obj1.fun(obj3)) {
        cout << "obj1 is equal to obj3" << endl;
    } else {
        cout << "obj1 is not equal to obj3" << endl; // 输出: obj1 is not equal to obj3
    }

    return 0;
}
```

在这个示例中，我们定义了一个名为 `fun` 的成员函数，它接受一个 `X` 类型的对象并返回一个布尔值，表示两个对象是否相等。


#### 重载 `==` 运算符

类似地，我们可以重载 `==` 运算符，使其表现得像 `fun` 函数，但使用更直观的运算符语法：

``` cpp
##include <iostream>
using namespace std;

class X {
private:
    int value;
public:
    // 构造函数
    X(int v = 0) : value(v) {}

    // 重载 == 运算符为成员函数
    bool operator==(const X& other) const {
        return value == other.value;
    }
};

int main() {
    X obj1(10);
    X obj2(10);
    X obj3(20);

    // 使用重载的 == 运算符
    if (obj1 == obj2) {
        cout << "obj1 is equal to obj2" << endl; // 输出: obj1 is equal to obj2
    } else {
        cout << "obj1 is not equal to obj2" << endl;
    }

    if (obj1 == obj3) {
        cout << "obj1 is equal to obj3" << endl;
    } else {
        cout << "obj1 is not equal to obj3" << endl; // 输出: obj1 is not equal to obj3
    }

    return 0;
}
```


#### 总结

- **运算符重载**：`operator==` 是一个特殊的成员函数，用于重载 `==` 运算符。
- **编译器解释**：`obj1 == obj2` 被编译器解释为 `obj1.operator==(obj2)`。
- **类比**：可以将 `operator==` 看作一个普通的成员函数 `fun`，只是使用了运算符语法，使得代码更简洁和易读。

通过运算符重载，你可以定义对象之间的自定义比较逻辑，使得对象间的操作更加直观和符合习惯。





## 12-组合与继承
### 组合

定义一个复数类，而复数的虚部和实部都用有理数表示，实现复数的加法和输出

方案：利用已有的有理数类，复数类的数据成员是两个有理数类对象

```cpp
class Rational {
  int num;
  int den;
  void ReductFraction();

public:
  Rational(int a = 0, int b = 1);
  void add(const Rational &r1, const Rational &r2);
  // r3.add(r1, r2)
  void multi(const Rational &r1, const Rational &r2);
  // r3.multi(r1, r2)
  void display();
};

class Complex {
private:
  Rational Re;
  Rational Im;

public:
  Complex(int r1 = 0, int r2 = 1, int i1 = 0, int i2 = 1)
      : Re(r1, r2), Im(i1, i2) {}
  Complex(Rational r1, Rational r2) : Re(r1), Im(r2) {}
  void add(const Complex &c1, const Complex &c2) {
    Re.add(c1.Re, c2.Re);
    Im.add(c1.Im, c2.Im);
  }
  void display() {
    Re.display();
    cout << '+';
    Im.display();
    cout << 'i';
  }
};

```

### 继承

#### 派生类的定义

protected 访问特性：protected 成员是一类特殊的私有成员，
它不可以被全局函数或其他类的成员函数访问，但能被派
生类的成员函数和友元函数访问

``` cpp
class 派生类名 : 派生方法 基类名
{
    // 派生类新增的数据成员和成员函数
};
```


|           | 基类成员在派生类中的访问特性                                                            |
| :-------: | ------------------------------------------------------------------------- |
|  public   | 在派生类中为 public，<br />可以由任何非 static 成员函数、友元函数和非成员函数访问                       |
| protected | 在派生类中为 protected，<br />可以直接由任何非 static 成员函数、友元函数访问                        |
|  private  | 在派生类中隐藏，<br />可以通过基类的 public 或 protected 成员函数、<br />或非 static 成员函数和友元函数访问 |

#### 访问控制

继承时，可以使用不同的**访问控制符**（派生方法）来决定基类成员在派生类中的可见性：

- `public`：基类的 `public` 成员在派生类中保持 `public`，`protected` 成员保持 `protected`。
- `protected`：基类的 `public` 和 `protected` 成员在派生类中都变为 `protected`。
- `private`：基类的 `public` 和 `protected` 成员在派生类中都变为 `private`。

#### 派生实例

```cpp
class base {
  int x; // 默认为private

public:
  void setx(int k);
};

class derived1 : public base {
  int y; // derived1 有两个成员变量x,y

public:
  void sety(int k);
  // derived1 有两个成员函数 setx,sety
};

```

#### 派生类对象的构造

派生类对象的构造
* 派生类的构造函数体负责派生类新增成员的构造
* 基类部分由基类构造函数完成
* 派生类构造函数调用基类的构造函数
* 派生类构造函数的格式：`派生类构造函数名（参数表）：基类构造函数名（参数表）{...} `




派生类引用基类的同名函数
#### 虚函数



``` cpp
class Shape {
public:
    virtual void printShapeName() { cout << "Shape" << endl; }
};

class Point : public Shape {
public:
    virtual void printShapeName() { cout << "Point" << endl; }
};

class Circle : public Point {
public:
    virtual void printShapeName() { cout << "Circle" << endl; }
};

class Cylinder : public Circle {
public:
    virtual void printShapeName() { cout << "Cylinder" << endl; }
};

int main(){
	Shape * shape_list[3];
	shape_list[0] = new Pi

}
```



### 纯虚函数和抽象类

- **纯虚函数**：没有函数体的函数
- **纯虚函数声明格式**：`virtual 类型 函数名（参数表）= 0;`
- **抽象类**
    - 含有纯虚函数的类
    - **不能定义抽象类的对象！**
- **抽象类的意义**：保证进入继承层次的每个类都具有纯虚函数所要求的行为


抽象类实例：
- 对所有的汽车都要求有一个显示车辆信息的功能
- 如何保证每个类都有这个功能？
- 可以定义一个抽象类，包含一个显示车辆信息的纯虚函数，各类汽车类都从他派生

``` cpp
class vehicle {
public:
    virtual void display() const = 0;
};

class car : public vehicle {
public:
    // 其他成员函数和变量
    void display() const override {
        // 实现 display 方法
    }
};

class bus : public vehicle {
public:
    // 其他成员函数和变量
    void display() const override {
        // 实现 display 方法
    }
};
```



## 13-类模板-不在考试范围


### 类模板与其实例







## 14-输入输出与文件-考得不多


### 流与标准库


### 输入输出缓冲


### 基于控制台的 I/O


### 基于文件的 I/O



#### 流式文件

#### 文件的顺序访问

#### 文件的随机访问

``` cpp
##include <fstream>
##include <iostream>
using namespace std;

int main() {
  fstream in("file.txt", ios::in | ios::out); // 显式指定以读写模式打开文件

  // file.txt:
  // 1 2 3 4 5 6 7 8 9 10
  // 第0个字节对应1的位置
  // 第1个字节对应1后的空格
  // 以此类推

  int i;
  // 检查文件是否成功打开
  if (!in) {
    cerr << "open file error\n";
    return 1;
  }

  // 移动写指针到文件的第10个字节位置
  in.seekp(1);
  in << 207;

  // 移动读指针到文件的开头
  in.seekg(0);

  // 读取文件中的整数并显示
  while (in >> i) {
    cout << i << '\n';
  }

  // 关闭文件
  in.close();

  return 0;
}
```

#### 访问有记录概念的文件



## 15-tips


### 函数

switch 注意有无 break：即会不会 fall-through

作用域/生命周期

静态变量

函数模板

注意函数声明位置

### 一些常考算法

冒泡排序/选择排序

二分查找

递归：汉诺塔/插入排序

### 类

静态变量和函数

访问限制

### 继承

访问限制



