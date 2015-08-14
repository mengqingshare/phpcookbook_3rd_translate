## CHAPTER 1 Strings 第一章：字符串 

### 1.13 生成固定宽度的数据记录

#### 问题

你需要格式化数据，使得每一个字段都能够有固定的宽度。

#### 解决方案

使用带有指定了在字符串前后添加一系列空格的格式化字符串的 `pack()` 函数可以实现。【例1-31】将一个数组中的元素转变成了固定宽度的数据集。

【例1-31】

	$books = array( array('Elmer Gantry', 'Sinclair Lewis', 1927),
					array('The Scarlatti Inheritance','Robert Ludlum', 1971),
					array('The Parsifal Mosaic','William Styron', 1979) );
	foreach ($books as $book) {
		print pack('A25A15A4', $book[0], $book[1], $book[2]) . "\n";
	}

#### 讨论

- 这个格式化字符串 `A25A15A4` 告诉了 `pack()` 函数要为后面来的参数留下 25 字节的空间、14 字节的空间和4个字节的空间，由此可见， `pack()` 函数为固定宽度的空格字段提供了一个简洁有效的处理方案。

- 如果要在字符串后面填充其他的字符而不是空格的话，你需要用 `substr()` 函数保证要填的字符串不会太长，用 `str_pad()` 函数保证字符串不会太短。【例1-31】将一个数组中的元素转变成了固定宽度的数据集，用 `.` 去填充了后面不足的字节。


	【例1-32】

		$books = array( array('Elmer Gantry', 'Sinclair Lewis', 1927),
						array('The Scarlatti Inheritance','Robert Ludlum', 1971),
						array('The Parsifal Mosaic','William Styron', 1979) );
		foreach ($books as $book) {
			$title = str_pad(substr($book[0], 0, 25), 25, '.');
			$author = str_pad(substr($book[1], 0, 15), 15, '.');
			$year = str_pad(substr($book[2], 0, 4), 4, '.');
			print "$title$author$year\n";
		}

#### 请参阅

- `pack()` 函数的[参考手册](http://php.net/manual/zh/function.pack.php)

- `str_pad()` 函数的[参考手册](http://php.net/manual/zh/function.str-pad.php)

- 第 1.17 小节有仔细讨论 `pack()` 函数是怎么格式化数据的。



----------


[【返回目录】](/README.md)