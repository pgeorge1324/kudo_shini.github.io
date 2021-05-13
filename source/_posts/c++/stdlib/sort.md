---
date: 2021-05-13 14:26:52
title: sort
tags:
- stdlib
categories:
- [c++, stdlib]
---

# sort

```c++
sort(arr.begin(), arr.end(), greater<int>());
template <class T> struct greater : binary_function <T,T,bool> {
  bool operator() (const T& x, const T& y) const {return x>y;}
};
```

```c++
std::sort (arr.begin(), arr.end(), std::less<int>());
template <class T> struct less : binary_function <T,T,bool> {
  bool operator() (const T& x, const T& y) const {return x<y;}
};
```

