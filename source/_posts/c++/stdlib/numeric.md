---
date: 2021-05-13 14:26:52
title: numeric
tags:
- stdlib
categories:
- [c++, stdlib]
---

# numeric

## accumulate

- 无自定义函数版本

  - 源码

    ```c++
    /**
       *  @brief  Accumulate values in a range.
       *
       *  Accumulates the values in the range [first,last) using operator+().  The
       *  initial value is @a init.  The values are processed in order.
       *
       *  @param  __first  Start of range.
       *  @param  __last  End of range.
       *  @param  __init  Starting value to add other values to.
       *  @return  The final sum.
       */
      template<typename _InputIterator, typename _Tp>
        inline _Tp
        accumulate(_InputIterator __first, _InputIterator __last, _Tp __init)
        {
          // concept requirements
          __glibcxx_function_requires(_InputIteratorConcept<_InputIterator>)
          __glibcxx_requires_valid_range(__first, __last);
    
          for (; __first != __last; ++__first)
    	__init = __init + *__first;
          return __init;
    }
    ```

  - 测试

    ```c++
    int res = std::accumulate(numbers.begin(),numbers.end(),0);
    ```

    

- 自定义操作

  - 源码

    ```c++
      /**
       *  @brief  Accumulate values in a range with operation.
       *
       *  Accumulates the values in the range [first,last) using the function
       *  object @p __binary_op.  The initial value is @p __init.  The values are
       *  processed in order.
       *
       *  @param  __first  Start of range.
       *  @param  __last  End of range.
       *  @param  __init  Starting value to add other values to.
       *  @param  __binary_op  Function object to accumulate with.
       *  @return  The final sum.
       */
      template<typename _InputIterator, typename _Tp, typename _BinaryOperation>
        inline _Tp
        accumulate(_InputIterator __first, _InputIterator __last, _Tp __init,
    	       _BinaryOperation __binary_op)
        {
          // concept requirements
          __glibcxx_function_requires(_InputIteratorConcept<_InputIterator>)
          __glibcxx_requires_valid_range(__first, __last);
    
          for (; __first != __last; ++__first)
    	__init = __binary_op(__init, *__first);
          return __init;
    }
    ```

  - 测试

    ```
    int myfunction (int x, int y) {return x+2*y;}
    struct myclass {
    	int operator()(int x, int y) {return x+3*y;}
    } myobject;
    int res = std::accumulate(numbers.begin(),numbers.end(),0,myfunction);
    int res = std::accumulate(numbers.begin(),numbers.end(),0,myobject);
    // return 减去这些元素
    int res = std::accumulate(numbers.begin(),numbers.end(),0,std::minus<int>());
    // 匿名函数
    string res = std::accumulate(numbers.begin(),numbers.end(),std::string{"str:"},[](std::string str,int i){return str+=std::to_string(i);});
    ```

    