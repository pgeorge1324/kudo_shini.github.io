# reference 引用

```c++
int x = 0;
int* p = &x;// pointer
int& r = x; // reference
// sizeof(r) == sizeof(x)
// &x == &r;
double imag(const double& im){}
double imag(const double im){}
// same signature 函数签名相同，产生二义性
// const是函数签名的一部分
// 所以const也可以重载
```

