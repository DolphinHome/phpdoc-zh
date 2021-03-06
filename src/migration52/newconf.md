New INI Configuration Directives
--------------------------------

New `php.ini` directives introduced in PHP 5.2.0:

-   <span class="simpara">
    <a href="/filesystem/setup.html#" class="link">allow_url_include</a>
    </span> <span class="simpara"> This useful option makes it possible
    to differentiate between standard file operations on remote files,
    and the inclusion of remote files. While the former is usually
    desirable, the latter can be a security risk if used naively.
    Starting with PHP 5.2.0, you can allow remote file operations while
    disallowing the inclusion of remote files in local scripts. In fact,
    this is the default configuration. </span>
-   <span class="simpara">
    <a href="/pcre/setup.html#" class="link">pcre.backtrack_limit</a>
    </span> <span class="simpara"> PCRE's backtracking limit. </span>
-   <span class="simpara">
    <a href="/pcre/setup.html#" class="link">pcre.recursion_limit</a>
    </span> <span class="simpara"> PCRE's recursion limit. Please note
    that if you set this value to a high number you may consume all the
    available process stack and eventually crash PHP (due to reaching
    the stack size limit imposed by the Operating System). </span>
-   <span class="simpara">
    <a href="/session/setup.html#" class="link">session.cookie_httponly</a>
    </span> <span class="simpara"> Marks the cookie as accessible only
    through the HTTP protocol. This means that the cookie won't be
    accessible by scripting languages, such as JavaScript. This setting
    can effectively help to reduce identity theft through XSS attacks
    (although it is not supported by all browsers). </span>

New directives in PHP 5.2.2:

-   <span class="simpara">
    <a href="/info/setup.html#" class="link">max_input_nesting_level</a>
    </span> <span class="simpara"> Limits how deep
    <a href="/language/variables/external.html" class="link">input variables</a>
    can be nested, default is 64. </span>
