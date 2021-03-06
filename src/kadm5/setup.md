安装／配置
==========

**目录**

-   [需求](/kadm5/setup.html#需求)
-   [安装](/kadm5/setup.html#安装)
-   [运行时配置](/kadm5/setup.html#运行时配置)
-   [资源类型](/kadm5/setup.html#资源类型)

需求
----

构建此扩展不需要其他扩展。

安装
----

安装此 PECL 扩展相关的信息可在手册中标题为
<a href="/install/pecl.html" class="link">PECL 扩展的安装</a>章节中找到。更多信息如新的发行版本、下载、源文件、
维护人员信息及变更日志等，都在此处：
<a href="https://pecl.php.net/package/kadm5" class="link external">» https://pecl.php.net/package/kadm5</a>.

> **Note**:
>
> If this option is used without specifying the path to KADM5, PHP will
> use the built-in KADM5 client libraries. Users who run other
> applications that use KADM5 (for example, running PHP 4 and PHP 5 as
> concurrent apache modules, or auth-kadm5) should always specify the
> path to KADM5: *--with-kadm5=/path/to/kadm5*. This will force PHP to
> use the client libraries installed by KADM5, avoiding any conflicts.

PECL 扩展的 DLL 当前不可用。参见
<a href="/install/windows/legacy/index.html#install.windows.building" class="link">在 Windows 上构建</a>章节。

运行时配置
----------

此扩展没有在 `php.ini` 中定义配置指令。

资源类型
--------

This extension defines a KADM5 handle returned by <span
class="function">kadm5\_init\_with\_password</span>.
