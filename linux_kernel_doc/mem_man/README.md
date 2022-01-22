# 内存管理概览

Linux内存管理子系统，顾名思义，就是用来管理系统内存的子系统。本子系统包括虚拟内存的实现、按需分页、用户程序和内核内部的内存分配、文件在进程地址空间的映射以及一些厉害的东西。

Linux内存管理子系统结构复杂，可调整的选项设置多。绝大多数设置都可以通过`/proc`文件系统和`sysctl`命令来设置。相关API文档参见[`/proc/sys/vm`的文档](https://www.kernel.org/doc/Documentation/sysctl/vm.txt)和[`proc`的man手册]()。

Linux内存管理使用了自己的术语，如果你不熟悉这些术语，可以参考[概念一览](https://www.kernel.org/doc/html/latest/admin-guide/mm/concepts.html)。

以下是Linux内存管理细节的文档：

