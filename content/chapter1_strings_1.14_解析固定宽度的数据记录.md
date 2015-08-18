## CHAPTER 1 Strings 第一章：字符串 

### 1.14 解析固定宽度的数据记录

#### 问题

你需要将字符串中那些固定长度的数据记录打散。

#### 解决方案

- 使用 `substr()` 函数。

	【例1-33】

		$fp = fopen('fixed-width-records.txt','r',true) or die ("can't open file");
		while ($s = fgets($fp,1024)) {
			$fields[1] = substr($s,0,25); // 第一个字段，前 25 个字节
			$fields[2] = substr($s,25,15); // 第二个字段，接下来的 15 个字节
			$fields[3] = substr($s,40,4); // 第三个字段，接下来的 4 个字节
			$fields = array_map('rtrim', $fields); // 去除结尾的空白
			// 处理数据的函数
			process_fields($fields);
		}
		fclose($fp) or die("can't close file");

- 或者使用 `unpack()` 函数。

	【例1-34】

		function fixed_width_unpack($format_string,$data) {
			$r = array();
			for ($i = 0, $j = count($data); $i < $j; $i++) {
				$r[$i] = unpack($format_string,$data[$i]);
			}
			return $r;
		}

#### 讨论

- 每一行有固定字段的那种数据可能看起来像下面的这个包含书单、标题和出版日期的数据一样：

		$booklist=<<<END
		Elmer Gantry Sinclair Lewis 1927
		The Scarlatti InheritanceRobert Ludlum 1971
		The Parsifal Mosaic Robert Ludlum 1982
		Sophie's Choice William Styron 1979
		END;

	在这个数据的每一行，前 25 个字节是标题，接下来的 15 个字节是作者名字，然后出版日期是后面的 4 个字节。 知道这些信息以后，你就可以很容易的用 `substr()` 函数去解析每一行的数据到一个数组当中。

		$books = explode("\n",$booklist);
		for($i = 0, $j = count($books); $i < $j; $i++) {
			$book_array[$i]['title'] = substr($books[$i],0,25);
			$book_array[$i]['author'] = substr($books[$i],25,15);
			$book_array[$i]['publication_year'] = substr($books[$i],40,4);
		}

	把 `$booklist` 解析到一个数组当中，让循环部分的代码的功能看起来是一样的，而不会去考虑数据是来自一个字符串还是从文件中读取的。

- 上面的那个循环部分的代码可以变得更加灵活，你可以指定那些字段名到一个单独的数组当中，从而可以将这个字段数组传递到一个专门用来做解析的函数中去，就像下面【例1-35】中那个 `fixed_width_substr()` 函数一样。

	【例1-35】

		function fixed_width_substr($fields,$data) {
			$r = array();
			for ($i = 0, $j = count($data); $i < $j; $i++) {
				$line_pos = 0;
				foreach($fields as $field_name => $field_length) {
					$r[$i][$field_name] = rtrim(substr($data[$i],$line_pos,$field_length));
					$line_pos += $field_length;
				}
			}
			return $r;
		}
		$book_fields = array('title' => 25,
							'author' => 15,
							'publication_year' => 4);
		$book_array = fixed_width_substr($book_fields,$booklist);

	变量 `$line_pos` 会紧跟在每一个字段后面，并且能够随着每一行的当前的字段宽度而改变自己的值。使用 `rtrim()` 函数可以去除每一个字段多余的空白。

- 在解析字段的这个工作中，你可以使用 `unpack()` 函数作为 `substr()` 的一种替换品。 对于 `substr()` 函数来说，你需要提供字段名字和宽度做为一个联合数组给传递进去，而对于 `unpack()` 函数来说你只需要给一个字符串。用 `unpack()` 函数解析固定宽度字段的数据，看起来和【例1-36】中那个 `fixed_width_substr()` 函数很相似。

	【例1-36】

		function fixed_width_unpack($format_string,$data) {
			$r = array();
			for ($i = 0, $j = count($data); $i < $j; $i++) {
				$r[$i] = unpack($format_string,$data[$i]);
			}
			return $r;
		}


	由于对 `unpack()` 函数来说，传递进去的字符串就是带空格格式 `space-padded` 的字符串，所以就不需要用 `rtrim()` 函数再去消除尾部的空格。

- 一旦格式化的数据被上面任意一个方法解析成 `$book_array`数组以后，这些数据就可以被打印成 HTML 的表格样式，向下面这种：

		$book_array = fixed_width_unpack('A25title/A15author/A4publication_year',$books);
		print "<table>\n";
		// 打印表格抬头
		print '<tr><td>';
		print join('</td><td>',array_keys($book_array[0]));
		print "</td></tr>\n";
		// 打印每一行数据
		foreach ($book_array as $row) {
			print '<tr><td>';
			print join('</td><td>',array_values($row));
			print "</td></tr>\n";
		}
		print "</table>\n";

	
	上面的代码中，我们把 `</td><td>` 加入到每一行的数据中去，想组合出来 HTML 的表格中每一行的数据，但是缺少了第一个 `<td>` 和 最后的一个`</td>`，为了打印出一个完整的表格，我们就先打印 `<tr><td>` 在 `join()` 函数之前，并且在 `join()` 函数之后打印出 `</td></tr>`。

-  如果格式化的数据是字符串的话，那么不论是 `substr()` 函数还是 `unpack()`函数，都可以达到相同的解析效果，但是如果格式化的数据仅仅就是字符串的话，那么 `unpack()` 函数是更好的一个选择。

- 如果你的数据都是一样的长度，那么使用 `str_spilt()` 函数可以让你更加方便快捷的把输入的数据“切成小块”解析出来，并且返回一个部分字符串构成的数组。【例1-37】 展示了如何将一个字符串分隔成 32字节的小块。

	【例1-37】

	$fields = str_split($line_of_data,32);
	// 第一个字段是 0 - 31 字节
	// 第二个字符是 32 - 63 字节
	// 以此类推

#### 请参阅

- `unpack` 函数的更多使用方式可以参考 1.17 小节

- `str_split()` 函数的[参考手册](http://php.net/manual/zh/function.str-split.php)

- `join()` 函数的更多使用方式可以参考 4.8 小节

----------


[【返回目录】](/README.md)