## CHAPTER 1 Strings 第一章：字符串 

### 1.8 字符串大小写转换

#### 问题

你想讲字符串中的字母进行大写化、小写化或者其他方式的转换。例如，你想将字符串中的第一个字母变成大写但是其他的字母都是小写。

#### 解决方案

- 使用 `ucfirst()` 函数或者 `ucwords()` 来将字符串中的第一个字母或者每一个单词的第一个字母变成大写。

【例1-24】

	print ucfirst("how do you do today?");
	print ucwords("the prince of wales");

【例1-24】输出

	How do you do today?
	The Prince Of Wales

- 使用 `strtolower()` 函数和 `strtoupper()` 函数可以将整个字符串变成小写或者大写。

【例1-25】

	print strtoupper("i'm not yelling!");
	print strtolower('<A HREF="one.php">one</A>');

【例1-25】输出

	I'M NOT YELLING!
	<a href="one.php">one</a>

#### 讨论

- 使用 `ucfirst()` 函数会把字符串中的第一个字母变成大写（如果第一个字符不是字母，就不会变）

		print ucfirst('monkey face');
		print ucfirst('1 monkey face');

	会输出

		Monkey face
		1 monkey face
 
	注意，第二个并没有输出 `1 Monkey face`.

- 使用 `ucwords()` 函数可以把字符串中每个单词的首字母都变成大写。

		print ucwords('1 monkey face');
		print ucwords("don't play zone defense against the philadelphia 76-ers");

	会输出：

		1 Monkey Face
		Don't Play Zone Defense Against The Philadelphia 76-ers		

- 不出意料，`ucwords()` 函数并不会将 `don't` 中的 `t` 变成大写，也不会把 `76-ers` 中的 `e` 变成大写。这是因为，对于 `ucwords()` 函数来说，一个单词 `word` 的定义是：`一个由一个或者多个非空格字符组成的并且跟在一个或者多个空格字符后面的字符序列`,由于 `'` 和 `-` 都不是空格符，所以 `ucwords()` 函数并不会把 `don't` 中的 `t` 和 `76-ers` 中的 `e` 当作是一个单词的开头。

- `ucfirst()` 函数和 `ucwords()` 函数都是只改变首字母，其他字母不会去做任何变化。

		print ucfirst('macWorld says I should get an iBook');
		print ucwords('eTunaFish.com might buy itunaFish.Com!');

	会输出：
		
		MacWorld says I should get an iBook
		ETunaFish.com Might Buy ItunaFish.Com!			

- `strtolower()` 函数和 `strtoupper()` 函数是作用于整个字符串的，而不仅仅是对单个的字符进行处理，换言之，一个字符串中所有的字母都会被 `strtolower()` 函数变成小写，类似的，一个字符串中所有的字母都会被 `strtoupper()` 函数变成大写。

		print strtolower("I programmed the WOPR and the TRS-80.");
		print strtoupper('"since feeling is first" is a poem by e. e. cummings.');
	
	会输出：

		i programmed the wopr and the trs-80.
		"SINCE FEELING IS FIRST" IS A POEM BY E. E. CUMMINGS.

- 同时，当你决定对字符串进行大小写转换的时候，这些函数都会很人性化的根据你的需要做出本地化 `locale settings`的处理。

#### 请参阅

- 有关本地化设置的详细内容，请参照第 19 章

- 函数参考手册如下：
	
	- `ucwords()` 函数 [参考手册](http://php.net/manual/zh/function.ucwords.php)
	- `ucfirst()` 函数 [参考手册](http://php.net/manual/zh/function.ucfirst.php)
	- `strtolower()` 函数 [参考手册](http://php.net/manual/zh/function.strtolower.php)
	- `strtoupper()` 函数 [参考手册](http://php.net/manual/zh/function.strtoupper.php)

----------


[【返回目录】](/README.md)