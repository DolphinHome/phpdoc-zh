不向后兼容的变更
----------------

尽管大部分现有的 PHP 5
代码不需要任何改变就可以正常运行，但请注意一些不向后兼容的变更：

-   <span class="simpara"> 不再支持
    <a href="/features/safe-mode.html" class="link">安全模式</a>
    。任何依赖安全模式的应用在安全方面都需要进行调整。 </span>
-   <span class="simpara"> 移除
    <a href="/security/magicquotes.html" class="link">魔术引号</a>
    。为避免出现安全问题，依赖此特性的应用可能需要升级。 </span> <span
    class="simpara"> <span
    class="function">get\_magic\_quotes\_gpc</span> 和 <span
    class="function">get\_magic\_quotes\_runtime</span> 现在总是返回
    **`FALSE`** 。 调用 <span
    class="function">set\_magic\_quotes\_runtime</span> 将产生一个
    **`E_CORE_ERROR`** 级别的错误。 </span>
-   <span class="simpara">
    <a href="/ini/core.html#ini.register-globals" class="link">register_globals</a>
    和
    <a href="/ini/core.html#ini.register-long-arrays" class="link">register_long_arrays</a>
    `php.ini` 指令被移除。 </span>
-   <span class="simpara">
    <a href="/language/references/pass.html" class="link">调用时的引用传递</a>
    被移除。 </span>
-   <span class="simpara">
    <a href="/control-structures/break.html" class="link">break</a> 和
    <a href="/control-structures/continue.html" class="link">continue</a>
    语句不再接受可变参数（ 比如： *break 1 + foo() \* $bar;* ）。像类似
    *break 2;* 这样的固定参数仍可使用。受此变化影响，不再允许出现 *break
    0;* 和 *continue 0;* 。 </span>
-   <span class="simpara"> 在
    <a href="/book/datetime.html" class="link">日期与时间扩展</a>
    中，不再支持时区使用 TZ（TimeZone）环境变量设置。必须使用
    <a href="/datetime/setup.html#" class="link">date.timezone</a>
    `php.ini` 配置选项或 <span
    class="function">date\_default\_timezone\_set</span>
    函数来指定时区。PHP 将不再尝试猜测时区，而是回退到“UTC”并发出一条
    **`E_WARNING`** 错误。 </span>
-   <span class="simpara"> 非数字的字符串偏移量，比如 *$a\['foo'\]* 此处
    $a 是一个字符串，现在使用 <span class="function">isset</span> 时返回
    false，使用 <span class="function">empty</span> 时返回
    true，并产生一条 **`E_WARNING`** 错误。偏移量类型是布尔和 null
    则产生一条 **`E_NOTICE`** 错误。 数字字符串（比如 *$a\['2'\]*
    ）仍像以前一样运行。注意像类似 *'12.3'* 和 *'5 foobar'*
    这样的偏移量将被视为非数字并产生一条 **`E_WARNING`**
    错误，但因为向后兼容的原因它们会被分别转换成 12 和 5 。 </span>
    <span class="simpara"> 注意：下列代码返回不同的结果。 </span> <span
    class="simpara"> $str='abc';var\_dump(isset($str\['x'\])); // 在 PHP
    5.4 或更新版本返回 false，但在 PHP 5.3 或更低版本返回 true </span>
-   <span class="simpara"> 数组转换成字符串将产生一条 **`E_NOTICE`**
    级别的错误，但返回的结果仍是字符串 *"Array"* 。 </span>
-   <span class="simpara"> **`NULL`** 、**`FALSE`** 、或
    一个空字符串被添加成一个对象的属性时将发出一条 **`E_WARNING`**
    级别的错误，而不是 **`E_STRICT`** 。 </span>
-   <span class="simpara">
    现在参数名使用全局变量将会导致一个致命错误。禁止类似 *function
    foo($\_GET, $\_POST) {}* 这样的代码。 </span>
-   <span class="simpara"> Salsa10 和 Salsa20
    <a href="/book/hash.html" class="link">哈希算法</a> 被移除。 </span>
-   <span class="simpara"> 当使用两个空数组作为参数时， <span
    class="function">array\_combine</span> 现在返回 *array()* 而不是
    **`FALSE`** 。 </span>
-   <span class="simpara"> <span class="function">htmlentities</span>
    将像 <span class="function">htmlspecialchars</span>
    一样处理亚洲字符集，这是以前 PHP 版本的处理情况，但现在将会发出一条
    **`E_STRICT`** 错误。 </span>
-   <span class="simpara"> 强烈建议不要再使用 <span
    class="function">eregi</span> ，此特性在最新版本中被移除。 </span>

下列关键字现在被 <a href="/reserved.html" class="link">保留</a>
，且不能用于函数名或类名。

-   <span class="simpara">
    <a href="/language/oop5/traits.html" class="link">trait</a> </span>
-   <span class="simpara">
    <a href="/language/types/callable.html" class="link">callable</a>
    </span>
-   <span class="simpara">
    <a href="/language/oop5/traits.html" class="link">insteadof</a>
    </span>

下列函数已从 PHP 中移除：

-   <span class="simpara"> <span
    class="function">define\_syslog\_variables</span> </span>
-   <span class="simpara"> <span
    class="function">import\_request\_variables</span> </span>
-   <span class="simpara"> <span
    class="function">session\_is\_registered</span> 、 <span
    class="function">session\_register</span> 以及 <span
    class="function">session\_unregister</span> 。 </span>
-   <span class="simpara"> 别名 <span
    class="function">mysqli\_bind\_param</span> 、 <span
    class="function">mysqli\_bind\_result</span> 、 <span
    class="function">mysqli\_client\_encoding</span> 、 <span
    class="function">mysqli\_fetch</span> 、 <span
    class="function">mysqli\_param\_count</span> 、 <span
    class="function">mysqli\_get\_metadata</span> 、 <span
    class="function">mysqli\_send\_long\_data</span> 、
    mysqli::client\_encoding() 以及 mysqli\_stmt::stmt() 。 </span>
