## CHAPTER 1 Strings 第一章：字符串 

### 1.9 在字符串中插入函数或者表达式

#### 问题

你想把函数或者表达式的结果插入到一个字符串中

#### 解决方案

- 如果你想插入字符串中的那个值不能被直接写在字符串中的话，你可以使用字符串连接符 `.` 来连接。 

	【例1-26】

		print 'You have '.($_POST['boys'] + $_POST['girls']).' children.';
		print "The word '$word' is ".strlen($word).' characters long.';
		print 'You owe '.$amounts['payment'].' immediately.';
		print "My circle's diameter is ".$circle->getDiameter().' inches.';


#### 讨论

- 你可以直接在双引号字符串中插入变量、对象的属性和数组的元素（如果数组的脚标没有引号的话） 

		print "I have $children children.";
		print "You owe $amounts[payment] immediately.";
		print "My circle's diameter is $circle->diameter inches.";

- 改写双引号字符串会有一些语法上的限制，会限制哪些元素能够被插入到双引号字符串中。在下面的例子中，`$amounts['payment']` 必须要写成 `$amounts[payment]` 才能被正确的解析出来。如果还有一些更加复杂的表达式，那么就应该把那些都用大括号扩起来才能被正确的插入。

		print "I have {$children} children.";
		print "You owe {$amounts['payment']} immediately.";
		print "My circle's diameter is {$circle->getDiameter()} inches.";

- 上面所说的两种方式（直接插入和用字符串连接符插入）也可以使用到 `heredoc` 字符串中，然而用字符串连接符方式在 `heredoc` 字符串中看起来有一点点奇怪，因为 `heredoc` 字符串的结尾符号必须要和字符串连接符在不同的行：

		print <<< END
		Right now, the time is
		END
		. strftime('%c') . <<< END
		 but tomorrow it will be
		END
		. strftime('%c',time() + 86400);

- 还要注意的是，如果你要在 `heredoc` 字符串中插入变量什么的，必须要确保你添加了适当的空格符号来使得整个字符串看起来是正确的。在上面的那个例子当中，`Right now, the time is` 后面必须要有一个结尾的空格，而 `but tomorrow it will be` 必须要有开头的空格和结尾的空格。

#### 请参阅

- 了解更多有关插入变量的语法（类似于 `${"amount_$i"}` 这样的）可以看 5.4 小节。

- 字符串连接符 `.` 的[参考手册](http://php.net/manual/zh/language.operators.string.php)。


----------


[【返回目录】](/README.md)