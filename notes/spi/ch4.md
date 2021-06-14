# Lab 4：正则表达式

## 1.0 提取数字

* 结果 ： `\d+` 
  *  其中 `\d` 代表数字 0-9 。`+` 代表前面的子表达式出现一次或多次。也就是整数出现一个或多个都匹配上。
* 也可以用 `[0-9]+` 来匹配
  * 其中方括号 [] 代表匹配范围，匹配方括号中的任意字符，0-9 中的横线省略了 1，2，3... 等数字，也就是 [0-9] 等价于 [0123456789] 横线简写了范围。

## 2.0 提取负整数
* 结果： `-\d+` 
  * 在上一题的基础上加上负号就行了，
* 也可以采用 `-[0-9]+`

## 3.0 提取整数

整数包含正整数和负整数，也就是上面二者的结合，正整数的正号一般省略不写，所以二者区别就是是否出现负号。也就是负号出现一次或多次。

* 结果 ： `-?\d+`
  * 引入了新的符号问号(?) 代表前面出现的字符出现零次或一次。这样就实现了既可以提取正整数也可以提取负整数。

## 3.0 如何提取小写单词？
* `[a-z]+` 
  * 单词范围是小写 a-z 并且出现一次或多次。

## 4.0 如何提取小写字母？
* 结果： `([a-z])*`
  * 其中圆括号 ()表示其中内容是一个模块，也就是 [a-z] 只出现一个 其中星号 * 代表前面的子模式出现零次或多次
* 提取大写字母 `([A-Z])*`

## 5.0 提取 n 位数字

* `\d{n}`
  * 其中花括号 {} 表示范围, n 确定下来的范围 `\d{3}`  就代表 3 位数字

## 6.0 提取至少 n 位数字

* `\d{n,}` 
  * 花括号里面存在两个参数 n,m 第一个参数是上界,第二个参数是下界,如果第二个不写代表无穷,所以 `\d{3,}` 就代表至少 3 位整数.

## 7.0 提取 m 到 n 位数字

* `\d{m,n}`
  * `\d{3,10}` 意思就说提取 3 位到 10 的数字.

## 8.0 提取 0 或非零开头的数字

* 结果 `(0|[1-9][0-9]*)`
  * 其中圆括号代表一个模式也就是一个整体
  * 管道线 | 代表两侧的模式选择一个,如何满足前者就匹配,满足后者也匹配,而前者则代表零开头的数字,后者则代表非零开头的数字.
  * 其中 * 号代表前面的出现的 [0-9] 零次或多次而之前的 [1-9] 则固定了开头出现的数字不能是 0 

## 9.0 提取非零开头的且最多带两位小数的数字

* 结果: `([1-9][0-9]*)+(\.[0-9]{1,2})?`
  * 这个表达式分为两部分,整数部分和小数部分 采用圆括号分割开来, 而圆括号结尾部分采用加号 + 链接, 加号代表前面的子模式也就是圆括号中的匹配出现一次或多次. 
  * 其中整数部分就和上一题类似了,不明白可以回头看第八题.
  * 小数部分的特征则是以点号 . 开头,但是这个符号本身是元字符,代表匹配任意字符零次或一次,所以需要转移,使其恢复原意,也就是反斜杠 \ 的作用了.
  * 之后的匹配应该就很熟悉了,匹配所有数字并且花括号{} 保证了数字出现 1 到 2 位. 结尾用问号 ? 代表出现零次或一次,也就是小数部分的子模式了,可以没有小数部分,也可以存在,但是存在必须满足 1 - 2 位的小数.

## 10.0 带 1-2 位 小数的正数或负数?


# 正则表达式

正则表达式是元字符通过一定的规则来实现对文本的匹配，就像字符串查询一样，KMP 也是在匹配，只不过的是自身，而正则更为强大，通过元字符的组合可以匹配任何你想要的结果。

> 正则表达式的特点是：
> 1. 灵活性、逻辑性和功能性非常强；
> 2. 可以迅速地用极简单的方式达到字符串的复杂控制。
> 3. 对于刚接触的人来说，比较晦涩难懂。

其应用场景如下：
* 例如网址，`https://www.google.com/` 网址都存在相同的格式，如何在一堆乱的字符串将网址提取出来？
* 例如邮箱 `jiewei199@qq.com` 也存在特定的格式，如何文本中提取出邮箱。

而正则表达式就是提取这些信息的有力武器！而正则表达式则是由元字符组成。例如 \ ^ $ * + ? {}  等等

# 元字符

## . 

点用来匹配除**换行符**以外的**任意单个**字符。注意是一个字符！

e.g. 

* a. => '<font color ="#1abc9c"><strong>ap</strong></font>le' '<font color ="#1abc9c"><strong>an</strong></font>other' ''<font color ="#1abc9c"><strong>ad</strong></font>dress' 
* w. => '<font color ="#1abc9c"><strong>wh</strong></font>ite' '<font color ="#1abc9c"><strong>wr</strong></font>ite' '<font color ="#1abc9c"><strong>wo</strong></font>uld'
* c. => '<font color ="#1abc9c"><strong>ca</strong></font>ttle' '<font color ="#1abc9c"><strong>co</strong></font>py' '<font color ="#1abc9c"><strong>co</strong></font>de'

## [] 
方括号用来匹配方括号中包含的任意字符。

例如：
* [0-9] => '<font color ="#1abc9c"><strong>2001</strong></font>xxx' 
  * 匹配含有数字的字符。
* [a-z] => '<font color ="#1abc9c"><strong>kkk</strong></font>200'<font color ="#1abc9c"><strong>coding</strong></font>1777' 
  * 匹配含有小写字母的字符。 
* [A-Z] => '<font color ="#1abc9c"><strong>A</strong></font>xxx<font color ="#1abc9c"><strong>CODE</strong></font>xxx' 
  * 匹配含有大写字母的字符。
* [a-zA-Z0-9] '<font color ="#1abc9c"><strong>2001JieWeiqq</strong></font>'
  * 匹配数字大小写字母
* '[pjc]ython' 可以匹配 'python' 'jython' 'cython' 。
* '[a-zA-Z0-9]' 可以匹配任意大小写字母或数字。

## *   
星号用来匹配前面的子表示**零次**或**多次**

e.t.
* app*le => '<font color ="#1abc9c"><strong>aple</strong></font>' '<font color ="#1abc9c"><strong>appppppppple</strong></font>'
  * 星号代表其前面的字符 p 可以出现零次或多次的都能匹配上。
* stu*dy => '<font color ="#1abc9c"><strong>stdy</strong></font>' '<font color ="#1abc9c"><strong>study</strong></font>' '<font color ="#1abc9c"><strong>stuuuuudy</strong></font>'

## +
加号代表前面的字符出现一次或多次。

e.t.
* app+le => '<font color ="#1abc9c"><strong>apple</strong></font>' '<font color ="#1abc9c"><strong>appppppppple</strong></font>'
  * 加号代表其前面的字符 p 可以出现一次或多次的都能匹配上。
* stu+dy => '<font color ="#1abc9c"><strong>study</strong></font>' '<font color ="#1abc9c"><strong>stuuuuudy</strong></font>'


## ？
问好表示可以匹配前面出现的字符零次或一次。或者指明非贪婪模式。

e.t.
* app?le => '<font color ="#1abc9c"><strong>aple</strong></font>' '<font color ="#1abc9c"><strong>aple</strong></font>'
  * 加号代表其前面的字符 p 可以出现一次或多次的都能匹配上。
* stu?dy => '<font color ="#1abc9c"><strong>study</strong></font>' '<font color ="#1abc9c"><strong>stdy</strong></font>'
* aa test1 bb test2 cc '.*?' => '<font color ="#1abc9c"><strong>aa</strong></font>'

## ^

否定字符。

* [^a-z]
  * 匹配出结果中将小写剔除出去。
* [^0-9]
  * 匹配出结果中将数字剔除出去。

## {n,m}
花括号，表示范围，n 则是前面字符至少 n 次，但是不超过 m 次。

* [a-z]{2,3} => ''

## (xyz)
指定顺序。

## |
选择，或 的意思，管道线前后的字符都能匹配上。

## $

匹配行的结束。

# 间写字符集
## \
转义字符，使字符恢复本来的含义。

例如匹配问号 ？ 问号本身具备一个特殊的含义即匹配之前字符出现零次或一次，但是如何匹配问好本身，加上反斜杠即可 \? 这样就使问好 ? 恢复其原意。

不仅如此，反斜杠和字母组合实现了更多的功能。

## \w
\w 相当于 [a-zA-Z0-9_] 匹配了大小写字母以及数字。而 \W 大写字母则等价于 [^\w] 。

## \d
\d 用来匹配数字 \D 用来匹配非数字。

## \s
\s 用来匹配空格符，\S 则用来匹配非空格符。

## 参考

[1]. https://www.cnblogs.com/zxin/archive/2013/01/26/2877765.html
[2]. https://github.com/cdoco/learn-regex-zh