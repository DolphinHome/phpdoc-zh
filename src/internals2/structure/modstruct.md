The zend\_module structure
--------------------------

The main source file of a PHP extension contains several new constructs
for a C programmer. The most important of these, the one touched first
when starting a new extension, is the *zend\_module* structure. This
structure contains a wealth of information that tells the Zend Engine
about the extension's dependencies, version, callbacks, and other
critical data. The structure has mutated considerably over time; this
section will focus on the structure as it has appeared since PHP 5.2,
and will identify the very few parts which have changed in PHP 5.3.

The *zend\_module* declaration from `counter.c` looks like this before
any code has been written. The example file was generated by **ext\_skel
--extname=counter**, with some obsolete constructs removed:

**示例 \#1 zend\_module declaration in the counter extension**

``` c
/* {{{ counter_module_entry
 */
zend_module_entry counter_module_entry = {
    STANDARD_MODULE_HEADER,
    "counter",
    counter_functions,
    PHP_MINIT(counter),
    PHP_MSHUTDOWN(counter),
    PHP_RINIT(counter),        /* Replace with NULL if there's nothing to do at request start */
    PHP_RSHUTDOWN(counter),    /* Replace with NULL if there's nothing to do at request end */
    PHP_MINFO(counter),
    "0.1", /* Replace with version number for your extension */
    STANDARD_MODULE_PROPERTIES
};
/* }}} */
```

This may look a bit daunting at first glance, but most of it is very
simple to understand. Here's the declaration of *zend\_module* from
`zend_modules.h` in PHP 5.3:

**示例 \#2 zend\_module definition in PHP 5.3**

``` c
struct _zend_module_entry {
    unsigned short size;
    unsigned int zend_api;
    unsigned char zend_debug;
    unsigned char zts;
    const struct _zend_ini_entry *ini_entry;
    const struct _zend_module_dep *deps;
    const char *name;
    const struct _zend_function_entry *functions;
    int (*module_startup_func)(INIT_FUNC_ARGS);
    int (*module_shutdown_func)(SHUTDOWN_FUNC_ARGS);
    int (*request_startup_func)(INIT_FUNC_ARGS);
    int (*request_shutdown_func)(SHUTDOWN_FUNC_ARGS);
    void (*info_func)(ZEND_MODULE_INFO_FUNC_ARGS);
    const char *version;
    size_t globals_size;
#ifdef ZTS
    ts_rsrc_id* globals_id_ptr;
#else
    void* globals_ptr;
#endif
    void (*globals_ctor)(void *global TSRMLS_DC);
    void (*globals_dtor)(void *global TSRMLS_DC);
    int (*post_deactivate_func)(void);
    int module_started;
    unsigned char type;
    void *handle;
    int module_number;
};
```

Many of these fields will never be touched by an extension writer. There
are a number of standard macros that set them to their proper values
automatically. The macro **`STANDARD_MODULE_HEADER`** fills in
everything up to the `deps` field. Alternatively, the
**`STANDARD_MODULE_HEADER_EX`** will leave the `deps` field empty for
the developer's use. The developer is always responsible for everything
from `name` to `version`. After that, the
**`STANDARD_MODULE_PROPERTIES`** macro will fill in the rest of the
structure, or the **`STANDARD_MODULE_PROPERTIES_EX`** macro can be used
to leave the extension globals and post-deactivation function fields
unfilled. Most modern extensions will make use of module globals.

> **Note**:
>
> This table gives the values that each field would have if the
> developer were to fill in the structure entirely by hand, without
> recourse to any of the shortcut macros. *This is not recommended.* The
> "correct" values for many fields may change. Use the macros whenever
> possible.

**Module structure field values**

Field

Value

Description

`size`
<a href="#fnidinternals2.structure.modstruct.struct-values.not-for-dev" id="fninternals2.structure.modstruct.struct-values.not-for-dev"><sup>[1]</sup></a>
<a href="#fnidinternals2.structure.modstruct.struct-values.given-by-smhe" id="fninternals2.structure.modstruct.struct-values.given-by-smhe"><sup>[2]</sup></a>
<a href="#fnidinternals2.structure.modstruct.struct-values.given-by-smh" id="fninternals2.structure.modstruct.struct-values.given-by-smh"><sup>[3]</sup></a>

`sizeof(zend_module_entry)`

The size in bytes of the structure.

`zend_api`
[<sup>\[1\]</sup>](#fnidinternals2.structure.modstruct.struct-values.not-for-dev)
[<sup>\[2\]</sup>](#fnidinternals2.structure.modstruct.struct-values.given-by-smhe)
[<sup>\[3\]</sup>](#fnidinternals2.structure.modstruct.struct-values.given-by-smh)

**`ZEND_MODULE_API_NO`**

The version of the Zend API this module was compiled against.

`zend_debug`
[<sup>\[1\]</sup>](#fnidinternals2.structure.modstruct.struct-values.not-for-dev)
[<sup>\[2\]</sup>](#fnidinternals2.structure.modstruct.struct-values.given-by-smhe)
[<sup>\[3\]</sup>](#fnidinternals2.structure.modstruct.struct-values.given-by-smh)

**`ZEND_DEBUG`**

A flag indicating whether the module was compiled with debugging turned
on.

`zts`
[<sup>\[1\]</sup>](#fnidinternals2.structure.modstruct.struct-values.not-for-dev)
[<sup>\[2\]</sup>](#fnidinternals2.structure.modstruct.struct-values.given-by-smhe)
[<sup>\[3\]</sup>](#fnidinternals2.structure.modstruct.struct-values.given-by-smh)

**`USING_ZTS`**

A flag indicating whether the module was compiled with ZTS (TSRM)
enabled (see
<a href="/internals2/memory.html" class="xref">内存管理</a>).

`ini_entry`
[<sup>\[1\]</sup>](#fnidinternals2.structure.modstruct.struct-values.not-for-dev)
[<sup>\[3\]</sup>](#fnidinternals2.structure.modstruct.struct-values.given-by-smh)

**`NULL`**

This pointer is used internally by Zend to keep a non-local reference to
any INI entries declared for the module.

`deps`
[<sup>\[3\]</sup>](#fnidinternals2.structure.modstruct.struct-values.given-by-smh)

**`NULL`**

A pointer to a list of dependencies for the module.

`name`

"mymodule"

The name of the module. This is the short name, such as "spl" or
"standard".

`functions`

mymodule\_functions

A pointer to the module's function table, which Zend uses to expose
functions in the module to user space.

`module_startup_func`

PHP\_MINIT(mymodule)

A callback function that Zend will call the first time a module is
loaded into a particular instance of PHP.

`module_shutdown_func`

PHP\_MSHUTDOWN(mymodule)

A callback function that Zend will call the when a module is unloaded
from a particular instance of PHP, typically during final shutdown.

`request_startup_func`

PHP\_RINIT(mymodule)

A callback function that Zend will call at the beginning of each
request. This should be as short as possible or *NULL* as calling this
has costs on every request.

`request_shutdown_func`

PHP\_RSHUTDOWN(mymodule)

A callback function that Zend will call at the end of each request. This
should be as short as possible or *NULL* as calling this has costs on
every request.

`info_func`

PHP\_MINFO(mymodule)

A callback function that Zend will call when the <span
class="function">phpinfo</span> function is called.

`version`

**`NO_VERSION_YET`**

A string giving the version of the module, as specified by the module
developer. It is recommended that the version number be either in the
format expected by version\_compare() (e.g. "1.0.5-dev"), or a CVS or
SVN revision number (e.g. "$Rev: 322138 $").

`globals_size`
[<sup>\[1\]</sup>](#fnidinternals2.structure.modstruct.struct-values.not-for-dev)
<a href="#fnidinternals2.structure.modstruct.struct-values.given-by-smp" id="fninternals2.structure.modstruct.struct-values.given-by-smp"><sup>[4]</sup></a>
<a href="#fnidinternals2.structure.modstruct.struct-values.given-by-nmg" id="fninternals2.structure.modstruct.struct-values.given-by-nmg"><sup>[5]</sup></a>
<a href="#fnidinternals2.structure.modstruct.struct-values.given-by-pmg" id="fninternals2.structure.modstruct.struct-values.given-by-pmg"><sup>[6]</sup></a>

sizeof(zend\_mymodule\_globals)

The size of the data structure containing the module's globals, if any.

`globals_id_ptr`
[<sup>\[1\]</sup>](#fnidinternals2.structure.modstruct.struct-values.not-for-dev)
[<sup>\[4\]</sup>](#fnidinternals2.structure.modstruct.struct-values.given-by-smp)
[<sup>\[5\]</sup>](#fnidinternals2.structure.modstruct.struct-values.given-by-nmg)
[<sup>\[6\]</sup>](#fnidinternals2.structure.modstruct.struct-values.given-by-pmg)
<a href="#fnidinternals2.structure.modstruct.struct-values.only-with-zts" id="fninternals2.structure.modstruct.struct-values.only-with-zts"><sup>[7]</sup></a>

&mymodule\_globals\_id

Only one of these two fields will exist, depending upon whether the
**`USING_ZTS`** constant is **`TRUE`**. The former is an index into
TSRM's allocation table for the module's globals, and the latter is a
pointer directly to the globals.

`globals_ptr`
[<sup>\[1\]</sup>](#fnidinternals2.structure.modstruct.struct-values.not-for-dev)
[<sup>\[4\]</sup>](#fnidinternals2.structure.modstruct.struct-values.given-by-smp)
[<sup>\[5\]</sup>](#fnidinternals2.structure.modstruct.struct-values.given-by-nmg)
[<sup>\[6\]</sup>](#fnidinternals2.structure.modstruct.struct-values.given-by-pmg)
<a href="#fnidinternals2.structure.modstruct.struct-values.only-without-zts" id="fninternals2.structure.modstruct.struct-values.only-without-zts"><sup>[8]</sup></a>

&mymodule\_globals

`globals_ctor`
[<sup>\[4\]</sup>](#fnidinternals2.structure.modstruct.struct-values.given-by-smp)
[<sup>\[5\]</sup>](#fnidinternals2.structure.modstruct.struct-values.given-by-nmg)
[<sup>\[6\]</sup>](#fnidinternals2.structure.modstruct.struct-values.given-by-pmg)

PHP\_GINIT(mymodule)

This funtion is called to initialize a module's globals *before* any
`module_startup_func`.

`globals_dtor`
[<sup>\[4\]</sup>](#fnidinternals2.structure.modstruct.struct-values.given-by-smp)
[<sup>\[5\]</sup>](#fnidinternals2.structure.modstruct.struct-values.given-by-nmg)
[<sup>\[6\]</sup>](#fnidinternals2.structure.modstruct.struct-values.given-by-pmg)

PHP\_GSHUTDOWN(mymodule)

This funtion is called to deallocate a module's globals *after* any
`module_shutdown_func`.

`post_deactivate_func`
[<sup>\[4\]</sup>](#fnidinternals2.structure.modstruct.struct-values.given-by-smp)

ZEND\_MODULE\_POST\_ZEND\_DEACTIVATE\_N(mymodule)

This function is called by Zend after request shutdown. It is rarely
used.

`module_started`
[<sup>\[1\]</sup>](#fnidinternals2.structure.modstruct.struct-values.not-for-dev)
<a href="#fnidinternals2.structure.modstruct.struct-values.given-by-smpe" id="fninternals2.structure.modstruct.struct-values.given-by-smpe"><sup>[9]</sup></a>
[<sup>\[4\]</sup>](#fnidinternals2.structure.modstruct.struct-values.given-by-smp)

0

These fields are used for Zend's internal tracking information.

`type`
[<sup>\[1\]</sup>](#fnidinternals2.structure.modstruct.struct-values.not-for-dev)
[<sup>\[9\]</sup>](#fnidinternals2.structure.modstruct.struct-values.given-by-smpe)
[<sup>\[4\]</sup>](#fnidinternals2.structure.modstruct.struct-values.given-by-smp)

0

`handle`
[<sup>\[1\]</sup>](#fnidinternals2.structure.modstruct.struct-values.not-for-dev)
[<sup>\[9\]</sup>](#fnidinternals2.structure.modstruct.struct-values.given-by-smpe)
[<sup>\[4\]</sup>](#fnidinternals2.structure.modstruct.struct-values.given-by-smp)

**`NULL`**

`module_number`
[<sup>\[1\]</sup>](#fnidinternals2.structure.modstruct.struct-values.not-for-dev)
[<sup>\[9\]</sup>](#fnidinternals2.structure.modstruct.struct-values.given-by-smpe)
[<sup>\[4\]</sup>](#fnidinternals2.structure.modstruct.struct-values.given-by-smp)

0

<a href="#fninternals2.structure.modstruct.struct-values.not-for-dev" id="fnidinternals2.structure.modstruct.struct-values.not-for-dev"><sup>[1]</sup></a><span
class="para footnote"> This field is not intended for use by module
developers. </span>

<a href="#fninternals2.structure.modstruct.struct-values.given-by-smhe" id="fnidinternals2.structure.modstruct.struct-values.given-by-smhe"><sup>[2]</sup></a><span
class="para footnote"> This field is filled in by
**`STANDARD_MODULE_HEADER_EX`**. </span>

<a href="#fninternals2.structure.modstruct.struct-values.given-by-smh" id="fnidinternals2.structure.modstruct.struct-values.given-by-smh"><sup>[3]</sup></a><span
class="para footnote"> This field is filled in by
**`STANDARD_MODULE_HEADER`**. </span>

<a href="#fninternals2.structure.modstruct.struct-values.given-by-smp" id="fnidinternals2.structure.modstruct.struct-values.given-by-smp"><sup>[4]</sup></a><span
class="para footnote"> This field is filled in by
**`STANDARD_MODULE_PROPERTIES`**. </span>

<a href="#fninternals2.structure.modstruct.struct-values.given-by-nmg" id="fnidinternals2.structure.modstruct.struct-values.given-by-nmg"><sup>[5]</sup></a><span
class="para footnote"> This field is filled in by
**`NO_MODULE_GLOBALS`**. </span>

<a href="#fninternals2.structure.modstruct.struct-values.given-by-pmg" id="fnidinternals2.structure.modstruct.struct-values.given-by-pmg"><sup>[6]</sup></a><span
class="para footnote"> This field is filled in by
**`PHP_MODULE_GLOBALS`**. </span>

<a href="#fninternals2.structure.modstruct.struct-values.only-with-zts" id="fnidinternals2.structure.modstruct.struct-values.only-with-zts"><sup>[7]</sup></a><span
class="para footnote"> This field only exists when **`USING_ZTS`** is
**`TRUE`**. </span>

<a href="#fninternals2.structure.modstruct.struct-values.only-without-zts" id="fnidinternals2.structure.modstruct.struct-values.only-without-zts"><sup>[8]</sup></a><span
class="para footnote"> This field only exists when **`USING_ZTS`** is
**`FALSE`**. </span>

<a href="#fninternals2.structure.modstruct.struct-values.given-by-smpe" id="fnidinternals2.structure.modstruct.struct-values.given-by-smpe"><sup>[9]</sup></a><span
class="para footnote"> This field is filled in by
**`STANDARD_MODULE_PROPERTIES_EX`**. </span>

### Filling in the structure in a practical situation

With all these fields to play with, it can be confusing to know which to
use for what purpose. Here is the *zend\_module* definition from the
"counter" example extension after updating it to its final form.

**示例 \#3 Counter extension module definition**

``` c
/* {{{ counter_module_entry
 */
zend_module_entry counter_module_entry = {
    STANDARD_MODULE_HEADER,
    "counter",
    counter_functions,
    PHP_MINIT(counter),
    PHP_MSHUTDOWN(counter),
    PHP_RINIT(counter),
    PHP_RSHUTDOWN(counter),
    PHP_MINFO(counter),
    NO_VERSION_YET,
    PHP_MODULE_GLOBALS(counter),
    PHP_GINIT(counter),
    PHP_GSHUTDOWN(counter),
    NULL,
    STANDARD_MODULE_PROPERTIES_EX
};
/* }}} */
```

-   <span class="simpara"> **`STANDARD_MODULE_HEADER`** is used since
    this module doesn't define any dependencies. </span>
-   <span class="simpara"> "counter" is the extension's name, and is
    used to define the various callback functions the module passes to
    Zend. "counter" uses module, globals, and request functions at
    startup and shutdown times, and provides information to <span
    class="function">phpinfo</span>, so all seven callbacks are defined.
    </span>
-   <span class="simpara"> It is assumed that there is a variable of
    type <span class="type">zend\_function\_entry \*</span> named
    `counter_functions` earlier in the file that contains the module
    definition, listing the functions the module exports to userspace.
    </span>
-   <span class="simpara"> **`NO_VERSION_YET`** is a particularly nice
    way of telling Zend the module doesn't have a version. It might have
    been more correct to place *"1.0"* here instead in a real module.
    </span>
-   <span class="simpara"> "counter" uses per-module globals, so
    **`PHP_MODULE_GLOBALS`** is used </span>
-   <span class="simpara"> This module has no post-deactivate function,
    so **`NULL`** is used. </span>
-   <span class="simpara"> Since this module *does* use globals,
    **`STANDARD_MODULE_PROPERTIES_EX`** is used to finish the structure.
    </span>

### What's changed between 5.2 and 5.3?

Nothing. The only differences in the *zend\_module* structure between
PHP 5.2 and PHP 5.3 are a few <span class="modifier">const</span>
keywords.
