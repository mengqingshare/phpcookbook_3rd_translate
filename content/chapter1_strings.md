## CHAPTER 1 Strings 第一章：字符串 

### 1.0 简介 


在PHP当中，字符串是被认为是“字节的序列”，例如`"We hold these truths to be self-evident” `,或者`'这本书叫PHP CookBook'`,甚至`"2114141412"`都是字符串。一般来说，当我们从一个文件读取出来数据或者要把数据输出到浏览器的时候，你的数据将会以字符串 string 的方式展现出来。

PHP的字符串是二进制安全的(binary-safe)，它可以包含空字节（null），同时可以根据需要增加或缩小。字符串的长度只受限于PHP所能获取到的内存大小。

> 通常情况下，PHP的字符串是 ASCII 字符的字符串(即字符串中只能存在ASCII码的字符)，如果你想处理非ASCII字符的数据（如UTF-8或者其他多字节编码格式的字符）你必须要做额外的编码转化工作（详情见第19章）

与 Perl 以及 Unix shell 类似，字符串可以用一下三种方式进行初始化：用单引号、用双引号、用“here document”格式化（heredoc format）。

- 单引号字符串

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

- 双引号字符串

用双引号定义的字符串并不能识别出字符串中的转义字符，但是可以识别出字符串中的变量，因此如果在双引号字符串中出现了变量，则会被变量的值替代。在双引号中能被转义的字符如下表：

表1-1

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

下面是一些双引号字符串的例子：

	print "I've gone to the store.";
	print "The sauce cost \$10.25.";
	$cost = '$10.25';
	print "The sauce cost $cost.";
	print "The sauce cost \$\061\060.\x32\x35.";

输出：

	I've gone to the store.
	The sauce cost $10.25.
	The sauce cost $10.25.
	The sauce cost $10.25.



