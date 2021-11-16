# typedef的详细用法

##  0 例子 or 引子

```c++
typedef int P(); // 声明一个函数类型P：无参，返回值为int
typedef int Q(); // 声明一个函数类型Q：无参，返回值为int
class x {
    static P(Q); // 等价于 `static int Q()`, Q在此作用域中不再是一个类型
    static Q(P); // 等价于 `static int Q(int ())`, 定义了一个名为Q的function
};
```

## 1 官方定义

> Typedef does not work like typedef [type] [new name]. **The [new name] part does not always come at the end**.
>
> Typedef不像Typedef [type] [new name]那样工作。**[新名称]部分并不总是在结尾**。
>
> You should look at it this way: if [some declaration] declares a variable, typedef [same declaration] would define a type.
>
> 你应该这样看:如果[某个声明]声明了一个变量，typedef[相同的声明]将定义一个类型。

看我标黑的这句话, 总结一下就是: **任何声明变量的语句前面加上typedef之后，原来是变量的都变成一种类型**。**不管这个声明中的标识符号出现在中间还是最后**.

## 2 隐藏技能

> typedef 定义的新类型, 使用时可以省略括号.

```cpp
typedef int NUM;
NUM a = 10; // 也可写成`NUM(a) = 10;`
NUM(b) = 12; // 也可写成`NUM b = 12;`
```

## 3 举例

先从初级的开始:

```cpp
// 整形
typedef int x; // 定义了一个名为x的int类型
// 结构体
typedef struct { char c; } s; // 定义名为s的struct类型
// 指针
typedef int *p; //定义了一个名为p的指针类型, 它指向int (中文描述指针好累)
```


接下来是 **高级**的(注意标识符不一定在最后):

```cpp
// 数组
typedef int A[];  // 定义一个名为A的ints数组的类型
// 函数
typedef int f(); // 定义一个名为f, 参数为空, 返回值为int的函数类型
typedef int g(int); // 定义一个名为g, 含一个int参数, 返回值为int行的函数类型
```

回过头看：

```cpp
typedef int P(); // P:function类型，无参，返回值为int
static P(Q); // 等价于 `static P Q`，声明 Q 是一个无参，返回值为int的函数
```

## 4 用处（简化复杂的声明和定义）

我们都知道C++语言里, 函数都是先声明后使用的(除非在使用之前定义), 看以下例子:

```c++
#include <iostream>
#include <stdio.h>
#include <string>
 
typedef int P(); // 简单的
typedef void Q(int *p, const std::string& s1, const std::string& s2, size_t size, bool is_true); // 复杂的
class X {
public:
    P(eat_shit); // 等价于声明`int eat_shit();`
    Q(bullshit); // 等价于声明`void bullshit(int *p, const string& s1, const string& s2, size_t size, bool is_true);`
};
 
int main() {
    X *xx;
    printf("shit ret: %d\n", xx->eat_shit());
    int a[] = {1, 3, 4, 5, 7};
    xx->bullshit(a, "foo", "bar", sizeof(a)/sizeof(int), true);
}
 
int X::eat_shit() {
    return 888;
}
 
void X::bullshit(int *p, const std::string& s1, const std::string& s2, size_t size, bool is_true) {
    std::cout << "s1: " << s1 << ", s2: " << s2 << ", size: " << size << std::endl;
    printf("elems:\n");
    for(int i = 0; i < size; i++) {
        printf("%d %s",  *p++, (i == size-1) ? "" : ",");
    }
    printf("\n");
}
```

## 5 复杂声明和定义

```c++
1. 定义一个整型数
int a;
2. 定义一个指向整型数的指针
int *p;
3. 定义一个*指向指针的***指针**，它指向的指针指向一个整型数
int **pp;

到这一步我想大多数人都还好理解，我们可以用一些简单的代码把这三条给串起来：
int a;
int *p;
int **pp;
p = &a;   // p指向整数a所在的地址
pp = &p;  // pp指向指针p
```

```cpp
4. 定义一个包含10个整型数的数组
int arr[10];
5. 定义一个指向*包含10个整型数数组*的指针
int (*pArr) [10];

用几行代码将4、5两个定义串起来：
int arr[10];
int (*pArr) [10];
pArr = &arr;
```

```cpp
6. 定义一个指向函数的**指针**，被指向的函数有一个整型参数并返回整型值
int (*pfunc) (int);
7. 定义一个包含10个指针的**数组**，其中包含的指针指向函数，这些函数有一个整型参数并返回整型值
int (*arr[10]) (int);

用几行代码将6、7两个定义串起来：
    int (*pfunc) (int);
int (*arr[10]) (int);
arr[0] = pfunc;
```

## 6 *右左法则*

### 6.1 步骤

**第一步：从变量名看起，先往右，再往左，碰到圆括号就调转阅读的方向；**

**第二步：括号内分析完就跳出括号，还是先右后左的顺序。**

**第三步：如此循环，直到分析完整个定义**。**

>6. int (*pfunc) (int);
>
>1）找到变量名 **pfunc**，先往右是圆括号，调转阅读方向，左边是一个*号，说明 **pfunc**是一个**指针**；
>
>2）跳出括号，先往右，又遇到圆括号，说明**(*pfunc)**是一个函数，所以**pfunc**一个指向这类函数的指针（函数指针）：这类函数具有一个int类型的参数，返回值类型是int。
>
>7. int (*arr[10]) (int);
>
>  找到变量名arr，先往右是[]运算符，说明arr是一个数组；再往左是一个*号，说明arr数组的元素是指针（**注意**：这里的*修饰的不是arr，而是arr[10]。原因是[]运算符的优先级比*要高，arr先与[]结合。）；跳出圆括号，先往右又遇到圆括号，说明arr数组的元素是指向函数的指针，它指向的函数有一个int类型的参数，返回值类型是int。

### 6.2 **可以抽象出几个模式：**

- type (*var)(...); // 变量名var与*结合，被圆括号括起来，右边是参数列表。表明这是**函数指针**
- type (*var)[];  //变量名var与*结合，被圆括号括起来，右边是[]运算符。表示这是**数组指针**
- type (*var[])...;   // 变量名var先与[]结合，说明这是一个**数组**（至于数组包含的是什么，由旁边的修饰决定）  

例子： float (* (*fp2) (int, int, float)) (int);

​    找到变量名fp2，往右看是圆括号，调转方向往左看到*号，说明fp2是一个指针；跳出内层圆括号，往右看是参数列表，说明fp2是一个函数指针，接着往左看是*号，说明指向的函数返回值是指针；再跳出外层圆括号，往右看还是参数列表，说明返回的指针是一个函数指针，该函数有一个int类型的参数，返回值类型是float。*简言之*，fp2是一个指向函数的指针，该函数接受三个参数(int, int和float)，且返回一个指向函数的指针，该函数接受一个整型参数并返回一个float。

 ## 7 用法

  typedef可以给现有的类型起个**别名**。

```c++
typedef int *(*A[10]) (int, char*); *// 在之前定义的前面加入typedef，然后将变量名a替换成类型名A*
// 之后就简单了
A c;
```

