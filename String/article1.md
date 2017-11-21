#### 字符在es6中表示

在es5中，js采用\uxxxx形式表示一个字符，其中xxxx表示字符的 Unicode 码点。但是此种方式不支持表示双字节的字符，也就是说如果某个字符的Unicode码点超出\u0000~\uFFFF这个范围，必须用两个双字节的形式表示。如：

```javascript
  "\uD842\uDFB7"
  // "𠮷"

  "\u20BB7" //此处es5中解析为'\u20bb'+'\u7'，其中'\u20bb'为不可打印字符，所以最终结果是' 7'
  // " 7"
```

但是在es6中，是可以支持双字节字符的，只要把该字符的Unicode码用大括号包起来！如：

```javascript
    "\u{20BB7}"
    // "𠮷"

    "\u{41}\u{42}\u{43}"
    // "ABC"

    let hello = 123;
    hell\u{6F} // 123

    '\u{1F680}' === '\uD83D\uDE80'
    // true
```

所以在es6中，表示一个字符，可以有以下六种方式，（以表示字符‘z’为例）如：

```javascript
  'z' === 'z' //true
  //此种是最原始的方法
  '\z' === 'z'  // true
  //加/的方法
  '\172' === 'z' // true
  //八进制
  '\x7A' === 'z' // true
  //2位16进制Unicode
  '\u007A' === 'z' // true
  //4位16进制Unicode
  '\u{7A}' === 'z' // true
  //es6加大括号法，支持双字节字符
```

#### 字符转Unicode码

在es5中主要是在字符实例上调用调用charCodeAt方法，如：

```javascript
  var str = 'hello world!';
  var strCode = str.charCodeAt(0);
  //104
```
但是这种方法在用在某些双字节字符上的时候，会有点意外情况，所以es6中支持codePointAt方法，来读取双字节字符的Unicode码，其使用方式更charCodeAt一样，如：

```javascript
  var str = '𠮷'
  var strCode = str.codePointAt(0)
  //20bb7
```

#### Unicode码转字符

在es5中主要在String对象上调用fromCharCode方法，如：

```javascript
  var a = String.fromChatCode()
```
