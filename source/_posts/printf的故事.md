---
title: printf的故事
date: 2017-09-28 22:01:31
tags: printf
---

# 奇怪的输出结果
晚上，小佳佳突然问了我一个很有意思的问题，原来是printf输出了一个令人出乎意料的结果。

<!--more-->

问题描述：当一个2byte的整型负数，以16进制格式输出时，却得到了一个4byte的输出结果。

于是基于此问题，我顺便做了以下几组数据的输出，用来理解这一奇怪的现象。

```
	short a = 1, b = -5;
	short c = 0xf234ffff;
	short d = 0x1234ffff;
	int e = 0x1234ffff;

	printf("sizeof(short) = %d\n", sizeof(short));
	printf("a=%04x, b=%04x, c=%04x, d=%04x, e=%04x\n", a, b, c,d,e);
	printf("a=%04x, b=%04x, c=%04x, d=%04x,e=%04x\n", a, (unsigned int)b, (unsigned int)c,(unsigned int)d,(unsigned int)e);
	printf("a=%04x, b=%04x, c=%04x, d=%04x,e=%04x\n", a, (unsigned short)b,(unsigned short)c,(unsigned short)d,(unsigned short)e);
```

```
	输出结果
	
	sizeof(short) = 2
	a=0001, b=fffffffb, c=ffffffff, d=ffffffff, e=1234ffff
	a=0001, b=fffffffb, c=ffffffff, d=ffffffff,e=1234ffff
	a=0001, b=fffb, c=ffff, d=ffff,e=ffff
```

其中，2byte的变量b在第2，3次的printf打印中，输出结果均为4byte。刚看到这个输出结果的时候，大吃一惊，着实从系统编译器的位数，到人生的意义，甚至到宇宙的起源，彻彻底底的怀疑了一遍。

# 解密

其实对比一下这几组输出的结果，已经明显能够看出些端倪。
首先，认识一下printf中%x格式控制符的具体含义。

![image](https://user-images.githubusercontent.com/31929148/30979474-4f70b024-a4b0-11e7-8d28-d59681806f49.png)

原版对%x的介绍为：unsigned int as a hexadecimal number. x uses lower-case letters and X uses upper-case.即在对变量进行打印时，是将变量当做unsigned int进行处理的。

其次，在不同类型的数据进行操作时，存在以下转换规则

![image](https://user-images.githubusercontent.com/31929148/30979829-7ce1ae68-a4b1-11e7-9e3d-e6cea54c83b9.png)

如果short类型的变量为负数，在printf输出时，相当于将short类型强制转换成unsigned int类型。转换顺序为short -> int -> unsigned int.

而当该负数在传递给printf之前，先进行了unsigned short的转换时，转换顺序为short -> unsigned short -> unsigned int.

其中相同符号类型的变量在转换时，符号保持不变。

最后，在变量值超出了变量类型所能存储的最大值时，会出现数据溢出。当数据溢出时，编译器会根据变量的类型，来对变量值进行截取。如示例代码中的变量c 和 d.

# 扩展
## printf的参数传递

printf的参数个数是可变的，所以在处理参数的传递时，采用了栈的方式来逐个存储参数列表中的各个参数，然后再按照格式控制符逐个输出各个参数。其中 【参数入栈的顺序为从右向左】，以保证输出时，首先出栈的是参数列表中最左边的参数。



