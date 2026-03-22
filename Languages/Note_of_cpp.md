# 常成员函数及静态成员


## 常成员函数

目的：保证对象不被修改，强制只允许调用 const 成员函数。不改变对象状态，只获得状态。编译器一旦发现在常成员函数中修改数据成员的值，将会报错。

例如
``` cpp
class Player {
public:
    // 普通成员函数
    void move(int x, int y) { /* 修改坐标 */ }

    // 常成员函数
    int getScore() const { // 语法：const
        return score; // 只能读取，不能修改
    }

private:
    int score;
};
```

代码案例与运行案例之一

``` cpp
#include <iostream>

class Rectangle {
private:
    double width, height;
    mutable int debug_call_count = 0; // 用于统计调用次数

public:
    Rectangle(double w, double h) : width(w), height(h) {}

    // 这是一个常成员函数：它计算面积，不改状态
    double getArea() const {
        debug_call_count++; 
        return width * height;
    }

    // 这是一个普通成员函数：它会改变状态
    void setWidth(double w) {
        width = w;
    }

    void printInfo() const {
        std::cout << "Area: " << getArea() << " (Called " << debug_call_count << " times)" << std::endl;
    }
};

int main() {
    Rectangle rect(10, 5);
    const Rectangle const_rect(20, 10);

    rect.setWidth(12);       // OK: 普通对象
    rect.printInfo();        // OK: 普通对象调用 const 函数

    // const_rect.setWidth(25); // 报错！常量对象不能调用非 const 函数
    const_rect.printInfo();     // OK: 常量对象调用 const 函数

    return 0;
}
```

## 静态成员(static成员变量/函数)

静态成员变量：属于**类**本身，而不是某个对象；静态成员函数：可以访问**静态成员变量**，但不能访问普通成员变量（因为没有 this 指针）。

静态变量在类内只是声明，通常必须在类外进行定义和初始化（因为它不随对象创建而分配空间）。

定义方法（内部声明和类外初始化）

```cpp
class BankAccount {
public:
    static double interest_rate; // 类内声明 // 语法：static
};

// 类外初始化（不加 static 关键字，但要加类名作用域 ::）
double BankAccount::interest_rate = 0.05; 

int main() {
    BankAccount a, b;
    // a 和 b 共享同一个 interest_rate
}
```

**静态成员与普通成员最大的区别：没有 this 指针。**

普通成员函数在调用时会隐式传递 this 指针，指向当前对象。而静态成员函数没有 this 指针。

这意味着：

-它不能访问非静态成员变量（因为它不知道该找哪个对象的变量）。

-它不能调用非静态成员函数。

-它可以直接通过类名调用，无需创建对象。

```
class Player {
private:
    static int total_players;
    int id;

public:
    Player() { total_players++; }
    
    static int getTotal() { 
        // return id; // 错误！静态函数看不到具体的 id
        return total_players; // 正确：只能访问静态成员
    }
};

// 调用方式
int count = Player::getTotal(); // 优雅，不需要实例化
```

类的静态数据成员对该类的所有对象只有一个拷贝。

静态成员除了通过对象来访问外，也可以直接通过类来访问。如下。

```
Player Misaki;

...... 

cout << misaki.getTotal(); // 或者写成 cout << A::getTotal(); // 记得写括号调用函数

```

