title: 关于（伪）随机数生成
date: 2015-08-22 21:06:42
updated: 2015-08-22 21:06:42
cover: 
catalog: false
categories:
- 技术
tags:
- 密码学
- 随机数
---

本文描述现代密码学体系中最重要的组件之一，通常也被认为是最难描述的组件：随机数生成器

什么样的是随机数？
我见过的最通俗地对随机数的描述是这样的：
一串数字，如果没有比它更短的可以描述它的形式的程序，那么它就是随机的。
<!--more--> 

随机数从哪来？如何生成？
我们认为，一个完美的抛硬币是一个真随机的过程——假设硬币是中心对称，且在空中是平稳飞行。还有比如半导体噪声，元素的放射性衰变，这样都认为是真随机的过程。如果把物理过程的输出记录下来作为随机数，那这有了个真随机数生成器。

真随机数生成器发生的随机数可以认为是完全不可预测，这正是密码学需要的最佳选择。但是真随机数生成器一般效率都比较低，不能满足使用需求。所以就有了伪随机数生成器（Pseudo Random Number Generator, PRNG），或者叫做确定性随机位生成器（Deterministic Random Bit Generator, NIST, DRBG）。这类随机数发生器一般是一个确定性算法，即按照一种准确的规则集作为软件或硬件的算法，比如从一个初始种子值开始通过各种计算得到序列。一个广泛使用的例子就是ANSI C中的rand()函数，它的实现模型为：   
S0 = 12345   
S(n+1) = 1103515245S(n) + 12345 mod 2xp31, n=0,1...

对PRNG的一个一般要求就是必须拥有良好的统计属性，意味着它的输出近乎与真随机序列相同。有很多数学测试，如卡方校验，可以验证PRNG的统计行为。一般工程学上使用的PRNG满足此要求即可。

然后密码安全的伪随机数发生器（CSPRNG）则还要求不可预测性，即CSPRNG应该满足   
1. 给定密钥序列中n个连续位，不存在一个时间复杂度位多项式的算法使得成功预测下一位S(n+1)的概率超过50%   
2. 给出密钥S(i),S(i+1)...S(i+n-1)，计算任何之前的位S(i-1), S(i-2)...在计算上是不可行的。

NIST SP 800-90从提出了3个随机函数：   
//TODO

由于PRNG的算法一定是确定性的，为了让PRNG确定性状态的某些部分对外来的观察者是不可预测的，所以需要某些外来的源用熵来设置种子。

熵，entroy，源于希腊文，意为变化。熵是一个可以观察的事件中对不可确定性的一种度量，并且特别地以位为单位来表示，因为经常以二进制决策图来描述它。



[NIST: Random Number Generators](http://csrc.nist.gov/groups/ST/toolkit/rng/documentation_software.html)
[stackexchange: Interpretation of the results of NIST (p)NRG suite](http://crypto.stackexchange.com/questions/19861/interpretation-of-the-results-of-nist-pnrg-suite)
