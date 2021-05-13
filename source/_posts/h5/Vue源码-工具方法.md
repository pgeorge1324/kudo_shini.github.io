---
date: 2021-05-13 14:26:52
title: Vue源码-工具方法
tags:
- h5
- js
- vue
categories:
- [h5, js]
- [h5, vue]
---

# Vue源码-工具方法

- ## 空对象

  ```javascript
  //Object.freeze()阻止修改现有属性的特性和值，并阻止添加新属性。
  var emptyObject = Object.freeze({});
  ```

- ## 数值校验

  - ### 数据undefined或null

    ```javascript
    function isUndef(v) {
            return v === undefined || v === null
        }
    ```

  - ### 数据不是undfined且不是null

    ```javascript
    function isDef(v) {
            return v !== undefined && v !== null
        }
    ```

  - ### 数据是否是对应bool值

    ```javascript
    function isTrue(v) {
            return v === true
        }
    function isFalse(v) {
            return v === false
        }
    ```

  - ### 数据是否是简单数据类型

    ```javascript
    /**
    * Check if value is primitive
    */
    function isPrimitive(value) {
        //判断数据类型是否是string，number，symbol，boolean
        return (
        typeof value === 'string' ||
        typeof value === 'number' ||
        // $flow-disable-line
        typeof value === 'symbol' ||
        typeof value === 'boolean'
        )
    }
    ```

- ## 判断对象类型

  ```javascript
  	/**
       * Quick object check - this is primarily used to tell
       * Objects from primitive values when we know the value
       * is a JSON-compliant type.
       */
      function isObject(obj) {
          //判断是否是对象
          return obj !== null && typeof obj === 'object'
      }
  
      /**
       * Get the raw type string of a value e.g. [object Object]
       */
          //获取toString 简写
      var _toString = Object.prototype.toString;
  
      function toRawType(value) {
          //类型判断 返会Array ，Function，String,Object,Re 等
          return _toString.call(value).slice(8, -1)
      }
  
      /**
       * Strict object type check. Only returns true
       * for plain JavaScript objects.
       */
      function isPlainObject(obj) {
          return _toString.call(obj) === '[object Object]'
      }
  
      function isRegExp(v) {
          return _toString.call(v) === '[object RegExp]'
      }
  
      /**
       * Check if val is a valid array index.
       */
      /**
       * Check if val is a valid array index.
       * 检查VAL是否是有效的数组索引。
       */
      function isValidArrayIndex(val) {
          //isFinite 检测是否是数据
          var n = parseFloat(String(val));
          //isFinite 如果 number 是有限数字（或可转换为有限数字），那么返回 true。否则，如果 number 是 NaN（非数字），或者是正、负无穷大的数，则返回 false。
          return n >= 0 && Math.floor(n) === n && isFinite(val)
      }
  ```

- ## 数据转换

  ```javascript
  	/**
       * Convert a value to a string that is actually rendered.
       */
      function toString(val) {
  
          //将对象或者其他基本数据 变成一个 字符串
          return val == null
              ? ''
              : typeof val === 'object'
              ? JSON.stringify(val, null, 2)
              : String(val)
      }
  
      /**
       * Convert a input value to a number for persistence.
       * If the conversion fails, return original string.
       */
      function toNumber(val) {
          //字符串转数字，如果失败则返回字符串
          var n = parseFloat(val);
          return isNaN(n) ? val : n
      }
  ```

- ## 字典处理内置属性

  ```javascript
  /**
       * Make a map and return a function for checking if a key
       * is in that map.
       *
       *  //map 对象中的[name1,name2,name3,name4]  变成这样的map{name1:true,name2:true,name3:true,name4:true}
       *  并且传进一个key值取值，这里用到策略者模式
       */
      function makeMap(str,expectsLowerCase) {
          var map = Object.create(null);   //创建一个新的对象
          var list = str.split(',');    //按字符串,分割
          for (var i = 0; i < list.length; i++) {
              map[list[i]] = true;   //map 对象中的[name1,name2,name3,name4]  变成这样的map{name1:true,name2:true,name3:true,name4:true}
          }
          return expectsLowerCase
              ? function (val) {
              return map[val.toLowerCase()];
          }   //返回一个柯里化函数 toLowerCase转换成小写
              : function (val) {
              return map[val];
          }   //返回一个柯里化函数 并且把map中添加一个 属性建
      }
  
      /**
       * Check if a tag is a built-in tag.
       * 检查标记是否为内置标记。
       */
      var isBuiltInTag = makeMap('slot,component', true);
  
      /**
       * Check if a attribute is a reserved attribute.
       * 检查属性是否为保留属性。
       * isReservedAttribute=function(vale){ map{key:true,ref:true,slot-scope:true,is:true,vaule:undefined}  }
       */
      var isReservedAttribute = makeMap('key,ref,slot,slot-scope,is');
  ```

- ## 从数组中删除某一项

  ```javascript
  /**
  * Remove an item from an array
  */
  function remove(arr, item) {
      if (arr.length) {
          var index = arr.indexOf(item);
          if (index > -1) {
          return arr.splice(index, 1)
          }
      }
  }
  ```

- ## 其它

    ```javascript
    /**
         * Check whether the object has the property.
         *检查对象属性是否是实例化还是原型上面的
         */
        var hasOwnProperty = Object.prototype.hasOwnProperty;
    
        function hasOwn(obj, key) {
            return hasOwnProperty.call(obj, key)
        }
    
        /**
         * Create a cached version of a pure function.
         */
        /*
         * var aFn =  cached(function(string){
         *
         *      return string
         *  })
         * aFn(string1);
         * aFn(string2);
         * aFn(string);
         * aFn(string1);
         * aFn(string2);
         *
         * aFn 函数会多次调用 里面就能体现了
         *  用对象去缓存记录函数
         * */
    
        function cached(fn) {
            var cache = Object.create(null);
            return (function cachedFn(str) {
                var hit = cache[str];
                return hit || (cache[str] = fn(str))
            })
        }
    
        /**
         * Camelize a hyphen-delimited string.
         * 用连字符分隔的字符串。
         * camelize = cachedFn(str)=>{ var hit = cache[str];
        return hit || (cache[str] = fn(str))}
    
         调用一个camelize 存一个建进来 调用两次 如果建一样就返回 hit
    
         横线-的转换成驼峰写法
         可以让这样的的属性 v-model 变成 vModel
         */
        var camelizeRE = /-(\w)/g;
        var camelize = cached(function (str) {
            return str.replace(camelizeRE, function (_, c) {
                return c ? c.toUpperCase() : '';
            })
        });
    
        /**
         * Capitalize a string.  将首字母变成大写。
         */
        var capitalize = cached(function (str) {
            return str.charAt(0).toUpperCase() + str.slice(1)
        });
    
        /**
         * Hyphenate a camelCase string.
         * \B的用法
         \B是非单词分界符，即可以查出是否包含某个字，如“ABCDEFGHIJK”中是否包含“BCDEFGHIJK”这个字。
         */
        var hyphenateRE = /\B([A-Z])/g;
        var hyphenate = cached(function (str) {
            //大写字母，加完减号又转成小写了 比如把驼峰 aBc 变成了 a-bc
            //匹配大写字母并且两面不是空白的 替换成 '-' + '字母' 在全部转换成小写
            return str.replace(hyphenateRE, '-$1').toLowerCase();
        });
    
        /**
         * Simple bind polyfill for environments that do not support it... e.g.
         * PhantomJS 1.x. Technically we don't need this anymore since native bind is
         * now more performant in most browsers, but removing it would be breaking for
         * code that was able to run in PhantomJS 1.x, so this must be kept for
         * backwards compatibility.
         */
    
        /* istanbul ignore next */
        function polyfillBind(fn, ctx) {
            function boundFn(a) {
                var l = arguments.length;
                return l
                    ? l > 1
                    ? fn.apply(ctx, arguments)
                    : fn.call(ctx, a)
                    : fn.call(ctx)
            }
    
            boundFn._length = fn.length;
            return boundFn
        }
    
        function nativeBind(fn, ctx) {
            return fn.bind(ctx)
        }
    
        var bind = Function.prototype.bind
            ? nativeBind
            : polyfillBind;
    
        /**
         * Convert an Array-like object to a real Array.
         */
        function toArray(list, start) {
            start = start || 0;
            var i = list.length - start;
            var ret = new Array(i);
            while (i--) {
                ret[i] = list[i + start];
            }
            return ret
        }
    
        /**
         * Mix properties into target object.
         */
        function extend(to, _from) {
            for (var key in _from) {
                to[key] = _from[key];
            }
            return to
        }
    
        /**
         * Merge an Array of Objects into a single Object.
         */
        function toObject(arr) {
            var res = {};
            for (var i = 0; i < arr.length; i++) {
                if (arr[i]) {
                    extend(res, arr[i]);
                }
            }
            return res
        }
    
        /**
         * Perform no operation.
         * Stubbing args to make Flow happy without leaving useless transpiled code
         * with ...rest (https://flow.org/blog/2017/05/07/Strict-Function-Call-Arity/)
    
         */
        function noop(a, b, c) {
        }
    
        /**
         * Always return false.
         * 返回假的
         */
        var no = function (a, b, c) {
            return false;
        };
    
        /**
         * Return same value
         *返回相同值
         */
        var identity = function (_) {
            return _;
        };
    
        /**
         * Generate a static keys string from compiler modules.
         *
         *    [{ staticKeys:1},{staticKeys:2},{staticKeys:3}]
         * 连接数组对象中的 staticKeys key值，连接成一个字符串 str=‘1,2,3’
         */
        function genStaticKeys(modules) {
            return modules.reduce(
                            function (keys, m) {
                                            //累加staticKeys的值变成数组
                                              return keys.concat(m.staticKeys || [])
                                          },
                             []
            ).join(',') //转换成字符串
        }
    
        /**
         * Check if two values are loosely equal - that is,
         * if they are plain objects, do they have the same shape?
         * 检测a和b的数据类型，是否是不是数组或者对象，对象的key长度一样即可，数组长度一样即可
         */
        function looseEqual(a, b) {
            if (a === b) {
                return true
            }  //如果a和b是完全相等 则true
            var isObjectA = isObject(a);
            var isObjectB = isObject(b);
            if (isObjectA && isObjectB) {  //如果a和都是对象则让下走
                try {
                    var isArrayA = Array.isArray(a);
                    var isArrayB = Array.isArray(b);
                    if (isArrayA && isArrayB) {  //如果a和b都是数组
                        // every  条件判断
                        return a.length === b.length && a.every(function (e, i) {  //如果a长度和b长度一样的时候
                                return looseEqual(e, b[i])  //递归
                            })
                    } else if (!isArrayA && !isArrayB) {  //或者a和b都不是数组
                        var keysA = Object.keys(a);  // 获取到a的key值 变成一个数组
                        var keysB = Object.keys(b); // 获取到b的key值 变成一个数组
                        //他们的对象key值长度是一样的时候  则加载every 条件函数
                        return keysA.length === keysB.length && keysA.every(function (key) {
                                //递归 a和b的值
                                return looseEqual(a[key], b[key])
                            })
                    } else {
                        //如果不是对象跳槽循环
                        /* istanbul ignore next */
                        return false
                    }
                } catch (e) {
                    //如果不是对象跳槽循环
                    /* istanbul ignore next */
                    return false
                }
            } else if (!isObjectA && !isObjectB) {  //b和a 都不是对象的时候
                //把a和b变成字符串，判断他们是否相同
                return String(a) === String(b)
            } else {
                return false
            }
        }
    
    	// 判断 arr数组中的数组 是否和val相等。
    	// 或者 arr数组中的对象，或者对象数组 是否和val 相等
        function looseIndexOf(arr, val) {
            for (var i = 0; i < arr.length; i++) {
                if (looseEqual(arr[i], val)) {
                    return i
                }
            }
            return -1
        }
    
        /**
         * Ensure a function is called only once.
         *  确保该函数只调用一次 闭包函数
         */
        function once(fn) {
            var called = false;
            return function () {
                if (!called) {
                    called = true;
                    fn.apply(this, arguments);
                }
            }
        }
    ```

