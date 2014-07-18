---
title:php操作XML
layout: page
category: php
date: 2014-06-04
---
#php操作XML
```php
	$postStr = file_get_contents("./a.xml");
	$match = "/\<DataContent\>[\s\S]*?\<\/DataContent\>/";
	$num = preg_match_all( $match, $postStr, $out,PREG_PATTERN_ORDER );
	echo '<pre>';
	var_dump($out);
	echo '</pre>';exit;
```