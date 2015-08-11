## CHAPTER 1 Strings 第一章：字符串 

### 1.10 去除字符串中的空白字符

#### 问题

你想去除字符串中开头或者结尾的空白字符。例如，你想在校验用户输入的时候先对数据进行一些清理。

#### 解决方案

- 使用 `ltrim()` 函数、`rtrim()` 函数和 `trim()` 函数。

- `ltrim()` 函数可以去除字符串开头的空白字符，`rtirm()` 函数可以去除字符串结尾的空白，`trim()` 可以同时去除字符串开头和结尾的空白字符。

		$zipcode = trim($_GET['zipcode']);
		$no_linefeed = rtrim($_GET['text']);
		$name = ltrim($_GET['name']);

#### 讨论

- 对于这些函数，空白字符 `whitespace` 被定义为一下的一些字符：换行、回车、空格、横线、tab 和垂直 tab（vertical tab）以及 null。

- 去除字符串中的空白字符不仅可以节省存储空间，还可以方便的在 `<pre>` 标签中展示格式化的数据或者文本。例如，如果你在比较用户输入数值的大小，你就首先要对数据做一些必要的调整（去除前后的空白），如果有一些人错误的输入了 `98052` 同时后边跟着若干个空格当作他们的邮政编码，那么你就能够不会强制报错说这不是一个正确的邮政编码。在精确比较之前也应该要对字符串做 `trim` 处理，例如，你要确定 `salami\n` 和 `salami` 是等价的。还有一点，当你要把字符串存入数据库之前，你最好也能够考虑对字符串做一些去除不必要空格的处理，这是一个好习惯。

- `trim()` 系列函数还可以去除用户指定的字符。把你想去除的字符当作第二个参数传递到函数当中。你也可以用两个点 `..` 来指定一个范围的第一个值和最后一个值：

		// 从开头出去除空格和数字
		print ltrim('10 PRINT A$',' 0..9');
		// 去除结尾的分号
		print rtrim('SELECT * FROM turtles;',';');

	会输出：

		PRINT A$
		SELECT * FROM turtles
 
- 在 PHP 当中，还提供了一个函数 `chop()` 做为 `rtrim()` 函数的同构函数（可以实现完全一样的功能）但是我们还是建议大家都使用 `rtrim()` 函数而不是 `chop()` 函数，因为在 PHP 中， `chop()` 函数与 Perl 中的 `chop()` 函数所实现的功能是不一样的。（同时在 Perl 中 `chop()` 函数不被推荐使用，推荐使用的是 `chomp()` 函数）使用 `chop()` 会让一些人在阅读代码的时候产生一些疑惑，所以还是建议大家都使用 `rtrim()` 函数。

#### 请参阅

- trim() 的[参考手册](http://php.net/manual/zh/function.trim.php)
- ltrim() 的[参考手册](http://php.net/manual/zh/function.ltrim.php) 
- rtrim() 的[参考手册](http://php.net/manual/zh/function.rtrim.php)

----------


[【返回目录】](/README.md)