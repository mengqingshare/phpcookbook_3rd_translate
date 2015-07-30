## CHAPTER 1 Strings 第一章：字符串 

### 1.2 提取字符串 

#### 问题
 
你想从一个给定的字符串中截取出从指定位置开始的一个子字符串。例如，你想取出来自一个表单的`username` 这个字符串的前8个字符。

#### 解决方案

使用 `substr()` 函数来取出你想要的字符串。

【例1-8】

	$substring = substr($string,$start,$length);
	//实例
	$username = substr($_GET['username'],0,8);

#### 讨论

- 如果 `$start` 和 `$length` 都是正数，`substr()` 函数返回 `$string`这个字符串从 `$start` 位置开始的 `$length` 个字符。注意第一个字符是从位置 `0` 开始的。

【例1-9】

	print substr('watch out for that tree',6,5);

【例1-9】输出

	out f

- 如果你将 `$length` 留空，那么 `substr()` 函数将会返回 `$string` 字符串从 `$start` 位置开始一直到字符串结束的子字符串。

【例1-10】
	
	print substr('watch out for that tree',17);

【例1-10】输出

	t tree

- 如果 `$start` 的值大于给定字符串的长度，那么 `substr()` 函数将会返回 `false`	

- 如果 `$start + $length` 的值大于给定字符串的长度，那么 `substr()` 函数将会返回从 `$start` 开始一直到此字符串结束的所有字符。

【例1-11】

	print substr('watch out for that tree',20,5);

【例1-11】输出

	ree

- 如果 `$start` 是个负数，那么 `substr()` 函数将会返回从给定字符串的结尾**倒数** `$start` 个的位置开始，正向截取 `$length` 个字符。

【例1-12】

	print substr('watch out for that tree',-6);
	print substr('watch out for that tree',-17,5);

【例1-12】输出

	t tree
	out f

- 如果 `$start` 是一个负数其绝对值已经超过了指定字符串的长度（例如 `$start` 是 27 但是字符串的长度是 20），那么 `substr()` 函数将会按照 `$start = 0` 来处理 

- 如果 `$length` 是一个负数，那么 `substr()` 函数将会从字符串的结尾开始倒数 `$length` 个字符，把这个位置当作你的子字符串结尾的位置（即从字符串结尾处到 `$length` 的绝对值长度的字符都不会被取到）。

【例1-13】

	print substr('watch out for that tree',15,-2);
	print substr('watch out for that tree',-4,-1);

【例1-13】输出

	hat tr
	tre

#### 请参阅


`substr()` 函数的[使用方法](http://www.php.net/substr)

----------


[【返回目录】](/README.md)