---
date: 2021-05-13 14:26:52
title: 对象遍历
tags:
- c++
- unitest
categories:
- [c++, unitest]
---

# googletest

## 断言

| 断言方法  |                   断言结果                    |
| :-------: | :-------------------------------------------: |
| ASSERT_ * |      fatal failures(致命错误，程序中断)       |
| EXPECT_ * | non-fatal failure(非致命错误，适用于普通错误) |

## 基本断言方法

|        fatal assertion         |     **Nonfatal assertion**     |       Verifies       |
| :----------------------------: | :----------------------------: | :------------------: |
| `ASSERT_TRUE(`*condition*`)`;  | `EXPECT_TRUE(`*condition*`)`;  | *condition* is true  |
| `ASSERT_FALSE(`*condition*`)`; | `EXPECT_FALSE(`*condition*`)`; | *condition* is false |

## 二值比较

|         fatal assertion         | **Nonfatal assertion**          | Verifies           |
| :-----------------------------: | ------------------------------- | ------------------ |
| `ASSERT_EQ(`*val1*`,`*val2*`);` | `EXPECT_EQ(`*val1*`,`*val2*`);` | *val1* `==` *val2* |
| `ASSERT_NE(`*val1*`,`*val2*`);` | `EXPECT_NE(`*val1*`,`*val2*`);` | *val1* `!=` *val2* |
| `ASSERT_LT(`*val1*`,`*val2*`);` | `EXPECT_LT(`*val1*`,`*val2*`);` | *val1* `<` *val2*  |
| `ASSERT_LE(`*val1*`,`*val2*`);` | `EXPECT_LE(`*val1*`,`*val2*`);` | *val1* `<=` *val2* |
| `ASSERT_GT(`*val1*`,`*val2*`);` | `EXPECT_GT(`*val1*`,`*val2*`);` | *val1* `>` *val2*  |
| `ASSERT_GE(`*val1*`,`*val2*`);` | `EXPECT_GE(`*val1*`,`*val2*`);` | *val1* `>=` *val2* |

​	ASSERT_EQ（）指针的指针相等。如果在两个C字符串上使用，它会测试它们是否在同一个内存位置，而不是它们具有相同的值。因此，如果你想比较C字符串（例如const char *）的值，使用ASSERT_STREQ（），稍后将会描述。特别地，要断言C字符串为NULL，请使用ASSERT_STREQ（NULL，c_string）。但是，要比较两个字符串对象，应该使用ASSERT_EQ。

​	本节中的宏适用于窄和宽字符串对象（string和wstring）。

## 字符串比较

|            fatal assertion             | **Nonfatal assertion**                 |                           Verifies                           |
| :------------------------------------: | -------------------------------------- | :----------------------------------------------------------: |
|   `ASSERT_STREQ(`*str1*`,`*str2*`);`   | `EXPECT_STREQ(`*str1*`,`_str_2`);`     |           the two C strings have the same content            |
|   `ASSERT_STRNE(`*str1*`,`*str2*`);`   | `EXPECT_STRNE(`*str1*`,`*str2*`);`     |           the two C strings have different content           |
| `ASSERT_STRCASEEQ(`*str1*`,`*str2*`);` | `EXPECT_STRCASEEQ(`*str1*`,`*str2*`);` | the two C strings have the same content, ignoring case(忽略大小写) |
| `ASSERT_STRCASENE(`*str1*`,`*str2*`);` | `EXPECT_STRCASENE(`*str1*`,`*str2*`);` |   the two C strings have different content, ignoring case    |

NULL指针和空字符串被认为是不同的。