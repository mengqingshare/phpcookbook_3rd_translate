## CHAPTER 1 Strings 第一章：字符串 

### 1.3 替换字符串 

#### 问题

你想用一个不同的字符串替对指定的字符串进行部分或者全部的替换。例如在输出身份证号码之前，为了保密，你想将身份证号码除了最后面的后4位数字之外其余部分都不展示出来。


#### 解决方案

使用 `substr_replace()` 函数。

【例1-14】

	// $old_string 原字符串
	// $new_substring 新字符串
	// $start 替换的起始位置
	$new_string = substr_replace($old_string,$new_substring,$start);
	// $length 替换的长度
	$new_string = substr_replace($old_string,$new_substring,$start,$length);

#### 讨论

- 如果没有填写 `$length` 参数，那么 `substr_replace()` 函数会将原字符串从 `$start` 开始的所有字符串都会替换成新的字符串，如果指定了 `$length` 参数，那么只会从 `$start` 开始替换掉 `$length` 这么多的字符。下面是例子：

【例子】

	print substr_replace('My pet is a blue dog.','fish.',12);
	print substr_replace('My pet is a blue dog.','green',12,4);

	$credit_card = '4111 1111 1111 1111';
	print substr_replace($credit_card,'xxxx ',0,strlen($credit_card)-4);

	My pet is a fish.
	My pet is a green dog

	xxxx 1111

- 如果 `$start` 是负数，那么就会从原字符串的结尾开始倒数 `$start` 位置去做替换。当然可以与 `$length` 做组合使用。

【例子】

	print substr_replace('My pet is a blue dog.','fish.',-9);
	print substr_replace('My pet is a blue dog.','green',-9,4);
	
	My pet is a fish.
	My pet is a green dog.

-  如果 `$start` 和 `$length` 都是 `0` 的话，就会在原字符串的最前面插入新的字符串。

【例子】

	print substr_replace('My pet is a blue dog.','Title: ',0,0);

	Title: My pet is a blue dog.

- `substr_replace()` 函数有一个很常见的使用场景，那就是当你输出的字符串很长，一下子全部输出不合理，你只想输出这个字符串的一部分的时候，这个函数就很有用了。【例1-15】中，我们用一个超级链接展示出来了一个很长的 `message` 的前25个字符和一个字符串，当你点击这个链接，你就会去一个单独的页面看到更多的信息。

【例1-15】

	$r = mysql_query("SELECT id,message FROM messages WHERE id = $id") or die();
	$ob = mysql_fetch_object($r);
	printf('<a href="more-text.php?id=%d">%s</a>',
	$ob->id, substr_replace($ob->message,' ...',25));

在上例子中出现的这个 `more-text.php` 文件页面可以把 `message_id` 传递到一个查询的语句中，获取到全部完整的信息，并且展示这些内容。

#### 请参阅


`substr_replace()` 函数的[使用方法](http://php.net/manual/zh/function.substr-replace.php)


----------


[【返回目录】](/README.md)