## CHAPTER 1 Strings 第一章：字符串 

### 1.7 制表符(Tab键)收缩或者扩展

#### 问题

你想要将字符串中的空格键 `space` 和制表符 `tab` 相互转换，从而使得文本能够以制表符结尾，显得更好看。例如，你想给用户用一种标准化的方式展示一些已经格式化了的文本。

#### 解决方案

- 一般可以用 `str_replace()` 函数将空格转化为制表符或者将制表符转化为空格。

【例1-21】

	$rows = $db->query('SELECT message FROM messages WHERE id = 1');
	$obj = $rows->fetch(PDO::FETCH_OBJ);
	$tabbed = str_replace(' ' , "\t", $obj->message);
	$spaced = str_replace("\t", ' ' , $obj->message);
	print "With Tabs: <pre>$tabbed</pre>";
	print "With Spaces: <pre>$spaced</pre>";

- 需要注意的是，使用 `str_replace()` 函数去做空格和制表符的互转，是没有考虑到字符串中的制表位的。如果你想每 8 个字符都加一个 `tab` 的话，那么有一行是以5个字母和 1 个 `tab` 开始的，那么那一个 `tab` 就应该补全成为 3 个 `tab`(使得总数为 8 ),而不仅仅是 1 个。这时候就需要使用 `tab_expand()` 函数来做 	`tab` 和 `space` 的转换了，同时还可以兼顾制表位。

【例1-22】

	function tab_expand($text) {
		while (strstr($text,"\t")) {
			$text = preg_replace_callback('/^([^\t\n]*)(\t+)/m','tab_expand_helper', $text);
		}
		return $text;
	}

	function tab_expand_helper($matches) {
		$tab_stop = 8;
		return $matches[1] .str_repeat(' ',strlen($matches[2]) *$tab_stop - (strlen($matches[1]) % $tab_stop));
	}
	$spaced = tab_expand($obj->message);

- 同时可以用 `tab_unexpand()` 函数将 `space` 转会 `tab`。

【例1-23】

	function tab_unexpand($text) {
		$tab_stop = 8;
		$lines = explode("\n",$text);
		foreach ($lines as $i => $line) {
			// 把所有的制表符都转化为空格
			$line = tab_expand($line);
			$chunks = str_split($line, $tab_stop);
			$chunkCount = count($chunks);
			// 扫描除了最后一个元素的所有元素
			for ($j = 0; $j < $chunkCount - 1; $j++) {
				$chunks[$j] = preg_replace('/ {2,}$/',"\t",$chunks[$j]);
			}
			// 如果最后的制表位是一个空格
			// 转成 tab，或者不动
			if ($chunks[$chunkCount-1] == str_repeat(' ', $tab_stop)) {
				$chunks[$chunkCount-1] = "\t";
			}
			// 重组数组
			$lines[$i] = implode('',$chunks);
		}
		// 重组字符串
		return implode("\n",$lines);
	}

	$tabbed = tab_unexpand($obj->message);

以上两个函数都是传递一个字符串当作参数，然后返回一个按照要求适当修改以后的字符串。

#### 讨论

- 上面的两个函数都是假设了 1 个制表位是等于 8 个空格的，当然这个约定也是可以被修改的，你可以按照自己的要求去修改这个转换规则。

- 在 `tab_expand()` 函数中，那个正则表达式 `'/^([^\t\n]*)(\t+)/m'` 可以匹配到一个或者多个 `tab` 键已经在这一个或者多个 `tab` 键之前的那些文本。之所以需要匹配 `tab` 键之前的那些文本，是由于在制表符之前的那些文本的长度会影响到后面有多少个空格应该被转化为制表符，从而使得后面的那些文本能够用下一个 `tab` 来和上面的对齐。因此，这个函数并不仅仅是简单的将 1 个 `tab` 用 8 个 `space` 进行替代，而是可以根据 `tab` 进行文本格式的调整，使得每一行都以制表位 `tab stops` 来结尾。

- 类似的,`tab_unexpand()` 函数也不是仅仅只去找到 8 个空格以后把这些空格转变成 1 个制表符。它可以把每一行文本都分成 8 个分片 `chunks`,然后把每一个分片最后结尾的空格都用制表符去替代掉。这样的处理不仅仅用制表符将文本对齐了，还保留了字符串中的空格。

#### 请参阅

- `str_replace()` 函数 [参考手册](http://php.net/manual/zh/function.str-replace.php)
- `preg_replace_callback()` 函数 [参考手册](http://php.net/manual/zh/function.preg-replace-callback.php)
- `str_split()` 函数[参考手册](http://php.net/manual/zh/function.str-split.php)
- 在 23.10 小节会更加详细的讲解 `preg_replace_callback()` 函数

----------


[【返回目录】](/README.md)