## CHAPTER 1 Strings 第一章：字符串 

### 1.0 简介 


在PHP当中，字符串是被认为是“字节的序列”，例如 `"We hold these truths to be self-evident” `,或者 `'这本书叫PHP CookBook'`,甚至 `"2114141412"`都是字符串。一般来说，当我们从一个文件读取出来数据或者要把数据输出到浏览器的时候，你的数据将会以字符串 string 的方式展现出来。

PHP的字符串是二进制安全的 (binary-safe)，它可以包含空字节（null），同时可以根据需要增加或缩小。字符串的长度只受限于 PHP 所能获取到的内存大小。

> 通常情况下，PHP 的字符串是 ASCII 字符的字符串(即字符串中只能存在 ASCII 码的字符)，如果你想处理非 ASCII 字符的数据（如 UTF-8 或者其他多字节编码格式的字符）你必须要做额外的编码转化工作（详情见第19章）

与 Perl 以及 Unix shell 类似，字符串可以用一下三种方式进行初始化：用单引号、用双引号、用 “here document” 格式化（heredoc format），相对应的，就会有单引号字符串、双引号字符串，和 heredoc 字符串。分别介绍如下：

- **单引号字符串**

在一个用单引号定义的字符串中，只需要对反斜杠（\）和单引号（'）进行转义即可。如下所示：

    print 'I have gone to the store.';
	print 'I\'ve gone to the store.';
	print 'Would you pay $1.75 for 8 ounces of tap water?';
	print 'In double-quoted strings, newline is represented by \n';

输出为：

	I have gone to the store.
	I've gone to the store.
	Would you pay $1.75 for 8 ounces of tap water?
	In double-quoted strings, newline is represented by \n

> 上面的输出为我们展示的是最原始的输出形态，如果你在浏览器当中去看的话，你会发现所有的内容都会在一行当中输出，这是因为 HTML 有自己的换行标记符（即`<br >`）。

因为 PHP 并不对单引号定义的字符串进行变量替换和转义字符的检查，所以用单引号定义一个字符串是最快并且最直接有效的方式。

- **双引号字符串**

用双引号定义的字符串并不能识别出字符串中的转义字符，但是可以识别出字符串中的变量，因此如果在双引号字符串中出现了变量，则会被变量的值替代。在双引号中能被转义的字符如下表：

【表1-1】

|转义字符 | 解释
|---|---|
|\n |换行
|\r |回车 
|\t |Tab键
|\\ |反斜杠
|\$ |美元符号
|\""|双引号
|\0 through \777| 八进制
|\x0 through \xFF |十六进制

下面是一些双引号字符串的例子【例1-1】：

	print "I've gone to the store.";
	print "The sauce cost \$10.25.";
	$cost = '$10.25';
	print "The sauce cost $cost.";
	print "The sauce cost \$\061\060.\x32\x35.";

【例1-1】输出：

	I've gone to the store.
	The sauce cost $10.25.
	The sauce cost $10.25.
	The sauce cost $10.25.

注意：【例1-1】的最后一行能输出正确的答案，是因为字符 1 的 ASCII 码 用十进制表示是 49，八进制是 061，同理，字符 0 的 ASCII 码用十进制表示是 48，八进制是 060,字符 2 的 ASCII 码用十进制表示是 50，十六进制是 32，字符 5 的 ASCII 码用十进制表示是 53，十六进制是 35.

- **Heredoc-specified 字符串**

heredoc 格式的字符串会识别出所有在双引号字符串中能识别的变量和转义字符，但是它并不会对双引号进行转义。heredoc 字符串以 <<< 和一个标识符开始，这个标识符前后没有任何的空格，同时，hereodc 字符串以这个标识符加上一个分号结尾。【例1-2】告诉我们怎么定义一个 heredoc 字符串。

【例1-2】

	print <<< END
	It's funny when signs say things like:
	Original "Root" Beer
	"Free" Gift
	Shoes cleaned while "you" wait
	or have other misquoted words.
	END;

【例1-2】输出：

	It's funny when signs say things like:
	Original "Root" Beer
	"Free" Gift
	Shoes cleaned while "you" wait
	or have other misquoted words.

可以看到在 heredoc 字符串中新的行、空格以及引号这些格式都是会被保留的。根据一般的约定，heredoc 字符串结尾的的标识符通常是全部大写的，并且是这个标识符是大小写敏感的。【例1-3】又展示了两个合法的 heredoc 字符串：

【例1-3】 

	print <<< PARSLEY
	It's easy to grow fresh:
	Parsley
	Chives
	on your windowsill
	PARSLEY;

	print <<< DOGS
	If you like pets, yell out:
	DOGS AND CATS ARE GREAT!
	DOGS;

其实，heredoc 通常情况下是用来输出带有变量的 HTML 文档的，因为你不需要在 HTML 文档中对双引号进行转义。【例1-4】就是用 heredoc 来输出 HTML 文档的例子：

【例1-4】

	if ($remaining_cards > 0) {
		$url = '/deal.php';
		$text = 'Deal More Cards';
	} else {
		$url = '/new-game.php';
		$text = 'Start a New Game';
	}
	print <<< HTML
	There are <b>$remaining_cards</b> left.
	<p>
	<a href="$url">$text</a>
	HTML;

在【例1-4】中，heredoc 的结束标识符后面必须得加一个分号，这是为了告诉 PHP 这个字符串已经结束了。但是在有些情况下，你不应该在最后面加上那个分号。这种情况就是通过字符串链接符将一个普通字符串与 heredoc 字符串进行了连接，请看【例1-5】：

【例1-5】
	
	$html = <<< END
	<div class="$divClass">
	<ul class="$ulClass">
	<li>
	END
	. $listItem . '</li></div>';
	print $html;

当我们给 $divClass、$ulClass、和 $listItem 这三个变量赋给合适的值的时候，【例1-5】会输出：

	<div class="class1">>
	<ul class="class2">
	<li> The List Item </li></div>

在【例1-5】中可以看出，上面的表达式还没有结束，还需要从下一行继续连接，所以不能再这里使用分号。还需要注意的是，为了使 PHP 能够识别出来 heredoc 的结束标识符，字符串连接符（即那个 . ）需要从单独的一行开始进行字符串的连接。

- **Nowdocs 字符串**

nowdocs 字符串和 heredocs 字符串类似，但是 nowdocs 字符串不能识别变量，因此 nowdocs 和 heredocs 之间的区别就可以类比单引号字符串与双引号字符串之间的区别。当你需要把一段非 PHP 代码（例如 JavaScript 等）做为 HTML 文档的一部分进行输出，或者要传递这一段代码给其他的程序块的时候，nowdoc 字符串就会给你带来惊喜。

例如，这是一段 jQuery 代码：

	$js = <<<'__JS__'
	$.ajax({
		'url': '/api/getStock',
		'data': {
		'ticker': 'LNKD'
		},
		'success': function( data ) {
			$( "#stock-price" ).html( "<strong>$" + data + "</strong>" );
		}
	});
	__JS__;
	print $js;

这样你就可以输出这段代码了


**字符引用**

在一个字符串中，单独的字节是可以通过方括号引用的方式取出来，第一个字节是的 index 是 0，【例1-6】展示了如何从字符串中取出一个指定的字节：

【例1-6】 

	$neighbor = 'Hilda';
	print $neighbor[3];

【例1-6】输出：
	d

----------

[【返回目录】](README.md)





