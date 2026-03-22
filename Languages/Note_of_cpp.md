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
    int getScore() const { 
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
    static double interest_rate; // 类内声明
};

// 类外初始化（不加 static 关键字，但要加类名作用域 ::）
double BankAccount::interest_rate = 0.05; 

int main() {
    BankAccount a, b;
    // a 和 b 共享同一个 interest_rate
}
```

