
### underscore 源码学习

#### 1. optimizeCb函数

源码清单与分析：

```
 // 参数： fnc 函数 context 调用这个函数的对象(作用域) argCount 函数的参数个数
 var optimizeCb = function(func, context, argCount) {
  // 判断是否有传入作用域
    if (context === void 0) return func;
    // 根据参数个数的不同返回不同的函数
    switch (argCount == null ? 3 : argCount) {
      case 1: return function(value) {
        return func.call(context, value);
      };
      case 2: return function(value, other) {
        return func.call(context, value, other);
      };
      case 3: return function(value, index, collection) {
        return func.call(context, value, index, collection);
      };
      case 4: return function(accumulator, value, index, collection) {
        return func.call(context, accumulator, value, index, collection);
      };
    }

    return function() {
      return func.apply(context, arguments);
    };
  };

  var fn = optimizeCb(fn, context, argCount);
```

上面的代码执行之后fn的实际值就是:

```
var fn = function(accumulator, value, index, collection) {
       return func.call(context, accumulator, value, index, collection);
};
```

因此执行fn();的时候,就相当于把fnc在作用域context中执行了,

总结:

#### 2. cb()函数

源码

```
  var cb = function(value, context, argCount) {
    if (value == null) return _.identity;
    if (_.isFunction(value)) return optimizeCb(value, context, argCount);
    if (_.isObject(value)) return _.matcher(value);
    return _.property(value);
  };

```
