Exception::getTrace
===================

获取异常追踪信息

### 说明

<span class="modifier">final</span> <span class="modifier">public</span>
<span class="type">array</span> <span
class="methodname">Exception::getTrace</span> ( <span
class="methodparam">void</span> )

返回异常追踪信息。

### 参数

此函数没有参数。

### 返回值

返回包含异常追踪信息的数组（<span class="type">array</span>）。

### 范例

**示例 \#1 <span class="function">Exception::getTrace</span>示例**

``` php
<?php
function test() {
 throw new Exception;
}

try {
 test();
} catch(Exception $e) {
 var_dump($e->getTrace());
}
?>
```

以上例程的输出类似于：

    array(1) {
      [0]=>
      array(4) {
        ["file"]=>
        string(22) "/home/bjori/tmp/ex.php"
        ["line"]=>
        int(7)
        ["function"]=>
        string(4) "test"
        ["args"]=>
        array(0) {
        }
      }
    }

### 参见

-   <span class="methodname">Throwable::getTrace</span>
