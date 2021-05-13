# conversion function转换函数

## 语法

```c++
class Fraction {

public:
    Fraction(int num, int den = 1) : m_son(num), m_mom(den) {}
    // 必须加const
    // 返回类型为double
    // 函数参数this
    operator double const() {
        return (double) (double(m_son) / double(m_mom));
    }
private:
    int m_son;
    int m_mom;
};
```

## 函数调用

```c++
	Fraction f (3,5);
	// 若没有转换函数，此语句报错 有转换函数，编译器调用转换函数将f转换为double
	// 然后编译器继续使用operator(double,int)完成此语句	
	double b = f + 4; 
```

# explicit 显式关键字

## without explicit

##### 不使用explicit时，当operator+操作符查询到只有（const FractionExplicit&f）重载类型时，会自动调用单实参构造函数来进行隐式转换

```c++
class FractionExplicit {

public:
    /**
     * non-explicit-one-argument ctor 非显性单实参构造函数
     * two parameter one argument
     * FractionExplicit d2 = f + 4; // call non-explicit ctor 将4转为 FractionNonExplicit(4,1)
     */
     FractionExplicit(int num, int den = 1) : m_son(num), m_mom(den) {}

    FractionExplicit operator+(const FractionExplicit &f) {
        return FractionExplicit(f.m_son * m_mom + f.m_mom * m_son, f.m_mom * m_mom);
    }

    int son() const {
        return m_son;
    }

    int mom() const {
        return m_mom;
    }

private:
    int m_son;
    int m_mom;
};

FractionExplicit f(3, 5);
// call non-explicit ctor 将4转为 FractionNonExplicit(4,1)
FractionExplicit d2 = f + 4; 
```

## with explicit

```c++
class FractionExplicit {

public:
    /**
     *
     * explicit function
     * 加上explicit，编译器不会自动转换
     */
    explicit FractionExplicit(int num, int den = 1) : m_son(num), m_mom(den) {

    }

    FractionExplicit operator+(const FractionExplicit &f) {
        return FractionExplicit(f.m_son * m_mom + f.m_mom * m_son, f.m_mom * m_mom);
    }

    int son() const {
        return m_son;
    }

    int mom() const {
        return m_mom;
    }

private:
    int m_son;
    int m_mom;
};
FractionExplicit f(3, 5);
// 报错：Invalid operands to binary expression ('FractionExplicit' and 'int')
FractionExplicit d2 = f + 4; 
// 对整型进行显式转换
FractionExplicit d2 = f + FractionExplicit(4); 
```

# 常见错误

## 隐式转换和非显式构造函数导致产生二义性

```c++
class FractionExplicit {

public:
    /**
     * non-explicit-one-argument ctor 非显性单实参构造函数
     * two parameter one argument
     * FractionExplicit d2 = f + 4; // call non-explicit ctor 将4转为 FractionNonExplicit(4,1)
     */
     FractionExplicit(int num, int den = 1) : m_son(num), m_mom(den) {

    }

    // 必须加const
    // 返回类型为double
    // 函数参数this
    operator double const() {
        return (double) (double(m_son) / double(m_mom));
    }

    FractionExplicit operator+(const FractionExplicit &f) {
        return FractionExplicit(f.m_son * m_mom + f.m_mom * m_son, f.m_mom * m_mom);
    }

    int son() const {
        return m_son;
    }

    int mom() const {
        return m_mom;
    }

private:
    int m_son;
    int m_mom;
};

FractionExplicit f(3, 5);
// 报错：Use of overloaded operator '+' is ambiguous (with operand types 'FractionExplicit' and 'int')
// 因为操作符重载operator+(const FractionExplicit &f),所以编译器可以执行FractionExplicit(4),再执行加法
// 同时因为该类包含一个隐式转换函数，所以operator+也可以先double(f)然后重载为operator+(double,int)
// 因此编译器会报错，此处产生二义性
FractionExplicit d2 = f + 4;
// explicit FractionExplicit(int num, int den = 1) : m_son(num), m_mom(den) {}
// 此时仍然报错，此时编译器选择operator+(double,int),返回类型为double,不符合
FractionExplicit d2 = f + 4;
// 正确
FractionExplicit d2 = f + FractionExplicit(4);
```

## 多隐式转换造成的二义性

```c++
class Fraction {

public:
    Fraction(int num, int den = 1) : m_son(num), m_mom(den) {}

    // 必须加const
    // 返回类型为double
    // 函数参数this
    operator double const() {
        return (double) (double(m_son) / double(m_mom));
    }

    operator float const() {
        return (float) (float (m_son) / float (m_mom));
    }

private:
    int m_son;
    int m_mom;
};
Fraction f(3,5);
//  不能写成下面这种情形，因为有多个转换函数double() float()，而operator+对float double都有相应的操作符重载 所以需要显示转换f
// 报错：Use of overloaded operator '+' is ambiguous
float a = 4 + f;
// 4.6f
float a = 4 + double (f);
// 4.6f
float b = 4 + float (f);
// double 4.5999999999999996
double c = 4 + double (f);
// float(f)时会丢失部分精度，导致4.6 = 4.5999999999999996 d = 4.5999999046325684
double d = 4 + float (f);
```

- [:bookmark_tabs: github](https://github.com/pgeorge1324/c/tree/master/learnCpp/course2)