预定义常量
==========

下列常量由此扩展定义，且仅在此扩展编译入 PHP 或在运行时动态载入时可用。

**`FTP_ASCII`** (<span class="type">integer</span>)  

**`FTP_TEXT`** (<span class="type">integer</span>)  

**`FTP_BINARY`** (<span class="type">integer</span>)  

**`FTP_IMAGE`** (<span class="type">integer</span>)  

**`FTP_TIMEOUT_SEC`** (<span class="type">integer</span>)  
更多设置参考函数 <span class="function">ftp\_set\_option</span>。

下列变量在 PHP 4.3.0 以后版本中被加入。

**`FTP_AUTOSEEK`** (<span class="type">integer</span>)  
参考函数 <span class="function">ftp\_set\_option</span>。

**`FTP_AUTORESUME`** (<span class="type">integer</span>)  
为 GET 和 PUT 请求自动决定恢复和开始的位置 (只能工作在 FTP\_AUTOSEEK
打开的情况下)

**`FTP_FAILED`** (<span class="type">integer</span>)  
异步传输失败

**`FTP_FINISHED`** (<span class="type">integer</span>)  
异步传输成功

**`FTP_MOREDATA`** (<span class="type">integer</span>)  
异步传输是活动状态的
