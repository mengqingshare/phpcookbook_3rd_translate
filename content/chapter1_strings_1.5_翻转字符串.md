## CHAPTER 1 Strings 第一章：字符串 

### 1.5 翻转字符串

#### 问题

你想把字符串中的单词或者字节翻转输出。（原字符串为 `I am lkn`，翻转后是 `nkl ma I` 或者 `lkn am I`）

#### 解决方案

- 使用 `strrev()` 函数可以逐字节对字符串进行翻转操作。

【例1-18】

	print strrev('This is not a palindrome.');

【例1-18】输出：

	.emordnilap a ton si sihT

- 如果要按照单词去翻转字符串，首先要将字符串划分成一个由单词构成的数组，然后翻转这个数组的元素，最后把数组重组成一个字符串输出。

【例1-19】

	$s = "Once upon a time there was a turtle.";
	// 打散字符串成数组
	$words = explode(' ',$s);
	// 翻转数组元素
	$words = array_reverse($words);
	// 重组成字符串
	$s = implode(' ',$words);
	print $s;

【例1-19】输出

	turtle. a was there time a upon Once

#### 讨论

上面所说的第二种，“按照单词翻转字符串”，可以用一行代码实现功能：

【例1-20】

	$reversed_s = implode(' ',array_reverse(explode(' ',$s)));

输出也是 `turtle. a was there time a upon Once`

#### 请参阅

在 24.7 小节（抱歉还未翻译到）会讨论到用其他字符来做为字符串中单词的分隔符（而不仅仅是空格）。

其他资料，可以参考 `strrev()` [参考手册](http://php.net/manual/zh/function.strrev.php) 和 `array_-reverse()` [参考手册](http://php.net/manual/zh/function.array-reverse.php)。

----------


[【返回目录】](/README.md)