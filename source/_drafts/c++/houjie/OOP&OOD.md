# OOP面向对象编程

## 组合（Compostion）has-a

- ### 组合关系下的构造和析构函数

  - ##### 构造函数（由内而外）

    - container调用component的构造函数,然后执行自己的构造函数
    - 默认使用component的默认构造函数，定制需在container的函数内部调用

  - ##### 析构函数（由外而内）

    - container先调用自己的析构函数，然后调用component的析构函数

## 委托（Delegation）composition by reference

- 只有在需要component的时候才会去创建component
- ![image-20210222143212049](E:\github\notebook\c++\assets\接口.png)
- （编译防火墙）写时复制

## 继承（Inheritance）is-a

```c++
class foo :public bar{ //通常
	...
}
class foo :private bar{
	...
}
class foo :protected bar{
	...
}
```

- ### 继承关系下的构造和析构函数（base父类 && derived派生类）

  - ##### 构造函数（由内而外）

    - derived调用base的构造函数,然后执行自己的构造函数

  - ##### 析构函数（由外而内）

    - derived先调用自己的析构函数，然后调用base 的析构函数
    - base类的析构函数必须是virtual否则会出现undefined behavior

- ### 虚函数

  - 使用虚函数完成  模板模式

  - ##### 普通函数

    - 不希望派生类override(重写)

  - ##### 虚函数 virtual

    - 希望派生类override,有默认定义
    - virtual function();

  - ##### 纯虚函数 pure virtual

    - 派生类必须override
    - virtual function() = 0;
    - 纯虚函数和空函数有区别

- ### 继承和组合嵌套使用的情况下的顺序？

- ### 委托加继承用于观察者模式

  ![image-20210222154953285](E:\github\notebook\c++\assets\观察者模式.png)

# OOD面向对象设计

### 组合模式

比如文件系统：primitive表示文件，composite表示文件夹 

![image-20210222162154579](E:\github\notebook\c++\assets\组合模式.png)

### 原型模式

 ![image-20210222162726046](E:\github\notebook\c++\assets\原型模式.png)