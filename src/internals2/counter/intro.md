简介
----

“counter”扩展提供在 PHP
代码中加入许多计数器，并可由调用者决定什么时候进行重置。

“counter”有 3 种接口: 简单接口, 扩展接口, 对象化接口。 简单接口提供用
INI 设置文件控制的单个计数器的功能调用。
扩展接口提供被命名的任意多个计数器，并可选择进行持久化以用于多个的 PHP
请求生命周期内。 对象化接口则将简单接口和扩展接口融合成为<span
class="classname">Counter</span>类。
