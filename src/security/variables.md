用户提交的数据
==============

很多 PHP 程序所存在的重大弱点并不是 PHP
语言本身的问题，而是编程者的安全意识不高而导致的。因此，必须时时注意每一段代码可能存在的问题，去发现非正确数据提交时可能造成的影响。

**示例 \#1 危险的变量用法**

``` php
<?php
// 从用户目录中删除一个文件，或者……能删除更多的东西？
unlink ($evil_var);

// 记录用户的登陆，或者……能否在 /etc/passwd 添加数据？
fwrite ($fp, $evil_var);

// 执行一些普通的命令，或者……可以执行 rm -rf * ？
system ($evil_var);
exec ($evil_var);

?>
```

必须时常留意你的代码，以确保每一个从客户端提交的变量都经过适当的检查，然后问自己以下一些问题：

-   <span class="simpara"> 此脚本是否只能影响所预期的文件？ </span>
-   <span class="simpara"> 非正常的数据被提交后能否产生作用？ </span>
-   <span class="simpara"> 此脚本能用于计划外的用途吗？ </span>
-   <span class="simpara"> 此脚本能否和其它脚本结合起来做坏事？ </span>
-   <span class="simpara"> 是否所有的事务都被充分记录了？ </span>

在写代码的时候问自己这些问题，否则以后可能要为了增加安全性而重写代码了。注意了这些问题的话，也许还不完全能保证系统的安全，但是至少可以提高安全性。

还可以考虑关闭 register\_globals，magic\_quotes
或者其它使编程更方便但会使某个变量的合法性，来源和其值被搞乱的设置。在开发时，可以使用
error\_reporting(E\_ALL)
模式帮助检查变量使用前是否有被检查或被初始化，这样就可以防止某些非正常的数据的挠乱了。
