# 常成员函数及静态成员


## 常成员函数

目的：保证对象不被修改，强制只允许调用 const 成员函数。不改变对象状态，只获得状态。

例如
```cpp
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

