# Part 5.

How do you tackle something as complex as 
understanding how to create an interpreter 
or compiler? In the beginning it all looks 
pretty much like a tangled mess of yarn that 
you need to untangle to get that perfect ball.

如何解决理解创建一个解释器或编译器这样复杂的事情？
一开始看起来都挺像一团纠结的纱线，你需要解开它才能得到那个完美的球。

The way to get there is to just untangle it one 
thread, one knot at a time. Sometimes, though, 
you might feel like you don’t understand something 
right away, but you have to keep going. It will 
eventually “click” if you’re persistent enough, 
I promise you (Gee, if I put aside 25 cents every 
time I didn’t understand something right away 
I would have become rich a long time ago :).

办法就是把线解开就可以了，一次一个节。有时，尽管你不能马上理解，但是你必须坚持前进。
只要你足够执着，最终都能够理解。我向你保证。天啊，如果我每次不懂的时候马上拿出25美分，我早就发财了:) 。

Probably one of the best pieces of advice 
I could give you on your way to understanding 
how to create an interpreter and compiler is 
to read the explanations in the articles, 
read the code, and then write code yourself, 
and even write the same code several times over 
a period of time to make the material and code 
feel natural to you, and only then move on to 
learn new topics. Do not rush, just slow down 
and take your time to understand the basic ideas 
deeply. This approach, while seemingly slow, will 
pay off down the road. Trust me.

在你了解如何创建一个解释器和编译器的路上，我可能会给你一个最好的建议，
那就是阅读文章中的解释，阅读代码，然后自己写代码，甚至在一段时间内多次写同样的代码，
让你觉得材料和代码很自然，然后才继续学习新的主题。不要着急，放慢脚步花一些时间来深
入理解基础思想。这种方法看似缓慢，但是在以后的学习中会获得回报，相信我。

You will eventually get your perfect ball of yarn in the end. 
And, you know what? Even if it is not that perfect it is still 
better than the alternative, which is to do nothing and not learn 
the topic or quickly skim over it and forget it in a couple of days.

你最终会得到你完美的“纱线球”。而且，你知道吗？即使不是那么完美，
也比另一种方法要好，那就是什么都不做，不学这个题目，或者快速浏览，过两天就忘了。

Remember - just keep untangling: one thread, one knot at a time and 
practice what you’ve learned by writing code, a lot of it:

记住--只要不断地解开：一根线，一个结，通过写代码来练习你所学到的东西，大量的代码。

![lsbasi_part5_ballofyarn](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/lsbasi_part5_ballofyarn.3tkr6uybydw0.png)

Today you’re going to use all the knowledge you’ve gained from 
previous articles in the series and learn how to parse and interpret 
arithmetic expressions that have any number of addition, subtraction, 
multiplication, and division operators. You will write an interpreter 
that will be able to evaluate expressions like “14 + 2 * 3 - 6 / 2”.

今天，你将利用你从本系列前几篇文章中获得的所有知识，学习如何解析和解释
具有任意数量加、减、乘、除运算符的算术表达式。你将编写一个解释器，它将
能够评估 "14 + 2 * 3 - 6 / 2 "这样的表达式。

Before diving in and writing some code let’s talk about the associativity and precedence of operators.

在潜心写一些代码之前，我们先来谈谈操作符的关联性和优先性。

By convention 7 + 3 + 1 is the same as (7 + 3) + 1 and 
7 - 3 - 1 is equivalent to (7 - 3) - 1. No surprises here. 
We all learned that at some point and have been taking it 
for granted since then. If we treated 7 - 3 - 1 as 7 - (3 - 1) 
the result would be unexpected 5 instead of the expected 3.

按照惯例，7+3+1 等价于 (7+3)+1 ，7-3-1 等价于 (7-3)-1 。
我们都是在某个时刻学会了这一点，并且从那时起就把它视为理所当然。
如果我们把7 - 3 - 1当作7 - (3 - 1)，结果将是意外的5，而不是预期的3。

In ordinary arithmetic and most programming languages addition, subtraction, multiplication, and division are left-associative:

在普通算术和大多数编程语言中，加、减、乘、除都是左联的。

```python
7 + 3 + 1 is equivalent to (7 + 3) + 1
7 - 3 - 1 is equivalent to (7 - 3) - 1
8 * 4 * 2 is equivalent to (8 * 4) * 2
8 / 4 / 2 is equivalent to (8 / 4) / 2
```

What does it mean for an operator to be left-associative?

运算符的左联是什么意思？

When an operand like 3 in the expression 7 + 3 + 1 has plus 
signs on both sides, we need a convention to decide which 
operator applies to 3. Is it the one to the left or the one 
to the right of the operand 3? The operator + associates to 
the left because an operand that has plus signs on both sides 
belongs to the operator to its left and so we say that the 
operator + is left-associative. That’s why 7 + 3 + 1 is 
equivalent to (7 + 3) + 1 by the associativity convention.

当表达式 7+3+1 中像 3 这样的操作数两边都有加号时，
我们需要一个约定俗成的方式来决定哪个操作数适用于3，
是操作数3左边的操作数还是右边的操作数？运算符+联想到左边，
因为一个操作数如果两边都有加号，就属于它左边的操作数，
所以我们说操作数+是左连接的。这就是为什么7+3+1按联想约定
等同于（7+3）+1的原因。

Okay, what about an expression like 7 + 5 * 2 
where we have different kinds of operators on 
both sides of the operand 5? Is the expression 
equivalent to 7 + (5 * 2) or (7 + 5) * 2? How 
do we resolve this ambiguity?

好吧，如果我们在操作数 5 的两边都有不同类型的运算符，
那么像 7 + 5 * 2 这样的表达式呢？这个表达式是相当于 7 + (5 * 2) 还是 (7 + 5) * 2 ？
我们如何解决这种歧义？

In this case, the associativity convention is of 
no help to us because it applies only to operators 
of one kind, either additive (+, -) or multiplicative (*, /). 
We need another convention to resolve the ambiguity 
when we have different kinds of operators in the 
same expression. We need a convention that defines 
relative precedence of operators.

在这种情况下，关联性约定对我们没有帮助，
因为它只适用于一种运算符，即加法（+，-）或乘法（*，/）。
当我们在同一个表达式中有不同种类的运算符时，
我们需要另一个约定来解决这种模糊性。
我们需要一个公约来定义运算符的相对优先性。

And here it is: we say that if the operator * takes 
its operands before + does, then it has higher precedence. 
In the arithmetic that we know and use, multiplication 
and division have higher precedence than addition and subtraction. 
As a result the expression 7 + 5 * 2 is equivalent to 7 + (5 * 2) 
and the expression 7 - 8 / 4 is equivalent to 7 - (8 / 4).

这里就是：我们说，如果运算符 * 比 + 先取操作数，
那么它就有更高的优先性。在我们知道和使用的算术中，
乘法和除法比加法和减法具有更高的优先性。因此，表达式 7 + 5 * 2 相当
于 7 + (5 * 2)，表达式 7 - 8 / 4 相当于 7 - (8 / 4) 。

In a case where we have an expression with operators 
that have the same precedence, we just use the associativity 
convention and execute the operators from left to right:

在我们有一个表达式的运算符具有相同优先级的情况下，我们只需使用关联性惯例，从左到右执行运算符。

```python
7 + 3 - 1 is equivalent to (7 + 3) - 1
8 / 4 * 2 is equivalent to (8 / 4) * 2
```

I hope you didn’t think I wanted to bore you to death 
by talking so much about the associativity and precedence of operators. 
The nice thing about those conventions is that we can construct a grammar 
for arithmetic expressions from a table that shows the associativity and 
precedence of arithmetic operators. Then, we can translate the grammar 
into code by following the guidelines I outlined in Part 4, and our interpreter 
will be able to handle the precedence of operators in addition to associativity.

我希望你不要以为我说了这么多关于运算符的关联性和优先性的内容，是想让你无聊死。
这些约定的好处是，我们可以从一个显示算术运算符关联性和优先性的表格中构建一个算
术表达式的语法。然后，我们可以按照我在第 4 部分中概述的准则将语法翻译成代码，
我们的解释器将能够处理运算符的关联性和优先性之外的优先性。

Okay, here is our precedence table:

好了，这是我们的优先级表。

![lsbasi_part5_precedence](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/lsbasi_part5_precedence.oiue0cuihds.png)

From the table, you can tell that operators + and - have the same precedence 
level and they are both left-associative. You can also see that operators * and / are 
also left-associative, have the same precedence among themselves but have 
higher-precedence than addition and subtraction operators.

从表中可以看出，运算符+和-具有相同的优先级，它们都是左相关的。你还可以看到，
运算符*和/也是左联，它们之间的优先级相同，但比加减运算符的优先级高。

Here are the rules for how to construct a grammar from the precedence table:

下面是如何从先例表构建语法的规则。

1. For each level of precedence define a non-terminal. 
The body of a production for the non-terminal should 
contain arithmetic operators from that level and non-terminals 
for the next higher level of precedence.

对每一级的优先级定义一个非终结符。非终结符的产生式主体应包含该级别的算术运算符和下一个更高等级的非终结符。

2. Create an additional non-terminal factor for basic 
units of expression, in our case, integers. The general 
rule is that if you have N levels of precedence, you will 
need N + 1 non-terminals in total: one non-terminal for 
each level plus one non-terminal for basic units of expression.

为表达式的基本单位，在我们的例子中，整数，创建一个额外的非终结因子。
一般规则是，如果你有N个级别的优先级，你总共需要N+1个非终结因子：
每个级别一个非终结因子加上一个基本表达单位的非终结因子。

Onward!

继续前进！

Let’s follow the rules and construct our grammar.

让我们按照规则，构建我们的语法。

According to Rule 1 we will define two non-terminals: 
a non-terminal called expr for level 2 and a non-terminal 
called term for level 1. And by following Rule 2 we will define 
a factor non-terminal for basic units of arithmetic expressions, integers.

根据规则1，我们将定义两个非终结符：一个叫做expr的非终结符为2级，一个叫做term的非终结符为1级。而根据规则2，我们将为算术表达式的基本单位--整数定义一个因子非项。

The start symbol of our new grammar will be expr and the expr production 
will contain a body representing the use of operators from level 2, 
which in our case are operators + and - , and will contain term non-terminals 
for the next higher level of precedence, level 1:

我们的新语法的起始符号将是 expr，而 expr 生产将包含一个代表使用第 2 级运算符的主体，
在我们的例子中是运算符 + 和 - ，并将包含下一个更高的优先级，即第 1 级的非术语。

![lsbasi_part5_cfg_expr](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/lsbasi_part5_cfg_expr.2gnx8v3dr4n4.png)

The term production will have a body representing the use of operators from level 1, which are operators * and / , and it will contain the non-terminal factor for the basic units of expression, integers:

生产一词将有一个代表使用从第1级运算符的主体，即运算符*和/ ，它将包含表达式的基本单位--整数的非项系数。

![lsbasi_part5_cfg_term](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/lsbasi_part5_cfg_term.3kga22itnxe0.png)

And the production for the non-terminal factor will be:

而非终端符的产生式将是。

![lsbasi_part5_cfg_factor](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/lsbasi_part5_cfg_factor.7ed87wmqoig0.png)

You’ve already seen above productions as part of grammars 
and syntax diagrams from previous articles, but here we combine 
them into one grammar that takes care of the associativity and 
the precedence of operators:

你已经在之前的文章中看到了上述产品作为语法和句法图的一部分，
但在这里，我们将它们合并成一个语法，以处理操作符的关联性和优先性。

![lsbasi_part5_grammar](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/lsbasi_part5_grammar.3pvtu8kjoj60.png)

Here is a syntax diagram that corresponds to the grammar above:

下面是与上述语法相对应的语法图。

![lsbasi_part5_syntaxdiagram](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/lsbasi_part5_syntaxdiagram.7gqskfipjww0.png)
Each rectangular box in the diagram is a “method call” to another diagram. If you take the expression 7 + 5 * 2 and start with the top diagram expr and walk your way down to the bottommost diagram factor, you should be able to see that higher-precedence operators * and / in the lower diagram execute before operators + and - in the higher diagram.

图中的每个矩形框都是对另一个图的 "方法调用"。如果你把表达式7 + 5 * 2从最上面的图 expr 开始，一直走到最下面的图因子，你应该能够看到，下图中的高位运算符*和/在上图中的运算符+和-之前执行。

To drive the precedence of operators point home, let’s take a look at the decomposition of the same arithmetic expression 7 + 5 * 2 done in accordance with our grammar and syntax diagrams above. This is just another way to show that higher-precedence operators execute before operators with lower precedence:

为了说明运算符的优先级，我们来看看按照我们上面的语法和句法图所做的同一个算术表达式7 + 5 * 2的分解。这只是另一种方式来说明优先级较高的运算符在优先级较低的运算符之前执行。

![lsbasi_part5_exprdecomp](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/lsbasi_part5_exprdecomp.5wgi3mn1wqo0.png)

Okay, let’s convert the grammar to code following guidelines from Part 4 and see how our new interpreter works, shall we?

好了，让我们按照第4部分的指南将语法转换为代码，看看我们的新解释器如何工作，好吗？

Here is the grammar again:

这里又是语法：

![lsbasi_part5_grammar](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/lsbasi_part5_grammar.mh6y7smgkao.png)

And here is the complete code of a calculator that can handle valid arithmetic expressions containing integers and any number of addition, subtraction, multiplication, and division operators.

而这里是一个计算器的完整代码，它可以处理包含整数和任何数量的加减乘除运算符的有效算术表达式。

The following are the main changes compared with the code from Part 4:

与第四部分的代码相比，主要变化如下:

* The Lexer class can now tokenize +, -, *, and / (Nothing new here, we just combined code from previous articles into one class that supports all those tokens)
* Recall that each rule (production), R, defined in the grammar, becomes a method with the same name, and references to that rule become a method call: R(). As a result the Interpreter class now has three methods that correspond to non-terminals in the grammar: expr, term, and factor.

Source code:

源代码：

```python
# Token types
#
# EOF (end-of-file) token is used to indicate that
# there is no more input left for lexical analysis
INTEGER, PLUS, MINUS, MUL, DIV, EOF = (
    'INTEGER', 'PLUS', 'MINUS', 'MUL', 'DIV', 'EOF'
)


class Token(object):
    def __init__(self, type, value):
        # token type: INTEGER, PLUS, MINUS, MUL, DIV, or EOF
        self.type = type
        # token value: non-negative integer value, '+', '-', '*', '/', or None
        self.value = value

    def __str__(self):
        """String representation of the class instance.

        Examples:
            Token(INTEGER, 3)
            Token(PLUS, '+')
            Token(MUL, '*')
        """
        return 'Token({type}, {value})'.format(
            type=self.type,
            value=repr(self.value)
        )

    def __repr__(self):
        return self.__str__()


class Lexer(object):
    def __init__(self, text):
        # client string input, e.g. "3 * 5", "12 / 3 * 4", etc
        self.text = text
        # self.pos is an index into self.text
        self.pos = 0
        self.current_char = self.text[self.pos]

    def error(self):
        raise Exception('Invalid character')

    def advance(self):
        """Advance the `pos` pointer and set the `current_char` variable."""
        self.pos += 1
        if self.pos > len(self.text) - 1:
            self.current_char = None  # Indicates end of input
        else:
            self.current_char = self.text[self.pos]

    def skip_whitespace(self):
        while self.current_char is not None and self.current_char.isspace():
            self.advance()

    def integer(self):
        """Return a (multidigit) integer consumed from the input."""
        result = ''
        while self.current_char is not None and self.current_char.isdigit():
            result += self.current_char
            self.advance()
        return int(result)

    def get_next_token(self):
        """Lexical analyzer (also known as scanner or tokenizer)

        This method is responsible for breaking a sentence
        apart into tokens. One token at a time.
        """
        while self.current_char is not None:

            if self.current_char.isspace():
                self.skip_whitespace()
                continue

            if self.current_char.isdigit():
                return Token(INTEGER, self.integer())

            if self.current_char == '+':
                self.advance()
                return Token(PLUS, '+')

            if self.current_char == '-':
                self.advance()
                return Token(MINUS, '-')

            if self.current_char == '*':
                self.advance()
                return Token(MUL, '*')

            if self.current_char == '/':
                self.advance()
                return Token(DIV, '/')

            self.error()

        return Token(EOF, None)


class Interpreter(object):
    def __init__(self, lexer):
        self.lexer = lexer
        # set current token to the first token taken from the input
        self.current_token = self.lexer.get_next_token()

    def error(self):
        raise Exception('Invalid syntax')

    def eat(self, token_type):
        # compare the current token type with the passed token
        # type and if they match then "eat" the current token
        # and assign the next token to the self.current_token,
        # otherwise raise an exception.
        if self.current_token.type == token_type:
            self.current_token = self.lexer.get_next_token()
        else:
            self.error()

    def factor(self):
        """factor : INTEGER"""
        token = self.current_token
        self.eat(INTEGER)
        return token.value

    def term(self):
        """term : factor ((MUL | DIV) factor)*"""
        result = self.factor()

        while self.current_token.type in (MUL, DIV):
            token = self.current_token
            if token.type == MUL:
                self.eat(MUL)
                result = result * self.factor()
            elif token.type == DIV:
                self.eat(DIV)
                result = result / self.factor()

        return result

    def expr(self):
        """Arithmetic expression parser / interpreter.

        calc>  14 + 2 * 3 - 6 / 2
        17

        expr   : term ((PLUS | MINUS) term)*
        term   : factor ((MUL | DIV) factor)*
        factor : INTEGER
        """
        result = self.term()

        while self.current_token.type in (PLUS, MINUS):
            token = self.current_token
            if token.type == PLUS:
                self.eat(PLUS)
                result = result + self.term()
            elif token.type == MINUS:
                self.eat(MINUS)
                result = result - self.term()

        return result


def main():
    while True:
        try:
            # To run under Python3 replace 'raw_input' call
            # with 'input'
            text = raw_input('calc> ')
        except EOFError:
            break
        if not text:
            continue
        lexer = Lexer(text)
        interpreter = Interpreter(lexer)
        result = interpreter.expr()
        print(result)


if __name__ == '__main__':
    main()
```

Save the above code into the calc5.py file or download it directly from GitHub. 
As usual, try it out and see for yourself that the interpreter properly evaluates 
arithmetic expressions that have operators with different precedence.

将上面的代码保存到 calc5.py 文件中，或者直接从 GitHub 上下载。
像往常一样，尝试一下，自己看看解释器是否能正确地评估具有不同优先级运算符的算术表达式。

Here is a sample session on my laptop:

这是我的笔记本电脑上的一个样本会话。

```python
$ python calc5.py
calc> 3
3
calc> 2 + 7 * 4
30
calc> 7 - 8 / 4
5
calc> 14 + 2 * 3 - 6 / 2
17
```

Here are new exercises for today:

下面是今天的新练习。

Write an interpreter as described in this article off the top of your head, without peeking into the code from the article. Write some tests for your interpreter, and make sure they pass.

在不偷看文章中的代码的情况下，按照这篇文章中描述的内容脱口而出，写一个解释器。为你的解释器写一些测试，并确保它们通过。

Extend the interpreter to handle arithmetic expressions containing parentheses so that your interpreter could evaluate deeply nested arithmetic expressions like: 7 + 3 * (10 / (12 / (3 + 1) - 1))

扩展解释器以处理包含小括号的算术表达式，这样您的解释器就可以评估深嵌套的算术表达式，如：7 + 3 * (10 / (12 / (3 + 1) - 1))。7 + 3 * (10 / (12 / (3 + 1) - 1))

Check your understanding.

检查你的理解。

What does it mean for an operator to be left-associative?

运算符的左联是什么意思？

Are operators + and - left-associative or right-associative? What about * and / ?

运算符+和-是左关联的还是右关联的？那*和/呢？

Does operator + have higher precedence than operator * ?

运算符 + 比运算符 * 有更高的优先权吗？

Hey, you read all the way to the end! That’s really great. I’ll be back next time with a new article - stay tuned, be brilliant, and, as usual, don’t forget to do the exercises.

嘿，你都读到最后了! 那真是太好了。下一次我会带着新的文章回来--敬请期待，要有出色的表现，而且，和往常一样，别忘了做练习。

Here is a list of books I recommend that will help you in your study of interpreters and compilers:

下面是我推荐的书单，对你学习解释器和编译器会有所帮助。

1. [Language Implementation Patterns: Create Your Own Domain-Specific and General Programming Languages (Pragmatic Programmers)](https://www.amazon.com/gp/product/193435645X/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=193435645X&linkCode=as2&tag=russblo0b-20&linkId=MP4DCXDV6DJMEJBL)
2. [Writing Compilers and Interpreters: A Software Engineering Approach](https://www.amazon.com/gp/product/0470177071/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0470177071&linkCode=as2&tag=russblo0b-20&linkId=UCLGQTPIYSWYKRRM)
3. [Modern Compiler Implementation in Java](https://www.amazon.com/gp/product/052182060X/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=052182060X&linkCode=as2&tag=russblo0b-20&linkId=ZSKKZMV7YWR22NMW)
4. [Modern Compiler Design](https://www.amazon.com/gp/product/1461446988/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1461446988&linkCode=as2&tag=russblo0b-20&linkId=PAXWJP5WCPZ7RKRD)
5. [Compilers: Principles, Techniques, and Tools (2nd Edition)](https://www.amazon.com/gp/product/0321486811/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0321486811&linkCode=as2&tag=russblo0b-20&linkId=GOEGDQG4HIHU56FQ)