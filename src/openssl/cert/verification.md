证书验证
========

当调用需要验证签名/认证的函数时，`cainfo`
参数是一个包含可信CA文件的文件夹和文件名的数组。如果文件夹指定了，它应该是能够被**openssl**命令正确使用的哈希目录。
