## CHAPTER 1 Strings 第一章：字符串 

### 1.11 生成逗号分隔值数据

#### 问题

你想格式化数据，产生逗号分隔值 (CSV) 格式的数据，从而可以导入电子表格 (excel) 或者数据库。

#### 解决方案

使用 `fputcsv()` 函数从一个数组中生成一行格式化的 CSV 数据。例【1-27】把 `$sales` 中的数据写到了一个文件当中。

【例1-27】

	$sales = array( array('Northeast','2005-01-01','2005-02-01',12.54),
					array('Northwest','2005-01-01','2005-02-01',546.33),
					array('Southeast','2005-01-01','2005-02-01',93.26),
					array('Southwest','2005-01-01','2005-02-01',945.21),
					array('All Regions','--','--',1597.34) );
	$filename = './sales.csv';
	$fh = fopen($filename,'w') or die("Can't open $filename");
	foreach ($sales as $sales_line) {
		if (fputcsv($fh, $sales_line) === false) {
			die("Can't write CSV line");
		}
	}
	fclose($fh) or die("Can't close $filename");

#### 讨论

- 如果你想打印出格式化以后的 CSV 数据而不是将数据写到一个文件中，你可以使用 PHP 提供的一个特殊的输出流 `php://output` 来操作：

	【例1-28】

		$sales = array( array('Northeast','2005-01-01','2005-02-01',12.54),
						array('Northwest','2005-01-01','2005-02-01',546.33),
						array('Southeast','2005-01-01','2005-02-01',93.26),
						array('Southwest','2005-01-01','2005-02-01',945.21),
						array('All Regions','--','--',1597.34) );
		$fh = fopen('php://output','w');
		foreach ($sales as $sales_line) {
			if (fputcsv($fh, $sales_line) === false) {
				die("Can't write CSV line");
			}
		}
		fclose($fh);

- 如果你想把格式化了的 CSV 数据放到一个字符串当中而不是打印出来这些数据或者写到一个文件里面，你可以将【例1-28】中的方法与输出缓冲流结合一下：

	【例1-29】

		$sales = array( array('Northeast','2005-01-01','2005-02-01',12.54),
						array('Northwest','2005-01-01','2005-02-01',546.33),
						array('Southeast','2005-01-01','2005-02-01',93.26),
						array('Southwest','2005-01-01','2005-02-01',945.21),
						array('All Regions','--','--',1597.34) );
		ob_start();
		$fh = fopen('php://output','w') or die("Can't open php://output");
		foreach ($sales as $sales_line) {
			if (fputcsv($fh, $sales_line) === false) {
				die("Can't write CSV line");
			}
		}
		fclose($fh) or die("Can't close php://output");
		$output = ob_get_contents();
		ob_end_clean();

#### 请参阅

-  `fputcsv` 函数的[参考手册](http://php.net/manual/zh/function.fputcsv.php)

-  第 8.13 小节会有更加详细的介绍输出缓冲流的概念。


----------


[【返回目录】](/README.md)