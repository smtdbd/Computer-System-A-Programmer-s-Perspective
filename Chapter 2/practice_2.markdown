
## 练习2.21
### 假设在采用补码运算的32位机器上对这些表达式求值，按照图2-19的格式填写下表，描述强制类型转换和关系运算的结果。
|表达式|类型|求值|
|:-:|:-:|:-:|
|-2147483647-1 == 2147483648U|无符号|1|
|-2147483647-1 < 2147483647|有符号|1|
|-2147483647-1U < 2147483647|无符号|0|
|-2147483647-1 < -2147483647|有符号|1|
|-2147483647-1U < -2147483647|无符号|1|
***
## 练习2.23
### 考虑下面的C函数
```c
int fun1(unsigned word){
  return (int) ((word << 24) >> 24);
}
int fun1(unsigned word){
  return  (((int)word << 24) >> 24);
}
```
假设在一个采用补码运算的机器上以32位程序来执行这些函数。还假设有符号值的右移是算术右移，而无符号数值的右移是逻辑右移。
* 填写下表，说明这些函数对几个实例参数的结果，你会发现用十六进制表示来做会更方便，只要记住十六进制数字8到F的最高有效位等于一。    

|w|fun1(w)|fun2(w)|
|:-:|:-:|:-:|
|0x00000076|0x00000076|0x00000076|
|0x87654321|0x00000021|0x00000021|
|0x000000C9|0x000000C9|0xFFFFFFC9|
|0xEDCBA987|0x00000087|0xFFFFFF87|
***
## 练习2.24
### 假设将一个4位数值(用十六进制数字0~F表示)阶段到一个3位数值(用十六进制数字0~7表示)。填写下标，根据哪些位模式的无符号和补码解释，说明这种截断对某些情况的结果。
<table>
    <tr>
        <th colspan="2">十六进制</th>
        <th colspan="2">无符号</th>
        <th colspan="2">补码</th>
    </tr>
    <tr>
        <td>原始值</td>
        <td>截断值</td>
        <td>原始值</td>
        <td>截断值</td>
        <td>原始值</td>
        <td>截断值</td>
    </tr>
    <tr>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
    </tr>
    <tr>
        <td>2</td>
        <td>2</td>
        <td>2</td>
        <td>2</td>
        <td>2</td>
        <td>2</td>
    </tr>
    <tr>
        <td>9</td>
        <td>1</td>
        <td>9</td>
        <td>1</td>
        <td>-7</td>
        <td>1</td>
    </tr>
    <tr>
        <td>B</td>
        <td>3</td>
        <td>11</td>
        <td>3</td>
        <td>-5</td>
        <td>3</td>
    </tr>
    <tr>
        <td>F</td>
        <td>7</td>
        <td>15</td>
        <td>7</td>
        <td>-1</td>
        <td>-1</td>
    </tr>
</table>

***
## 练习2.25
### 考虑下列代码，这段代码试图计算数组a中所有元素的和，其中元素的数量由参数length给出
```cpp
/* WARNING: This is buggy code */
float sum_elements(float a[], unsigned length) {
  int i;
  float result = 0;
  for (i = 0; i <= length - 1; i++)
    result += a[i];
  return result;
}
```
**当参数length等于0时，运行这段代码应该返回0.0。但实际上，运行时会遇到一个内存错误。请解释为什么会发生这样的情况，并且说明如何修改代码**  
当length=0时，length-1=4294967295,发生了溢出，同时i是一个int，它的最大值为2147483647，永远不会达到循环终点，会持续向后读内存直到崩溃。改正：

```cpp
float sum_elements(float a[], unsigned length) {
  unsigned i;
  float result = 0;
  for (i = 0; i < length; i++)
    result += a[i];
  return result;
}
```
***
## 练习2.26
### 现在给你一个任务，写一个函数用来判定一个字符串是否比另一个更长。前提是你要用字符串库函数strlen，它的声明如下：
```cpp
size_t strlen(const char *s);
```
**最开始你写的函数是这样的：**
```cpp
int strlonger(char *s, char *t) { return strlen(s) - strlen(t) > 0; }
```
**当你在一些示例数据上测试这个函数是，一切似乎都是正确的。进一步研究发现，在头文件stdio.h中数据类型size_t是定义为unsigned int的。**
* * 什么情况下，这个函数会产生不正确的结果？
  * 当s比t更短时。
* * 解释为什么会出现这样的结果。
  * 由于负溢出,unsigned总是一个正数，会返回错误结果
* * 说明如何修改这段代码好让他能可靠的工作。
  * ```cpp
    int strlonger(char *s, char *t) { return (int)strlen(s) - (int)strlen(t) > 0; }
    ```
***
## 练习2.27
### 写出一个具有如下原型的函数：
```cpp
/* Determine whether arguments can be added without overflow */
int uadd_ok(unsigned x, unsigned y);
```
**如果参数x和y相加不会产生溢出，则此函数返回 1**
```cpp
int uadd_ok(unsigned x, unsigned y) { return (int)(x + y >= x); }
```
***
## 练习2.28
### 我们能用一个十六进制数字来表示长度w=4的位模式。对于这些数字的无符号解释，使用等式2.12填写下表，给出数字的无符号加法逆元的位表示(用十六进制形式)
<table>
    <tr>
        <th colspan="2">x</th>
        <th colspan="2">-<sup>u</sup><sub style="margin-left:-8px">4</sub>x</th>
    </tr>
    <tr>
        <td>十六进制</td>
        <td>十进制</td>
        <td>十进制</td>
        <td>十六进制</td>
    </tr>
    <tr>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
    </tr>
    <tr>
        <td>5</td>
        <td>5</td>
        <td>11</td>
        <td>B</td>
    </tr>
    <tr>
        <td>8</td>
        <td>8</td>
        <td>8</td>
        <td>8</td>
    </tr>
    <tr>
        <td>D</td>
        <td>13</td>
        <td>3</td>
        <td>3</td>
    </tr>
    <tr>
        <td>F</td>
        <td>15</td>
        <td>1</td>
        <td>1</td>
    </tr>
</table>

***
## 练习题2.29
### 按照图2-25的形式填写下表。分别列出五位参数的整数值、整数和与补码和的数值、补码和的位级表示，以及属于等式2.13中的哪种情况。
|x|y|x+y|x+ty|情况|
|:-:|:-:|:-:|:-:|:-:|
|10100|10001|100101|00101|负溢出|
|11000|11000|110000|10000|负溢出|
|10111|01000|11111|11111|正常|
|00010|00101|00111|00111|正常|
|01100|00100|10000|10000|正溢出|
***
## 练习2.30
### 写出一个具有如下原型的函数：
```cpp
/* Determine whether arguments can be added without overflow */
int tadd_ok(int x, int y);
```
**如果参数x和y相加不会产生溢出，这个函数就返回 1**
```cpp
int tadd_ok(int x, int y) {
  int sum = x + y;
  if (x > 0 && y > 0 && sum < 0)
    return 0;
  if (x < 0 && y < 0 && sum > 0)
    return 0;
  return 1;
}
```
***
## 练习2.31
### 你的同事对你补码加法溢出条件的分析有些不耐烦了，他给出了一个函数tadd_ok的实现，如下所示：
```cpp
/* WARNING: This is buggy code */
/* Determine whether arguments can be added without overflow */
int tadd_ok(int x, int y) {
  int sum = x + y;
  return (sum - x == y) && (sum - y == x);
}
```
**你看了代码后笑了，解释下为什么？**  
这个函数在计算sum发生溢出之后，return时有溢出回来了。因此它总会返回1
***
## 练习2.32
### 你现在有一个任务，编写函数tsub_ok的代码，函数的参数是x和y，如果计算x-y不发生溢出，函数就返回1.假设你写的代码如下所示：
```cpp
/* WARNING: This is buggy code */
/* Determine whether arguments can be added without overflow */
int tsub_ok(int x, int y) { return tadd_ok(x, -y); }
```
**x和y取何值时，这个函数会产生错误的结果？**  
当y去INT_MIN时，由于INT_MIN的负数依然是INT_MIN，所以此时当x为负数时，必定返回错误结果。
***
## 练习2.33
### 我们能用一个十六进制数字来表示长度w=4的位模式。对于这些数字的补码解释，填写下表，确定所示的数字的加法逆元
<table>
    <tr>
        <th colspan="2">x</th>
        <th colspan="2">-<sup>t</sup><sub style="margin-left:-8px">4</sub>x</th>
    </tr>
    <tr>
        <td>十六进制</td>
        <td>十进制</td>
        <td>十进制</td>
        <td>十六进制</td>
    </tr>
    <tr>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
    </tr>
    <tr>
        <td>5</td>
        <td>5</td>
        <td>-5</td>
        <td>-5</td>
    </tr>
    <tr>
        <td>8</td>
        <td>-8</td>
        <td>-8</td>
        <td>-8</td>
    </tr>
    <tr>
        <td>D</td>
        <td>-3</td>
        <td>3</td>
        <td>3</td>
    </tr>
    <tr>
        <td>F</td>
        <td>-1</td>
        <td>1</td>
        <td>1</td>
    </tr>
</table>

***
## 练习2.34
### 按照图2-27的风格填写下标，说明不同的三位数字乘法的结果。
|模式|x|y|x\*y|截断的x\*y|
|:-:|:-:|:-:|:-:|:-:|
|无符号|100|101|10100|100|
|补码|100|101|001100|100|
|无符号|010|111|001110|110|
|补码|010|111|111110|110|
|无符号|110|110|100100|100|
|补码|110|110|000100|100|
***
## 练习2.40
### 对于下面每个K的值，找出只用指定数量的运算表达x*K的方法，这里我们认为加法和减法的开销相当。除了我们已经考虑过的简单的形式A和B原则，你可能会需要使用一些技巧，
|K|移位|加减法|表达式|
|:-:|:-:|:-:|:-:|
|6|2|1|(x<<2)+(x<<1)|
|31|1|1|(x<<5)-x|
|-6|2|1|(x<<1)-(x<<3)|
|55|2|2|(x<<6)-(x<<3)-x|
***
