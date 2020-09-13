### Python

- [C++调用Python](https://www.cnblogs.com/findumars/p/6142330.html)

- [Python XML解析](http://www.runoob.com/python/python-xml.html)

- [Python XML生成](https://www.cnblogs.com/haigege/p/5712854.html) 

- `print()`函数也可以接受多个字符串，用逗号“,”隔开，就可以连成一串输出：

  ```python
  >>> print('The quick brown fox', 'jumps over', 'the lazy dog')
  The quick brown fox jumps over the lazy dog
  ```

  `print()`会依次打印每个字符串，遇到逗号“,”会输出一个空格。

- 如果字符串里面有很多字符都需要转义，就需要加很多`\`，为了简化，Python还允许用`r''`表示`''`内部的字符串默认不转义。

- 如果字符串内部有很多换行，用`\n`写在一行里不好阅读，为了简化，Python允许用`'''...'''`的格式表示多行内容。

- 只有1个元素的tuple定义时必须加一个逗号`,`，来消除歧义：

  ```python
  >>> t = (1,)
  >>> t
  (1,)
  ```

- Python提供一个`range()`函数，可以生成一个整数序列，再通过`list()`函数可以转换为list。比如`range(5)`生成的序列是从0开始小于5的整数：

  ```python
  >>> list(range(5))
  [0, 1, 2, 3, 4]
  ```

- 在Python中，字符串、整数等都是不可变的，因此，可以放心地作为key。而list是可变的，就不能作为dict的key。

- 赋值语句

  ```python
  a, b = b, a + b
  ```
  相当于
  ```python
  t = (b, a + b) # t是一个tuple
  a = t[0]
  b = t[1]
  ```