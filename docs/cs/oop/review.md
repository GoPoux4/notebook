# 期末复习

## Formatting

```cpp
#include <iomanip>
cout << d << "; "
    << scientific << d << "; " // 1.23456e+02 style
    << hexfloat << d << "; " // 0x1.23456p+2 style
    << fixed << d << "; " // 123.456000 style
    << defaultfloat << d << endl; // same as cout << d << endl;
```

```cpp
cout.precision(3); // 3 digits after the decimal point
cout << d << endl; // 123.456
```

## Reference

- 引用不能重新绑定
- 不能引用右值
- 不能引用引用 `int &&`
- 没有指向引用的指针 `int &*p;` （可以引用指针 `int *&p;`）
- 没有引用的数组 `int &a[10];` （可以引用数组 `int (&a)[10];`）
  
## Memory Allocation

- `new` 顺序调用构造函数，`delete` 逆序调用析构函数
- `new` 返回地址前的 `size_t` 存储了数组的长度

## Const

- 全局 `const` 变量默认为 `static`，只在当前文件可见。加上 `extern` 可以在其他文件访问。
- Runtime Const
    ```cpp
    const int size1 = 10; // 编译期常量
    int arr1[size1]; // OK
    ```
    ```cpp
    int x;
    cin >> x;
    const int size2 = x; // 运行期常量
    int arr2[size2]; // Error
    ```

## Ctor & Dtor

- 初始化列表 `: member(value), ... {}`，初始化顺序与声明顺序一致
- const 成员函数不能修改成员变量，不能调用非 const 成员函数，相当于 `const T * this`
    const 实例不能调用非 const 成员函数
- const 成员变量在初始化列表中初始化

## Inheritance

- 派生类构造时，先调用基类构造函数，再根据初始化列表按照声明顺序调用成员构造函数，最后调用派生类构造函数；析构函数逆序
- 调用基类函数：
    ```cpp
    Derived::Base::func();
    ```
    或在派生类成员函数中调用
    ```cpp
    Base::func();
    ```
- 派生类中的同名函数会隐藏基类函数，调用时要使用限定符
    ```cpp
    Drived d;
    d.Base::func();
    ```
- `friend` 声明友元函数，可以访问类的私有成员，在类外定义时不需要加限定符
- 继承限定符
  
    | 继承限定 | public | protected | private |
    |:---:|:---:|:---:|:---:|
    | public | public | protected | 不可见 |
    | protected | protected | protected | 不可见 |
    | private | private | private | 不可见 |

## Polymorphism

- `virtual` 根据指针指向的对象调用函数；`override` 明确重写基类函数，重写需要保证函数签名一致（返回类型必须要么相同，要么为协变）
- 基类析构函数应该声明为虚函数，否则析构时会调用基类析构函数而不会调用派生类析构函数
- 声明 `virtual` 函数后，会在类中生成一个指向虚函数表的指针，存放类中所有虚函数的地址。当进行继承时，如果派生类重写了基类的虚函数，会在虚函数表中将该函数的地址替换为派生类的函数地址。
- 成员函数函数签名实际上为 `void foo(ClassName *this, ...)`
- 进行赋值时，不会复制虚函数表指针
    ```cpp
    Drived d;
    Base b;
    b = d; // 赋值时不会复制虚函数表指针，b 中 vptr 仍指向 Base 的虚函数表，派生类中的成员变量被截断
    ```
- 如果基类虚函数存在重载，那么派生类中若没有讲所有重载函数都重写，那么基类中未重写的函数会被派生类隐藏
- Interface: 纯虚函数，没有实现，派生类必须实现
    ```cpp
    class Interface {
    public:
        virtual ~Interface() = default;
        virtual void func(...) = 0;
        virtual void foo(...) = 0;
        // ...
    };
    ```

## Design

- 将确定的部分放在基类，将不确定的部分放在派生类让派生类实现
    ```cpp
    class Base {
    private:
        int x;
        // ...
        virtual void func() = 0; // Implement in Drived
    public:
        void run() {
            // ...
            func();
            // ...
        }
    };

    class Drived : public Base {
    private:
        // ...
        void func() override {
            // ...
        }
    };
    ```
- Code equality:
    1. Coupling：耦合度，模块之间的依赖关系要尽量减少
    2. Cohesion：内聚度，模块内部的功能要尽量独立，一个模块尽量只做一件事
- SOLID Principles:
    1. Single Responsibility Principle：单一职责原则
    2. Open-Closed Principle：开闭原则，对扩展开放，对修改关闭
    3. Liskov Substitution Principle：里氏替换原则，子类可以替换父类
    4. Interface Segregation Principle：接口隔离原则，使用多个专门的接口比使用单一的总接口要好
    5. Dependency Inversion Principle：依赖倒置原则，高层模块不应该依赖底层模块，两者都应该依赖抽象

## Copy Constructor

- 拷贝构造函数：`ClassName(const ClassName &obj) {}`
- 默认拷贝构造函数：逐个成员变量拷贝，原生对象逐字节拷贝，自定义对象调用拷贝构造函数
- 在函数传参，函数返回，初始化时调用拷贝构造函数
- 编译器优化：如果函数返回时，返回值是一个临时对象，编译器会优化，不调用拷贝构造函数，直接返回临时对象
    ```cpp
    ClassName foo() {
        return ClassName(1);
    }
    ClassName obj = foo(); // 无拷贝构造函数调用
    ```

## Static

- 静态全局变量：只在当前文件可见
- 静态局部变量：只初始化一次，函数结束后不销毁
- 静态成员变量：所有对象共享，不属于对象，属于类，存储在全局数据区。需要在类外定义、初始化，否则实例化时会链接错误
- 静态成员函数：只能访问静态成员变量和静态成员函数
    ```cpp
    class ClassName {
    public:
        static int x;
        int y;
        static void func(int z) {
            cout << x << z << endl;
            // cout << y << endl; // Error
        }
    };
    int ClassName::x;
    ```
- 静态变量在程序结束后销毁。

## Operator Overloading

- 不能重载的运算符：`::`, `.*`, `.`，`?:`，`sizeof`，`typeid`，`const_cast`，`dynamic_cast`，`reinterpret_cast`，`static_cast`
- 重载运算符形式：
    1. 成员函数：`ClassName operator+(const ClassName &obj) {}`
    2. 全局函数：`ClassName operator+(const ClassName &obj1, const ClassName &obj2) {}`，一般需要声明为友元函数
- 类型限制：调用时，第一个操作数不能进行类型转换
    ```cpp
    Integer x, y, z;
    z = x + y; // OK
    z = x + 1; // OK
    z = 1 + x; // Error
    ```
- `++` 使用 `int` 占位
    ```cpp
    ClassName operator++(int) {} // 后缀
    ClassName operator++() {} // 前缀

    // Definition
    ClassName& ClassName::operator++() {
        // ...
        return *this;
    }
    ClassName ClassName::operator++(int) {
        ClassName tmp = *this;
        // ...
        return tmp;
    }
    ```
- 编译器会生成默认的 `=` 运算符重载函数，逐个成员变量赋值
- 自定义类型转换：
    1. 类型转换构造函数
        ```cpp
        class A {
        public:
            // ...
        };
        class B {
        public:
            B(const A &a) {}
        };
        ```
        这里 `B` 可以通过 `A` 构造，可以进行以下调用：
        ```cpp
        void foo(B b) {}
        A a;
        foo(a); // OK，隐式调用 B 的构造函数
        B b(a); // OK，显式调用 B 的构造函数
        ```
        如果要禁用隐式转换，可以在构造函数前加 `explicit` 关键字
        ```cpp
        class B {
        public:
            explicit B(const A &a) {}
        };
        void foo(B b) {}
        A a;
        foo(a); // Error
        B b(a); // OK
        ```
    2. 或者使用 conversion operator
        ```cpp
        class B {
        public:
            B() {}
        };
        class A {
        public:
            operator B() const { // 不写返回值类型，返回类型为 B
                return B();
            }
        };

        void foo(B b) {}
        int main() {
            A a;
            foo(a); // OK，隐式调用 A 的 conversion operator
            B b0 = a; // OK，隐式调用 A 的 conversion operator
            B b = static_cast<B>(a); // OK，显式调用 A 的 conversion operator，然后调用 B 的构造函数
        }
        ```

    如果两种方法都存在，编译器会报错。一般使用自定义函数 `to_xxx` 来进行类型转换。

## Streams

- Stream naming conventions:

    | | Input | Output | Header |
    |:---:|:---:|:---:|:---:|
    | Generic | istream | ostream | `<iostream>` |
    | File | ifstream | ofstream | `<fstream>` |
    | C String | istrstream | ostrstream | `<strstream>` |
    | C++ String | istringstream | ostringstream | `<sstream>` |

- Stream extractor:
    ```cpp
    istream &operator>>(istream &is, T &obj) {
        // ...
        return is;
    }
    ```
- Stream inserter:
    ```cpp
    ostream &operator<<(ostream &os, const T &obj) {
        // ...
        return os;
    }
    ```
- other output operators:
    - `put(char)` 打印字符
    - `flush()` 刷新缓冲区，立即输出

## Templates

- 寻找函数优先级：
    1. 参数列表完全匹配
    2. 模版能否匹配
    3. 能否通过类型隐式转换匹配
- 可以显式指定模版参数类型，也可以自动推导
    ```cpp
    template <typename T>
    void foo(T t) {}

    foo<int>(1);
    foo(1); // 自动推导
    ```
- 继承：
    1. 从非模版类继承
        ```cpp
        template<class T>
        class Derived : public Base {};
        ```
    2. 从模版类继承
        ```cpp
        template<class T>
        class Derived : public Base<T> {};
        ```
        这里 `Base` 在 `T` 确定时就已经是一个确定的类了，和前面的情况一样
    3. 非模版类继承模版类
        ```cpp
        template<class T>
        class Base {};
        class Derived : public Base<int> {};
        ```
        必须指定模版参数类型
    4. CRTP 模式（Curiously Recurring Template Pattern）
        ```cpp
        template<class T>
        class Base {
            // ...
        };
        class Derived : public Base<Derived> {
            // ...
        };
        ```

        此时 `Base` 知道 `T` 一定继承自 `Base`，可以在 `Base` 中调用 `T` 的成员函数

## Iterators

- 封装，使得实现部分知道迭代器指向内容的类型
    ```cpp
    template<class T, class U>
    void foo_imp(T iter, U &val) {
        U tmp = *iter;
        // ...
    }
    template<class T>
    void foo(T iter) {
        foo_imp(iter, *iter);
    }
    ```
- Type info. definition:
    ```cpp
    template<class T>
    class Iter {
    public:
        typedef T value_type;
        // ...
    };
    template <class T>
    typename T::value_type func(T iter) {
        return *iter;
    }
    
    Iter<int> iter;
    func(iter);
    ```
- iterator traits 从迭代器中提取类型信息
    ```cpp
    template<class Iter>
    struct iterator_traits {
        typedef typename Iter::value_type value_type;
    };
    template<class T>
    typename iterator_traits<T>::value_type func(T iter) {
        return *iter;
    }
    ```
- template specialization 模版特化
    ```cpp
    template <class T1, class T2, int N>
    class A {}; // primary template
    template <>
    class A<int, int, 10> {}; // full specialization
    template <class T>
    class A<int, T, 10> {}; // partial specialization

    // code
    A<double, int, 0> a; // primary template
    A<int, int, 10> a1; // full specialization
    A<int, double, 10> a2; // partial specialization
    ```
- 使用模版特化实现 `iterator_traits`
    ```cpp
    template<class T>
    struct iterator_traits {
        typedef typename T::value_type value_type;
        typedef typename T::pointer_type pointer_type;
    };
    template<class I>
    struct iterator_traits<I*> {
        typedef I value_type;
        typedef I* pointer_type;
    };

    // code
    iterator_traits<int*>::value_type; // int
    iterator_traits<int*>::pointer_type; // int*
    ```
- iterator categories
    - InputIterator 输入迭代器，能够从迭代器中读取元素
    - OutputIterator 输出迭代器，能够向迭代器中写入元素
    - ForwardIterator 前向迭代器
    - BidirectionalIterator 双向迭代器
    - RandomAccessIterator 随机访问迭代器

## Exceptions

- `throw` 抛出异常，`catch` 捕获异常
    ```cpp
    try {
        // ...
        throw exception(); // throw an exception (a class or other things)
    } catch (exception &e) {
        // ...
        throw; // rethrow the same exception
    } catch (...) {
        // catch all exceptions, must be the last catch block
    }
    ```
- 匹配方式：
    1. 异常类型完全匹配
    2. 可以向上转型
    3. `...` 匹配所有异常
    4. 否则向上抛出
- `noexcept` 承诺不抛出异常，如果抛出异常，程序会调用 `std::terminate` 终止程序
    ```cpp
    void foo() noexcept {
        // ...
    }
    ```
- 如果构造函数抛出异常，析构函数不会被调用，需要手动释放资源
- RAII（Resource Acquisition Is Initialization）：资源获取即初始化。
    用栈对象管理堆资源：
    ```cpp
    class Wrapper {
    private:
        int *vdata;
    public:
        Wrapper(int *vdata) : vdata(vdata) {}
        ~Wrapper() { delete vdata; }
    };
    class Data {
    private:
        Wrapper w;
    public:
        Data() : w(new int[10]) {
            // ...
            throw exception(); // 如果抛出异常，Wrapper 的析构函数会被调用，释放资源
        }
        ~Data() {} // Wrapper 的析构函数会被调用，释放资源
    };
    ```
    这样不会发生构造函数抛出异常时资源泄漏的问题（可以使用 `std::unique_ptr` 代替 `Wrapper`）
- 如果析构函数抛出异常，会导致栈推出时调用自己的析构函数，此时程序会调用 `std::terminate` 终止程序
- 一般使用引用 catch 异常，避免拷贝异常对象或者内存泄漏

## Smart Pointers

- `unique_ptr` 独占指针，只能有一个指向对象，析构时释放资源
    ```cpp
    std::unique_ptr<T> up = std::make_unique<T>(args);
    std::unique_ptr<T> up(new T(args));
    ```
    `args` 为 `T` 的构造函数参数，例如：
    ```cpp
    struct A {
        A(int x, int y) {}
    };
    std::unique_ptr<A> up = std::make_unique<A>(1, 2);
    ```
    `unique_ptr` 不能拷贝，拷贝语义均被禁用，只能移动（使用 `std::move`）
    ```cpp
    std::unique_ptr<A> up1 = std::make_unique<A>(1, 2);
    auto up2 = std::move(up1); // OK
    auto up3 = up1; // Error
    auto up3(up1); // Error
    ```
- `shared_ptr` 共享指针，引用计数，析构时引用计数减一，为 0 时释放资源
    ```cpp
    std::shared_ptr<T> sp = std::make_shared<T>(args);
    ```
    `shared_ptr` 可以拷贝，引用计数加一
    ```cpp
    std::shared_ptr<A> sp1 = std::make_shared<A>(1, 2);
    auto sp2 = sp1; // OK
    ```
    `shared_ptr::use_count()` 返回引用计数
- `weak_ptr` 弱指针，不增加引用计数，可以由 `shared_ptr` 拷贝，解决 `shared_ptr` 循环引用问题
- `get()` 返回原始指针
- UCPointer
    <div align="center"><img src="/assets/img/CS/oop/ucpointer.png" width="50%"></div>

    - UCObject 基类，包含引用计数
    - String Rep 继承自 UCObject，包含字符串数据
    - UCPointer 智能指针，模版类，包含指向 UCObject 的指针
    - String 包含 UCPointer，实现字符串操作

## Misc Topics

### Named Casts

- `static_cast` 

    ```cpp
    double d = 7.1;
    int i;
    i = d; // implicit conversion
    i = (int) d; // C-style cast, explicit conversion
    i = static_cast<int>(d); // static_cast, explicit conversion
    ```

- `reinterpret_cast`

    ```cpp
    int a = 1;
    double *p;
    p = (double *) &a; // C-style cast
    p = static_cast<double *>(&a); // error
    p = reinterpret_cast<double *>(&a); // reinterpret_cast, but not safe
    ```

- `const_cast`

    ```cpp
    const int a = 1;
    int *p;
    p = *a; // error
    p = (int *) &a; // C-style cast
    p = static_cast<int *>(&a); // error
    p = const_cast<int *>(&a); // const_cast, remove const qualifier, but can't modify a
    ```

- `dynamic_cast` 根据动态类型转换指针或引用，需要基类是多态类

    ```cpp
    struct A {
        virtual void foo() {}
    };
    struct B : A {};
    struct C : A {};
    A *pa = new B;
    C *pc = static_cast<C *>(pa); // ok, but will fail if access things in C
    C *pc = dynamic_cast<C *>(pa); // ok, return nullptr if fail, runtime check
    ```

### Multiple Inheritance

```cpp
class A {};
class B {};
class C : public A, public B {};
```

此时 `C` 包含 `A` 和 `B` 的字段。

- 菱形继承：
    ```cpp
    struct B1 { int mi; };
    struct D1 : public B1 {};
    struct D2 : public B1 {};
    struct MI : public D1, public D2 {};
    
    M m; // OK
    B1 *pb = &m; // Error, ambiguous mi
    B1 *pb = static_cast<D1 *>(&m); // OK, use mi in D1
    B1 *pb = static_cast<D2 *>(&m); // OK, use mi in D2
    m.D1::mi = 1; // OK
    m.D2::mi = 2; // OK
    ```

- 菱形继承解决方案
    - 基类为 interface
    - 使用虚继承
        ```cpp
        struct B1 { int mi; };
        struct D1 : virtual public B1 {};
        struct D2 : virtual public B1 {};
        struct MI : public D1, public D2 {};

        MI m;
        B1 *pb = &m; // OK, only one mi
        ```

### Namespace

避免命名冲突，可以使用命名空间

```cpp
namespace ns {
    int x;
    void foo() {}
}
ns::x = 1;
ns::foo();
```

- alias namespace

    ```cpp
    namespace ns1_longname {
        int x;
        void foo() {}
    }
    namespace ns2 = ns1_longname;
    ns2::x = 1;
    ns2::foo();
    ```
- composition

    ```cpp
    namespace ns1 {}
    namespace ns2 {}
    namespace all {
        using namespace ns1;
        using namespace ns2;
    }
    ```