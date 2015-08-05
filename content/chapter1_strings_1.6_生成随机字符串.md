## CHAPTER 1 Strings 第一章：字符串 

### 1.6 生成随机字符串

#### 问题

你想生成一个随机字符串。

#### 解决方案

使用 `str_rand()` 函数。

【例1-18】

	function str_rand($length = 32,$characters = '0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ') {
		if (!is_int($length) || $length < 0) {
			return false;
		}
		$characters_length = strlen($characters) - 1;
		$string = '';	
		for ($i = $length; $i > 0; $i--) {
			$string .= $characters[mt_rand(0, $characters_length)];
		}
		return $string;
	}
	
#### 讨论

- PHP 提供了若干个生成随机数字的原生函数，但是并没有一个函数能够产生随机字符串。`str_rand()` 函数返回了一个由数字和字母组成的长度为32字节的随机字符串。

- 你可以改变 `$length` 的值来根据实际需要返回一个字符串。如果你提供了限定范围的字符，只需要将第二个参数 `$characters` 换成你想要的字符范围就好（即传递一个整数到第一个参数，表示想要的随机字符串的长度，传递一个字符串到第二个参数，表示随机字符串中字符的可选范围），例如，你想获取一个随机的 16 位的摩尔斯电码，你可以这样做：

随机16位摩尔斯电码：

	print str_rand(16, '.-');
	//输出
	.--..-.-.--.----

#### 请参阅

第 2.5 小节有讨论怎么获取随机数字

----------


[【返回目录】](/README.md)