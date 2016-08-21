# PHP原理

Php的四层体系包括：zend引擎，Extensions，sapi，上层应用。

zend引擎：整体用纯C开发，主要将PHP代码翻译成为opcode,实现了基本的数据结构（如hashtable、oo）、内存分配及管理、提供了相应的api方法供外部调用

Extensions：常见的各种内置函数（如array系列）、标准库等都是通过extension来实现,用户也可以根据需要实现自己的extension以达到功能扩展、性能优化等目的

sapi：Server Application Programming Interface，也就是服务端应用编程接口，sapi通过一系列钩子函数，使得php可以和外围交互数据

上层应用：我们编写的php程序