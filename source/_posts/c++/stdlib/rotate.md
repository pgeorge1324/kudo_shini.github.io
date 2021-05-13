---
date: 2021-05-13 14:26:52
title: rotate
tags:
- stdlib
categories:
- [c++, stdlib]
---

# rotate

```c++
template <class ForwardIterator>
  void rotate (ForwardIterator first, ForwardIterator middle,
               ForwardIterator last)
{
  ForwardIterator next = middle;
    // 递归的交换
  while (first!=next)
  {
    swap (*first++,*next++);
    if (next==last) next=middle;
    else if (first==middle) middle=next;
  }
}
```

