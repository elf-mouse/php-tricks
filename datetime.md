# PHP日期格式转时间戳

- `strtotime()`：将任何英文文本的日期时间描述解析为时间戳。
- `mktime()`：从日期取得时间戳。

## `int strtotime ( string time [, int now] )`

	<?php
	echo strtotime("2009-10-21 16:00:10"); // 输出 1256112010
	echo strtotime("10 September 2008");   // 输出 1220976000
	echo strtotime("+1 day");              // 输出明天此时的时间戳

## `int mktime(时, 分, 秒, 月, 日, 年)`

	<?php
	echo mktime(21, 50, 55, 07, 14, 2010); // 输出 1279115455

	echo date("Y-m-d", mktime(0, 0, 0, 12, 32, 2007)); // 2008-01-01
	echo date("Y-m-d", mktime(0, 0, 0, 13, 1, 2007));  // 2008-01-01

	$lastday = mktime(0, 0, 0, 3, 0, 2008);
	echo strftime("2008年最后一天是：%d", $lastday); // 2008年最后一天是：29

## 自定义函数

	<?php
	function str_to_time($timestamp = '') {
		if (preg_match("/[0-9]{4}-[0-9]{1,2}-[0-9]{1,2} (0[0-9]|1[0-9]|2[0-3]):([0-5][0-9]):([0-5][0-9])/i", $timestamp)) {
			list($date,$time)=explode(" ",$timestamp);
			list($year,$month,$day)=explode("-",$date);
			list($hour,$minute,$seconds )=explode(":",$time);
			$timestamp=gmmktime($hour,$minute,$seconds,$month,$day,$year);
		}
		else {
			$timestamp=time();
		}
		return $timestamp;
	}

	$date_str = "2011-09-11 17:00:00";
	$time_str = str_to_time($date_str);
	echo $time_str, '<br>', date("Y-m-d H:i:s", $time_str);
