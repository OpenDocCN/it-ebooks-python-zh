## Python 补充 01 序列的方法

[`www.cnblogs.com/vamei/archive/2012/07/19/2599940.html`](http://www.cnblogs.com/vamei/archive/2012/07/19/2599940.html)

作者：Vamei 出处：http://www.cnblogs.com/vamei 欢迎转载，也请保留这段声明。谢谢！

在[快速教程](http://www.cnblogs.com/vamei/archive/2012/05/28/2522677.html)中，我们了解了最基本的序列(sequence)。回忆一下，序列包含有定值表(tuple)和表(list)。此外，字符串(string)是一种特殊的定值表。表的元素可以更改，定值表一旦建立，其元素不可更改。

任何的序列都可以引用其中的元素(item)。

下面的内建函数(built-in function)可用于序列(表，定值表，字符串):

# s 为一个序列

len(s)         返回： 序列中包含元素的个数

min(s)         返回： 序列中最小的元素

max(s)         返回： 序列中最大的元素

all(s)         返回： True, 如果所有元素都为 True 的话

any(s)         返回： True, 如果任一元素为 True 的话

下面的方法主要起查询功能，不改变序列本身, 可用于表和定值表:

sum(s)         返回：序列中所有元素的和

# x 为元素值，i 为下标(元素在序列中的位置)

s.count(x)     返回： x 在 s 中出现的次数

s.index(x)     返回： x 在 s 中第一次出现的下标

由于定值表的元素不可变更，下面方法只适用于表:

# l 为一个表, l2 为另一个表

l.extend(l2)        在表 l 的末尾添加表 l2 的所有元素

l.append(x)         在 l 的末尾附加 x 元素

l.sort()            对 l 中的元素排序

l.reverse()         将 l 中的元素逆序

l.pop()             返回：表 l 的最后一个元素，并在表 l 中删除该元素

del l[i]            删除该元素

(以上这些方法都是在原来的表的上进行操作，会对原来的表产生影响，而不是返回一个新表。)

下面是一些用于字符串的方法。尽管字符串是定值表的特殊的一种，但字符串(string)类有一些方法是改变字符串的。这些方法的本质不是对原有字符串进行操作，而是删除原有字符串，再建立一个新的字符串，所以并不与定值表的特点相矛盾。

#str 为一个字符串，sub 为 str 的一个子字符串。s 为一个序列，它的元素都是字符串。width 为一个整数，用于说明新生成字符串的宽度。

str.count(sub)       返回：sub 在 str 中出现的次数

str.find(sub)        返回：从左开始，查找 sub 在 str 中第一次出现的位置。如果 str 中不包含 sub，返回 -1

str.index(sub)       返回：从左开始，查找 sub 在 str 中第一次出现的位置。如果 str 中不包含 sub，举出错误

str.rfind(sub)       返回：从右开始，查找 sub 在 str 中第一次出现的位置。如果 str 中不包含 sub，返回 -1

str.rindex(sub)      返回：从右开始，查找 sub 在 str 中第一次出现的位置。如果 str 中不包含 sub，举出错误

str.isalnum()        返回：True， 如果所有的字符都是字母或数字

str.isalpha()        返回：True，如果所有的字符都是字母

str.isdigit()        返回：True，如果所有的字符都是数字

str.istitle()        返回：True，如果所有的词的首字母都是大写

str.isspace()        返回：True，如果所有的字符都是空格

str.islower()        返回：True，如果所有的字符都是小写字母

str.isupper()        返回：True，如果所有的字符都是大写字母

str.split([sep, [max]])    返回：从左开始，以空格为分割符(separator)，将 str 分割为多个子字符串，总共分割 max 次。将所得的子字符串放在一个表中返回。可以 str.split(',')的方式使用逗号或者其它分割符

str.rsplit([sep, [max]])   返回：从右开始，以空格为分割符(separator)，将 str 分割为多个子字符串，总共分割 max 次。将所得的子字符串放在一个表中返回。可以 str.rsplit(',')的方式使用逗号或者其它分割符

str.join(s)                返回：将 s 中的元素，以 str 为分割符，合并成为一个字符串。

str.strip([sub])           返回：去掉字符串开头和结尾的空格。也可以提供参数 sub，去掉位于字符串开头和结尾的 sub  

str.replace(sub, new_sub)  返回：用一个新的字符串 new_sub 替换 str 中的 sub

str.capitalize()           返回：将 str 第一个字母大写

str.lower()                返回：将 str 全部字母改为小写

str.upper()                返回：将 str 全部字母改为大写

str.swapcase()             返回：将 str 大写字母改为小写，小写改为大写

str.title()                返回：将 str 的每个词(以空格分隔)的首字母大写

str.center(width)          返回：长度为 width 的字符串，将原字符串放入该字符串中心，其它空余位置为空格。

str.ljust(width)           返回：长度为 width 的字符串，将原字符串左对齐放入该字符串，其它空余位置为空格。

str.rjust(width)           返回：长度为 width 的字符串，将原字符串右对齐放入该字符串，其它空余位置为空格。