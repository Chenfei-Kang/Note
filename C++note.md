## 第二章  变量和基本类型

### 类型别名

别名声明语句: `using SI = Sales_item`，SI为Sales_item的同义词。或者使用`typedef Sales_item SI`

## 第三章  字符串、向量和数组

### 迭代器

迭代器iter可以执行的操作有：  以及++和--

![IterratorOpe](C:\Users\SSL\Desktop\C++ note\picture\IterratorOpe.png)

![Iterator](C:\Users\SSL\Desktop\C++ note\picture\Iterator.png)

有如下的示例:

![iteratorExample](C:\Users\SSL\Desktop\C++ note\picture\iteratorExample.png)

迭代器it可以在for循环中使用++运算符进行移动。访问对象的成员时，需要使用(*it).成员，C++将其简化为->运算符，可以直接使用->进行访问。在使用迭代器对容器进行操作时，不能向容器中添加元素。

左侧的迭代器减去右侧的迭代器，得到的是二者之间的距离，即为右侧迭代器向前移动多少个距离就能到达左侧的迭代器。其作差的结果的类型是名为difference_type的**带符号整数**，

### 数组

数组的下标是size_t类型的，并且数组的大小固定，无法动态变化。数组的下标可以是负数，p[a]翻译为*(p+a)，p为指针，a 可以是负数。

可以使用数组初始化vector对象，语法为: `vector<T> array(begin(arr), end(arr))`，必须使用一个头指针和尾后指针来初始化vector，并且新数组array中不包含尾后指针指向的元素，因此可以使用arr的一部分来初始化array。	**应当尽量使用内置类型，避免使用数组**

### 多维数组

C++中的多维数组是数组的数组，例如`int ia[3][4]`表示一个长度为3的数组，每一个元素都是一个长度为4的数组。第一个维度为行，第二个维度为列。初始化多维数组时，可以使用{ }进行初始化，如果{ }没有使用{ }进行 行元素的区分，会按照行依次填充，填充满则进入下一行；也可以只初始化每一行的前几个元素。

对多维数组进行for循环处理时，除了最内层的循环，其他循环的“取每一个元素”都应该声明为引用类型。因为C++对**数组名会自动初始化为指向首元素的指针（是指针，不是元素）**，在下一个循环里遍历这个指针而不是遍历这个指针指向的数组。*指针名和数组名等价（如果指针指向了这个数组），例如:

```c++
for(const auto &row : ia)  //如果不加引用，那么下面一个循环col遍历的是一个指针。 
    for (auto col : row)   
        cout << col << endl;
```

## 第四章 表达式

左值与右值： 当一个对象被用作右值时，使用的是对象的值(内容)；而当一个对象被用作左值时，使用的是对象的身份(在内存中的位置，可以用来修改对象的内容)。

`*pbeg++`等价于`*(pbeg++)`。

### 位操作符

C++中的位操作符如下: 一般位操作符使用于无符号整型。

![bitOperator](C:\Users\SSL\Desktop\C++ note\picture\bitOperator.png)

左移右移运算符的expr2为移动的位数（十进制数），左移在右侧插入0，右移视运算对象而定，如果对象是无符号整型，则在左侧插入0，如果是带符号整型，则在左侧插入符号位的副本或者值为0的二进制位。移位运算符又叫做IO运算符，满足左结合律，优先级比算数运算符低，但是比关系运算符、赋值运算符和条件运算符的优先级高。

### `sizeof`运算符

`sizeof`运算符返回一条表达式或者一个类型的名字所占用的字节数，满足右结合律。可以使用`sizeof`求数组大小，然后除以每个元素的大小从而得到数组的长度。

### 算数转换

整形提升指将小整数类型转化为大整数类型，例如将`int`类型提升为`unsigned int`类型；此外还有隐式的类型转换，例如变量可以转化为指针或者引用的常量定义，而指向常量的指针或者常量的引用不能用来转化为变量。

```c++
int i;
const int &j = i; //非常量转化为const int 的引用
const int *p = &i; //非常量的地址转化为const 的地址
int &r = j, *q = p; //错误：不允许const转化为非常量
```

显式转换：强制类型转换，格式为`cast-name <type>(expression)`，type是目标类型，expression是要转化的值，cast-name可以选择为static_cast  dynamic_cast  const_cast  reinterpret_cast 中的一种；区别详见p145

运算符优先表 p147

## 第五章  语句

语句作用域，使用{ }括起来的部分是变量的作用域，在作用域之外无法访问。悬垂else会自动和最近的、未匹配的if来进行匹配，和缩进无关。

```C++
switch (ch){
    case 'a':
        ++aCnt;
        break;
    case 'b':
        ++bCnt;
        break;
    defalut:
        ++i;
        
}
```

switch语句在遇到一个满足条件的case之后，会从这个标签开始，执行之后的所有case标签下的语句。

### try语句和异常处理

throw表达式：

```C++
if (item1.isbn() != item2.isbn())
    throw runtime_error('Data is different!')
cout<< item1+item2<< endl;
```

在上述表达式中，如果item1和item2调用函数isbn()不同，则会抛出一个异常，该异常是类型runtime_error的对象。抛出异常会终止当前的函数，并将控制权转移给处理该异常的代码。

try语句块： 其用法为：

```C++
try{
    program-statements
} catch (exception-declaration){
    handler-statements
} catch() ... ...
```

可以结合`throw`语句抛出的异常，利用`catch`语句进行异常的捕获，例如如下的例子：

```C++
while (cin>> item1 >> item2){
    try {
        //执行添加两个Sales_item对象的代码
        //如果添加失败，代码抛出一个runtime_error异常
    } catch (runtime_error err){
        //提醒用户两个ISBN必须一致，询问是否重新输入
        cout<< err.what()
            << "\nTry Again? Enter y or n" << endl;
        char c;
        cin>>c;
        if (!cin || c == 'n') // cin 读取一个字符，如果读入的是一个字符，那么cin>>为true; 否则为false。
            break; // 输入的非字符或者c = n时，跳出循环；
    }
}
```

当catch语句被完成之后，程序跳转到最后一个catch子句后的第一条语句执行。

当一个异常被抛出后，首先搜索抛出该异常的函数。如果没有找到匹配的catch子句，那么终止该函数，并在调用该函数的函数中继续寻找，如果仍旧没有找到，这个函数被终止，然后搜索调用该函数的函数，以此类推，直到找到匹配的catch语句。如果最终没能找到，那么程序会自动调用`terminate`的标准库函数进行程序的终止。

C++标准库中定义了一组异常类，定义在4个头文件中，例如在`stdexcept`头文件中定义的类：

![QQ截图20200917204648](C:\Users\SSL\Desktop\C++ note\picture\QQ截图20200917204648.png)

对于`exception、 bad_alloc、bad_cast`的对象，只能使用其类内的构造函数初始化，即默认初始化（定义时不进行初始化），其他类型的异常必须提供初始值（通常是一个string或者字符串）进行初始化，不允许使用默认初始化，例如抛出一个`runtime_out`类型的异常时，使用字符串`"Data is different !"`进行了类的初始化。





