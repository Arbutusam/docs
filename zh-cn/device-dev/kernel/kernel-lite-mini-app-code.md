# 内核编码规范<a name="ZH-CN_TOPIC_0000001079036432"></a>

-   [总体原则](#section9512812145915)
-   [目录结构](#section1355317267017)
-   [命名](#section1375364815017)
-   [注释](#section1692516179119)
-   [格式](#section10888536113)
-   [宏](#section12276501124)
-   [头文件](#section158507231319)
-   [数据类型](#section91351731446)
-   [变量](#section575493915417)
-   [断言](#section13864440410)
-   [函数](#section671919481745)

此规范基于业界通用的编程规范整理而成，请内核的开发人员遵守这样的编程风格。

## 总体原则<a name="section9512812145915"></a>

总体原则:

-   清晰：代码应当易于理解、易于维护、易于重构，避免晦涩语法
-   简洁：命名简短，函数紧凑
-   高效：通过使用算法、编译器优化选项或硬件资源提高程序效率
-   美观：代码风格合理、一致

在大部分情况下，开发人员应当遵从以下规范，但也有一些例外场景。如修改第三方开源代码或大量使用开源代码接口下，应当与开源代码保持一致。请依据总体原则，灵活处理。

## 目录结构<a name="section1355317267017"></a>

建议按照功能模块划分子目录，子目录再定义头文件和源文件目录。

目录名和文件名如果没有特殊的需要，采用全小写的形式，可以使用下划线（“\_”）分割。

## 命名<a name="section1375364815017"></a>

推荐使用驼峰风格，具体规则如下：

<a name="table881274918408"></a>
<table><thead align="left"><tr id="row1886484994019"><th class="cellrowborder" valign="top" width="33.33333333333333%" id="mcps1.1.4.1.1"><p id="p6864184954016"><a name="p6864184954016"></a><a name="p6864184954016"></a>类型</p>
</th>
<th class="cellrowborder" valign="top" width="33.33333333333333%" id="mcps1.1.4.1.2"><p id="p486416495404"><a name="p486416495404"></a><a name="p486416495404"></a>命名风格</p>
</th>
<th class="cellrowborder" valign="top" width="33.33333333333333%" id="mcps1.1.4.1.3"><p id="p18864549124011"><a name="p18864549124011"></a><a name="p18864549124011"></a>形式</p>
</th>
</tr>
</thead>
<tbody><tr id="row486494913409"><td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.1.4.1.1 "><p id="p2864184916403"><a name="p2864184916403"></a><a name="p2864184916403"></a>函数、结构体类型、枚举类型、联合体类型、typedef的类型</p>
</td>
<td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.1.4.1.2 "><p id="p18864154944015"><a name="p18864154944015"></a><a name="p18864154944015"></a>大驼峰，或带有模块前缀的大驼峰</p>
</td>
<td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.1.4.1.3 "><p id="p10864249124015"><a name="p10864249124015"></a><a name="p10864249124015"></a>AaaBbb</p>
<p id="p1886444919403"><a name="p1886444919403"></a><a name="p1886444919403"></a>XXX_AaaBbb</p>
</td>
</tr>
<tr id="row198643495409"><td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.1.4.1.1 "><p id="p18864174964013"><a name="p18864174964013"></a><a name="p18864174964013"></a>局部变量，函数参数，宏参数，结构体中字段，联合体中成员</p>
</td>
<td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.1.4.1.2 "><p id="p12864204924018"><a name="p12864204924018"></a><a name="p12864204924018"></a>小驼峰</p>
</td>
<td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.1.4.1.3 "><p id="p8864549124012"><a name="p8864549124012"></a><a name="p8864549124012"></a>aaaBBB</p>
</td>
</tr>
<tr id="row15864184913405"><td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.1.4.1.1 "><p id="p786484914014"><a name="p786484914014"></a><a name="p786484914014"></a>全局变量</p>
</td>
<td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.1.4.1.2 "><p id="p1786544914400"><a name="p1786544914400"></a><a name="p1786544914400"></a>带“g_”前缀的小驼峰</p>
</td>
<td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.1.4.1.3 "><p id="p1986594994013"><a name="p1986594994013"></a><a name="p1986594994013"></a>g_aaaBBB</p>
</td>
</tr>
<tr id="row48651849104017"><td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.1.4.1.1 "><p id="p8865104914407"><a name="p8865104914407"></a><a name="p8865104914407"></a>宏（不含函数式宏），枚举值，goto标签</p>
</td>
<td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.1.4.1.2 "><p id="p186574954011"><a name="p186574954011"></a><a name="p186574954011"></a>全大写，下划线分割</p>
</td>
<td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.1.4.1.3 "><p id="p188653494401"><a name="p188653494401"></a><a name="p188653494401"></a>AAA_BBB</p>
</td>
</tr>
<tr id="row1286516495402"><td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.1.4.1.1 "><p id="p108651749114012"><a name="p108651749114012"></a><a name="p108651749114012"></a>函数式宏</p>
</td>
<td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.1.4.1.2 "><p id="p1586534910405"><a name="p1586534910405"></a><a name="p1586534910405"></a>全大写下划线分割，或大驼峰，或带有模块前缀的大驼峰</p>
</td>
<td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.1.4.1.3 "><p id="p1186544904010"><a name="p1186544904010"></a><a name="p1186544904010"></a>AAA_BBB</p>
<p id="p18865174918402"><a name="p18865174918402"></a><a name="p18865174918402"></a>AaaBbb</p>
<p id="p13865174910407"><a name="p13865174910407"></a><a name="p13865174910407"></a>XXX_AaaBbb</p>
</td>
</tr>
<tr id="row6865154974017"><td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.1.4.1.1 "><p id="p6865114944011"><a name="p6865114944011"></a><a name="p6865114944011"></a>头文件防止重复的符号</p>
</td>
<td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.1.4.1.2 "><p id="p16865134944010"><a name="p16865134944010"></a><a name="p16865134944010"></a>以下划线“_”开头以H结尾，中间为文件名的全大写并以下划线分割</p>
</td>
<td class="cellrowborder" valign="top" width="33.33333333333333%" headers="mcps1.1.4.1.3 "><p id="p1986520493408"><a name="p1986520493408"></a><a name="p1986520493408"></a>_AAA_H</p>
</td>
</tr>
</tbody>
</table>

内核对外API建议采用LOS\_ModuleFunc的形式，如果有宾语则建议采用前置的方式，比如：

```
LOS_TaskCreate
LOS_MuxLock
```

kernel目录下内部模块间的接口使用OsModuleFunc的形式，比如：

```
OsTaskScan
OsMuxInit
```

## 注释<a name="section1692516179119"></a>

一般的，尽量通过清晰的软件架构，良好的符号命名来提高代码可读性；然后在需要的时候，才辅以注释说明。

注释是为了帮助阅读者快速读懂代码，所以要从读者的角度出发，按需注释。

注释内容要简洁、明了、无歧义，信息全面且不冗余。

文件头部要进行注释，建议注释列出：版权说明、文件功能说明，作者、创建日期、注意事项等。

注释风格要统一，建议优先选择/\* \*/的方式，注释符与注释内容之间要有1空格，单行、多行注释风格如下：

```
/* 单行注释 */
// 单行注释
/*
 * 多行注释
 * 第二行
 */
// 多行注释
// 另一行
```

针对代码的注释，应该置于对应代码的上方或右方。

代码上方的注释，与代码行间无空行，保持与代码一样的缩进。

代码右边的注释，与代码之间，至少留1空格。

建议将多条连续的右侧注释对齐，比如：

```
#define CONST_A 100    /* Const A */
#define CONST_B 2000   /* Const B */
```

## 格式<a name="section10888536113"></a>

程序采用缩进风格编写，使用空格而不是制表符（’\\t’）进行缩进，每级缩进为4个空格。

换行时，函数左大括号另起一行放行首，并独占一行；其他左大括号跟随语句放行末。右大括号独占一行，除非后面跟着同一语句的剩余部分，如 do 语句中的 while，或者 if 语句的else/else if，或者逗号、分号。

一行只写一条语句。

比如：

```
struct MyType {  // 跟随语句放行末，前置1空格
    ...
};               // 右大括号后面紧跟分号
int Foo(int a) { // 函数左大括号独占一行，放行首
    if (a > 0) {
        Foo();   // 一行只有一条语句
        Bar();         
     } else {    // 右大括号、"else"、以及后续的左大括号均在同一行
        ...         
     }           // 右大括号独占一行
     ... 
}
```

每行字符数不要超过 120 个，代码过长时应当换行，换行时将操作符留在行末，新行缩进一层或进行同类对齐，并将表示未结束的操作符或连接符号留在行末。

```
// 假设下面第一行已经不满足行宽要求   
if (currentValue > MIN && // Good：换行后，布尔操作符放在行末
    currentValue < MAX) { // Good: 与(&&)操作符的两个操作数同类对齐
       DoSomething();
       ... 
}
flashPara.flashEndAddr = flashPara.flashBaseAddr + // Good: 加号留在行末
                         flashPara.flashSize; // Good: 加法两个操作数对齐

// Good：函数参数放在一行   
ReturnType result = FunctionName(paramName1, paramName2); 
ReturnType result = FunctionName(paramName1,
                                 paramName2,
                                 paramName3); // Good：保持与上方参数对齐                
ReturnType result = FunctionName(paramName1, paramName2,
      paramName3, paramName4, paramName5); // Good：参数换行，4 空格缩进  
ReturnType result = VeryVeryVeryLongFunctionName( // 行宽不满足第1个参数，直接换行
      paramName1, paramName2, paramName3); // 换行后，4 空格缩进  

// Good：每行的参数代表一组相关性较强的数据结构，放在一行便于理解   
int result = DealWithStructLikeParams(left.x, left.y,    // 表示一组相关参数
                                      right.x, right.y); // 表示另外一组相关参数
```

包括 if/for/while/do-while 语句应使用大括号，即复合语句。

```
while (condition) {} // Good：即使循环体是空，也应使用大括号
while (condition) {
     continue;       // Good：continue 表示空逻辑，使用大括号
}
```

case/default 语句相对 switch 缩进一层，风格如下：

```
switch (var) {
    case 0:             // Good: 缩进
        DoSomething1(); // Good: 缩进
        break;
    case 1: {           // Good: 带大括号格式
        DoSomething2();
        break;
    }
    default:
        break;
}
```

指针类型"\*"跟随变量或者函数名，例如：

```
int *p1;   // OK
int* p2;   // Bad：跟随类型
int*p3;    // Bad：两边都没空格
int * p4;  // Bad：两边都有空格
struct Foo *CreateFoo(void); // OK: "*"跟随函数名  
特例：
char * const VERSION = "V100";    // OK: 当有 const 修饰符时，"*"两边都有空格   
int Foo(const char * restrict p); // OK: 当有 restrict 修饰符时，"*"两边都有空格 
sz = sizeof(int*); // OK：右侧没有变量，"*"跟随类型
```

## 宏<a name="section12276501124"></a>

定义函数式宏前，应考虑能否用函数替代。对于可替代场景，建议用函数替代宏。对于有性能需求的场景，可以使用内联函数。

定义宏时，要使用完备的括号，例如：

```
#define SUM(a, b) ((a) + (b))   // 符合本规范要求.
#define SOME_CONST  100         // Good: 单独的数字无需括号   
#define ANOTHER_CONST   (-1)    // Good: 负数需要使用括号
#define THE_CONST   SOME_CONST  // Good: 单独的标识符无需括号
```

以下情况需要注意：

-   宏参数参与 '\#', '\#\#' 操作时，不要加括号；
-   宏参数参与字符串拼接时，不要加括号；
-   宏参数作为独立部分，在赋值（包括+=, -=等）操作的某一边时，可以不加括号；
-   宏参数作为独立部分，在逗号表达式，函数或宏调用列表中，可以不加括号。

```
// x 不要加括号
#define MAKE_STR(x) #x

// obj 不要加括号
#define HELLO_STR(obj) "Hello, " obj

// a, b 需要括号；而 value 可以不加括号
#define UPDATE_VALUE(value, a, b) (value = (a) + (b))

// a 需要括号；而 b 可以不加括号
#define FOO(a, b) Bar((a) + 1, b)
```

包含多条语句的函数式宏的实现语句必须放在 do-while\(0\)中。

禁止把带副作用的表达式作为参数传递给函数式宏，比如自加操作\(“a++”\)。

函数式宏定义中慎用 return、goto、continue、break 等改变程序流程的语句。

禁止宏调用参数中出现预编译指令，如\#include，\#deﬁne和\#ifdef，这样做会导致未定义的行为。

宏定义不以分号结尾。

## 头文件<a name="section158507231319"></a>

头文件应当职责单一。

通常情况下，每个.c文件都应有一个相应的.h文件（并不一定同名），用于放置对外提供的函数声明、宏定义、类型定义等。如果不需要提供对外接口，可以没有对应的.h文件。

避免头文件循环依赖，如a.h 包含 b.h，b.h 包含 c.h，c.h 包含 a.h。

头文件应当自包含，即包含某个头文件，不需要引入其他头文件就可以编译。

头文件用\#deﬁne、\#ifndef、\#endif保护，防止重复包含；不要使用 \#pragma once。

禁止通过声明的方式引用外部函数接口、变量，只能通过包含头文件的方式使用其他模块或文件提供的接口。

建议按稳定度包含头文件，依次顺序为： 源码对应的头文件，C标准库，操作系统库，平台库，项目公共库，自己其他的依赖。

## 数据类型<a name="section91351731446"></a>

基础数据类型建议使用los\_compiler.h中定义的类型，比如无符号32位整数位定义为UINT32。

## 变量<a name="section575493915417"></a>

避免大量栈分配，如较大的局部数组。

谨慎使用全局变量，尽量不用或少用全局变量。

变量应当初始化后再使用。

禁止将局部变量的地址返回到其作用域以外。

指向资源句柄或描述符的变量，在资源释放后立即赋予新值（如果变量的作用域马上结束可以不赋予新值）。指向资源句柄或描述符的变量包括指针、文件描述符、socket描述符以及其它指向资源的变量。

## 断言<a name="section13864440410"></a>

断言必须使用宏定义，且只能在调试版本中生效。

断言应当看作设计约束，禁止用断言检测程序在运行期间可能导致的错误，可能发生的错误要用错误处理代码来处理。

禁止在断言内改变运行环境。

一个断言只用于检查一个错误。

## 函数<a name="section671919481745"></a>

由一个进程向另一个进程发送的数据、由应用向内核发送的数据等应当进行合法性校验，校验包括但不限于：

-   校验数据长度
-   校验数据范围
-   校验数据类型和格式
-   校验输入只包含可接受的字符（“白名单”形式）

函数应避免使用全局变量、静态局部变量和直接的I/O操作，不可避免时，应当对读写操作进行封装。
