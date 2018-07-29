---
title: PHP 命名空间
date: 2015-08-03
updated: 2015-08-03
permalink: 181
categories:
    - PHP
    - 转载
---

## PHP中的命名空间是什么？

官方解释在此： [命名空间概述](http://php.net/manual/zh/language.namespaces.rationale.php)

命名空间用一句话说，就是：把 类、函数、变量 等放到子文件夹中去，以避免命名冲突。

## 被隐藏的第一个

在每个 PHP 文件的最开始定义命名空间：

```php
<?php

namespace TinyLara\TinyRoute;

class TinyRoute {
 ...
}
```

在定义命名空间之后引入命名空间：

```php
<?php

namespace TinyLara\TinyRoute;

use TinyLara\TinyView\TinyView;
class TinyRoute {
 ...
}
```

上述代码中，

```php
namespace TinyLara\TinyRoute

use TinyLara\TinyView\TinyView
```

这两行的真实地址是： `\TinyLara\TinyRoute`、`\TinyLara\TinyView\TinyView`，顶级命名空间标识（第一个 `\` ）被省略了。

## 被隐藏的别名

在上一节中中，这一行代码 `use TinyLara\TinyView\TinyView` 的完整写法应该是： `use \TinyLara\TinyView\TinyView as TinyView`

如果不指定别名，那就默认别名为类名，注入当前命名空间。

## 使用绝对路径直接调用

```php
<?php

namespace TinyLara\TinyRoute;

class TinyRoute
{
    public function foo()
    {
        return \TinyLara\TinyView\TinyView::fuck();
    }
}
```

使用绝对路径调用类时顶级命名空间标识（第一个 `\` ）**不能省略**。（很多人都在这个地方迷惑了）

## 命名空间的实际价值

命名空间的存在是为了解决下面两个问题：

- 用户编写的代码与PHP内部的类/函数/常量或第三方类/函数/常量之间的名字冲突。
- 为很长的标识符名称（通常是为了缓解第一类问题而定义的）创建一个别名（或简短）的名称，提高源代码的可读性。

目前非常流行的 Composer 就是一个基于命名空间的包管理器/依赖管理器，同样，Laravel 能达到今天的成功，很大程度上也是因为PHP5.3的普及，生恰逢时。你可以在 https://packagist.org/ 上下载到各种 composer 包，类似于 yum、npm或者gem。

## 同一命名空间下的类可以任意相互调用

```php
<?php

namespace A;

class ClassA
{
    public static function test()
    {
        echo 'Success!';
    }
}
```

```php
<?php

namespace A;

class ClassB
{
    public static function test()
    {
        ClassA::test();
    }
}
```

直接调用即可。

> 原文：https://lvwenhan.com/php/401.html