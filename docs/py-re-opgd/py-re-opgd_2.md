# 简单模式

## 简单模式

我们将从最简单的正则表达式学习开始。由于正则表达式常用于字符串操作，那我们就从最常见的任务：字符匹配 下手。

有关正则表达式底层的计算机科学上的详细解释（确定性和非确定性有限自动机），你可以查阅编写编译器相关的任何教科书。

# 字符匹配

### 字符匹配

大多数字母和字符一般都会和自身匹配。例如，正则表达式 test 会和字符串“test”完全匹配。（你也可以使用大小写不敏感模式，它还能让这个 RE 匹配“Test”或“TEST”；稍后会有更多解释。）

这个规则当然会有例外；有些字符比较特殊，它们和自身并不匹配，而是会表明应和一些特殊的东西匹配，或者它们会影响到 RE 其它部分的重复次数。本文很大篇幅专门讨论了各种元字符及其作用。

这里有一个元字符的完整列表；其含义会在本指南余下部分进行讨论。

```py
. ^ $ * + ? { [ ] \ | ( ) 
```

我们首先考察的元字符是"[" 和 "]"。它们常用来指定一个字符类别，所谓字符类别就是你想匹配的一个字符集。字符可以单个列出，也可以用“-”号分隔的两个给定字符来表示一个字符区间。例如，[abc] 将匹配"a", "b", 或 "c"中的任意一个字符；也可以用区间[a-c]来表示同一字符集，和前者效果一致。如果你只想匹配小写字母，那么 RE 应写成 [a-z].

元字符在类别里并不起作用。例如，[akm$]将匹配字符"a", "k", "m", 或 "$" 中的任意一个；"$"通常用作元字符，但在字符类别里，其特性被除去，恢复成普通字符。

你可以用补集来匹配不在区间范围内的字符。其做法是把"^"作为类别的首个字符；其它地方的"^"只会简单匹配 "^"字符本身。例如，^(5) 将匹配除 "5" 之外的任意字符。

也许最重要的元字符是反斜杠"\"。 做为 Python 中的字符串字母，反斜杠后面可以加不同的字符以表示不同特殊意义。它也可以用于取消所有的元字符，这样你就可以在模式中匹配它们了。举个例子，如果你需要匹配字符 "[" 或 "\"，你可以在它们之前用反斜杠来取消它们的特殊意义： [ 或 \。

一些用 "\" 开始的特殊字符所表示的预定义字符集通常是很有用的，象数字集，字母集，或其它非空字符集。下列是可用的预设特殊字符：

```py
\d  匹配任何十进制数；它相当于类 [0-9]。
\D  匹配任何非数字字符；它相当于类 [⁰-9]。
\s  匹配任何空白字符；它相当于类  [ \t\n\r\f\v]。
\S  匹配任何非空白字符；它相当于类 [^ \t\n\r\f\v]。
\w  匹配任何字母数字字符；它相当于类 [a-zA-Z0-9_]。
\W  匹配任何非字母数字字符；它相当于类 [^a-zA-Z0-9_]。 
```

这样特殊字符都可以包含在一个字符类中。如，[\s,.]字符类将匹配任何空白字符或","或"."。

本节最后一个元字符是 . 。它匹配除了换行字符外的任何字符，在 alternate 模式（re.DOTALL）下它甚至可以匹配换行。"." 通常被用于你想匹配“任何字符”的地方。

# 重复

### 重复

正则表达式第一件能做的事是能够匹配不定长的字符集，而这是其它能作用在字符串上的方法所不能做到的。 不过，如果那是正则表达式唯一的附加功能的话，那么它们也就不那么优秀了。它们的另一个功能就是你可以指定正则表达式的一部分的重复次数。

我们讨论的第一个重复功能的元字符是 *。* 并不匹配字母字符 "*"；相反，它指定前一个字符可以被匹配零次或更多次，而不是只有一次。

举个例子，ca*t 将匹配 "ct" (0 个 "a" 字符), "cat" (1 个 "a"), "caaat" (3 个 "a" 字符)等等。RE 引擎有各种来自 C 的整数类型大小的内部限制，以防止它匹配超过 20 亿个 "a" 字符；你也许没有足够的内存去建造那么大的字符串，所以将不会累计到那个限制。

象 * 这样地重复是“贪婪的”；当重复一个 RE 时，匹配引擎会试着重复尽可能多的次数。如果模式的后面部分没有被匹配，匹配引擎将退回并再次尝试更小的重复。

一步步的示例可以使它更加清晰。让我们考虑表达式 a[bcd]*b。它匹配字母 "a"，零个或更多个来自类 [bcd]中的字母，最后以 "b" 结尾。现在想一想该 RE 对字符串 "abcbd" 的匹配。

| Step | Matched | Explanation |
| --- | --- | --- |
| 1 | a | a 匹配模式 |
| 2 | abcbd | 引擎匹配 [bcd]*，并尽其所能匹配到字符串的结尾 |
| 3 | Failure | 引擎尝试匹配 b，但当前位置已经是字符的最后了，所以失败 |
| 4 | abcb | 退回，[bcd]*尝试少匹配一个字符。 |
| 5 | Failure | 再次尝次 b，但在当前最后一位字符是"d"。 |
| 6 | abc | 再次退回，[bcd]*只匹配 "bc"。 |
| 7 | abcb | 再次尝试 b ，这次当前位上的字符正好是 "b" |

RE 的结尾部分现在可以到达了，它匹配 "abcb"。这证明了匹配引擎一开始会尽其所能进行匹配，如果没有匹配然后就逐步退回并反复尝试 RE 剩下来的部分。直到它退回尝试匹配 [bcd] 到零次为止，如果随后还是失败，那么引擎就会认为该字符串根本无法匹配 RE 。

另一个重复元字符是 +，表示匹配一或更多次。请注意 *和 + 之间的不同；* 匹配零或更多次，所以可以根本就不出现，而 + 则要求至少出现一次。用同一个例子，ca+t 就可以匹配 "cat" (1 个 "a")， "caaat" (3 个 "a")， 但不能匹配 "ct"。

还有更多的限定符。问号 ? 匹配一次或零次；你可以认为它用于标识某事物是可选的。例如：home-?brew 匹配 "homebrew" 或 "home-brew"。

最复杂的重复限定符是 {m,n}(注意 m,n 之间不能有空格)，其中 m 和 n 是十进制整数。该限定符的意思是至少有 m 个重复，至多到 n 个重复。举个例子，a/{1,3}b 将匹配 "a/b"，"a//b" 和 "a///b"。它不能匹配 "ab" 因为没有斜杠，也不能匹配 "a////b" ，因为有四个。

你可以忽略 m 或 n；因为会为缺失的值假设一个合理的值。忽略 m 会认为下边界是 0，而忽略 n 的结果将是上边界为无穷大 -- 实际上是先前我们提到的 20 亿，但这也许同无穷大一样。

细心的读者也许注意到其他三个限定符都可以用这样方式来表示。 {0,} 等同于 *，{1,} 等同于 +，而{0,1}则与 ? 相同。如果可以的话，最好使用* ，+，或?。很简单因为它们更短也更容易懂。