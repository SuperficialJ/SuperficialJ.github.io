<!--

 * @Author: peerless

 * @Date: 2020-06-28 18:25:20

 * @LastEditTime: 2020-06-30 21:27:52

 * @LastEditors: Please set LastEditors

 * @Description: A General Form of Attribute Exploration

 * @FilePath: \peer-less.github.io\source\_posts\2012CS-A General Form of Attribute Exploration .md
   title:         A General Form of Attribute Exploration # 标题
   subtitle:                                              # 副标题
   date:          2020-06-30                              # 时间
   author         peerless                                # 作者
   heaeder-img:   img/post-bg-.jpg                        # 这篇文章标题背景图片
   catalog:       true                                    # 是否归档
   tags:                                                  # 标签
      - 学术
      - 属性探索
      --> 


# A General Form of Attribute Exploration

## 行文思路简要总结

问题：如何从经典属性探索出发获得通用的属性探索？

​			1.回顾经典属性探索算法。

​			2.通过引入3个条件扩展属性探索算法：

* 引入两个闭包算子$c_{cert}(A)$与$c_{univ}(A)$。
* 不明确指定提供的反例。
* 只向专家询问满足$c_{cert}(A)\subsetneq B\subset c_{univ}(A)$的蕴涵式$A\rightarrow B$。

​			3.对通用属性探索算法进行非冗余性与完备性验证。

​			4.对通用算法询问蕴涵式时进行改进，使算法向专家询问的蕴涵式的数量是最小的。

* 改进方法：若属性集$A=c_{cert}(A)\subsetneq c_{univ}(A)$，则向专家询问蕴涵式$A\rightarrow c_{univ}(A)$。

总结：

​			1.该通用属性算法首先保留了经典属性探索算法的大多性质：完备性、非冗余性和向专家询问蕴涵式的数量最少等。

​			2.该通用属性探索算法能够处理抽象给定的闭包算子和部分形式背景下给出的反例。

## 论文基本信息：

​			1.作者：Daniel Borchmann

​			2.期刊：Computer Science

​			3.时间：2018/11/15

## Abstract

* 提出一种属性探索的一般形式。
* 扩展属性探索的适用性。
* 将属性探索的现有变种转换为一般形式，简化理论。

## 1.Introduction

1.属性探索的变种：

* 部分形式背景的属性探索$^{[3]}$。
* 描述逻辑模型的探索$^{[1, 2]}$。

<font color='red'>研究问题：寻找一种将所有变种都包含在内的一般属性探索。</font>

可行原因：

 		1.属性探索算法的整体结构均保持不变。

 		2.属性探索的所有重要属性均保留了下来。

单词积累:

 		discourse: 论述、谈话、演讲。

​		 have a close look: 仔细研究。

## 2.Preliminaries

蕴涵式
$$
A\rightarrow B \Leftrightarrow A^{'}\subseteq B^{'} \Leftrightarrow B\subseteq A^{''}
$$

一些声明：

* $Imp(M)$：$M$上的所有蕴涵式集合。

* $Imp(K)$：形式背景$K$上的所有蕴涵式集合。

* $Th(K)$：形式背景$K$上的所有成立的蕴涵式集合。

* $L\subseteq Imp(K)$，$A\subseteq M$。如果对于所有的蕴涵式$(X\rightarrow Y)\in L$，都有$X\subsetneq A或Y\subseteq A$成立，则集合$A$在$L$下为封闭集合。则可做如下定义：
	$$
	\begin{align}
	L^0(A):&=A \newline
	L^1(A):&=\bigcup{ \{Y\mid (X\rightarrow Y)\in L, X\subseteq A\} } \newline
	L^i(A):&=L^1(L^{i-1}(A))\ for\ i > 1 \newline
	L(A):&=\bigcup\limits_{i\in N}{L^i(A)}
	\end{align}
	$$
	$L(A)$是基于$L$下比集合A大的最小集合。

	由上可知$L^N(A)$表示集合$A$的子集所能推出来的属性集合的子集所能推出来的属性集合。

	$Cn(L)$：在$L$下成立的所有蕴涵式集合。

	* $B\subseteq Imp(K)$在$L$下是非冗余的。$\Leftrightarrow B\subseteq Cn(L)$。
	* $B\subseteq Imp(K)$在$L$下是完备的。$\Leftrightarrow Cn(B)\supseteq L$。
	* $B$是主基。$\Leftrightarrow Cn(B)=Cn(L)$。

	$P$在$L$下是伪闭集，如果以下条件成立：

	* $P\neq L(P)$。
	* 对于所有的伪闭集$Q\subsetneq P$有$L(Q)\subseteq P$成立。

	特别的，若$L=Th(K)$，则$P$称为形式背景$K$的伪内涵。则主基可定义为：$Can(L):=\{P\rightarrow L(P)\mid P 在L下是伪闭集\}$。

	由于不知道对象$g$是否具有属性$m$，因此需要引入部分形式背景：存在一个属性集的有序对$(A, B), A, B\in M且A\bigcap B=\varnothing$所构成的集合即为部分形式背景。

	* 若$A\bigcup B=M$，则集合称为全局对象描述，即相应对象明确具有的属性集。
	* 若$A\bigcup B\neq M$，则集合称为部分对象描述，即相应对象明确不具有的属性集。

## 3.Classical Attribute Exploration

$M$是一个有限的属性集合，$K$是基于$M$的形式背景，$p$是基于$M$的领域专家。

1.对属性集通过字典序进行初始化，并获得第一个属性$\varnothing / P$.

2.如果$P^{'}=P$，跳到第5步；否则，令$r:=(P\rightarrow P^{"})$

3.如果专家认为$r$成立，把$r$加入蕴涵集$Imp(K)$中。

4.如果专家认为$r$不成立，给出相应的一个反例$C$，将$C$（及其相应的属性）作为新对象加入当前工作形式背景$K$中。

5.找到字典序$P$的下一个属性集$Q$，若不存在下一个，则算法终止，否则，将$P$设置为$Q$。

## 4.Generalizing Attribute Exploration

<font color='red'>推广目的：用更抽象的术语来描述属性探索，以允许该算法在经典属性探索算法之外的应用。</font>

3个扩展：

1.提出两个闭包操作符：$c_{univ}、c_{cert}$。

* $c_{univ}$：我们已知的全部领域知识。$c_{univ}(A)$可以从$A$推出的属性集。

* $c_{cert}$：我们已知的某些知识。$c_{cert}(A)$确定可以从$A$推出的属性集。

2.采用如下方法对算法进行扩展：

* 提供反例时，不需要完全指定。

* 只需要所提反例所拥有的信息与所给的蕴涵式相矛盾即可。

* 提供关于该对象具有哪些属性以及不具有的属性信息即可。

3.我们向专家提出的蕴涵式是一种特殊的形式：

 - 搜索关于$c_{cert}$和$c_{univ}$未确定的蕴涵式$A\rightarrow B$，即$c_{cert}(A)\subsetneq B\subseteq c_{univ}(A)$。对于这样的蕴涵式，我们不能从$c_{cert}$和$c_{univ}$推断出属性集$c_{univ}(A) \backslash B$是否能从$A$推出或不能推出，因此，我们需要向专家询问。

Algorithm(General Attribute Exploration)

   $q$为部分领域专家。  

   ​	1.$K = \varnothing$ 
   ​	2.对于有限属性集$A\subseteq M$，若存在有限属性集$B$，有$c_{cert}(A)\subsetneq B\subseteq c_{univ}(A)$成立，则考虑蕴涵式$A\rightarrow B$；如果不存在，则算法终止，输出$K$与$c_{cert}$。

   ​	3.若$q$认为$A\rightarrow B$成立，那么更新$c_{cert}^{'}=X\longmapsto c_{cert}(L(c_{cert}(X) ) )$。

​		4.否则，$(C, D)=q(A\rightarrow B)$作为反例，加入形式背景$K$。

​		5.对所有的反例$(C, D)$有：
$$
\begin{align}
C':&=c_{cert}(C) \newline
D':&=D\bigcup \{ {m\in M\backslash D\mid c_{cert}(C\bigcup \{m\})\bigcap D\neq \varnothing}\}
\end{align}
$$
​		6.对所有的$X\subseteq M，X\longmapsto c_{univ}(X)\bigcap K(X)$，即将$c_{univ}$更新为$c_{univ}^{'}(X)=c_{univ}(X)\bigcap K(X)$。

​		7.跳转到2。

两条性质：非冗余性与完备性。



## 5.Computing Undecided Implications

目的：使向专家询问蕴涵式的数量是最小的。

原因：该通用属性探索算法对询问蕴涵式的顺序未做约束。（可能询问以确定的蕴涵式）

经典属性探索下计算未确定的蕴涵式：

​		在基于已知蕴涵式$k$的条件下，计算属性集$P$字典序之后的最小属性集$Q(Q\subseteq M，且Q不是当前工作形式背景的内涵)$。那么就需要向专家询问蕴涵式$Q\rightarrow Q^{"}$。

通用属性探索下计算未确定的蕴涵式：

​		为了保证向专家询问的蕴涵式的数量是最小的，将通用属性探索算法的第二步更改为：$对于有限属性集A\subseteq M$，有$A=c_{cert}(A)\subsetneq c_{univ}(A)$并且$A$是$\subseteq-minimal$成立，则考虑蕴涵式$A\rightarrow c_{univ}(A)$。

在给出算法总是能得到最小数量的询问蕴涵式前，先给出如下定义：

## 6.Conclusions
 
<p style='text-indent:2em'>从使用领域专家的经典的属性探索中推导出一种更加通用的属性探索算法。它能够处理抽象给定的闭包算子，并且可以处理部分给定的反例。</p>

## 

