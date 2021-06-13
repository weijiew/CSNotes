# Part 3.

I woke up this morning and I thought to myself: 
“Why do we find it so difficult to learn a new skill?”

今天早上醒来后，我在想：“为什么学习一门新技能如此的难？”

I don’t think it’s just because of the hard work. 
I think that one of the reasons might be that we 
spend a lot of time and hard work acquiring knowledge 
by reading and watching and not enough time translating 
that knowledge into a skill by practicing it. 
Take swimming, for example. You can spend a lot of time 
reading hundreds of books about swimming, talk for hours 
with experienced swimmers and coaches, watch all the training 
videos available, and you still will sink like a rock the 
first time you jump in the pool.

我想这不仅仅是因为工作难。我认为其中之一的原因可能是花费了大量的时间去阅读来获取知识，
而没有花费足够的时间通过训练将知识转化为技能。
以游泳为例，你可以花费大量的时间来阅读游泳相关的书籍，和游泳教练交流，浏览游泳训练的视频，
但是当你第一次跳入泳池中，依旧像火箭一样沉入水中。

The bottom line is: it doesn’t matter how well you think you 
know the subject - you have to put that knowledge into practice 
to turn it into a skill. To help you with the practice part I 
put exercises into Part 1 and Part 2 of the series.  
And yes, you will see more exercises in today’s article and in future articles, I promise :)

底线是：不管你认为你掌握的多么好的学科，你依旧需要通过训练将知识转换为技能。
为了帮助你练习部分，我把练习放到了系列的第一部分和第二部分。
你会在今天的文章和以后的文章中看到更多的练习，我保证:)

Okay, let’s get started with today’s material, shall we?

好了，让我们开始今天的材料，好吗？

So far, you’ve learned how to interpret arithmetic expressions 
that add or subtract two integers like “7 + 3” or “12 - 9”. 
Today I’m going to talk about how to parse (recognize) and interpret 
arithmetic expressions that have any number of plus or minus operators 
in it, for example “7 - 3 + 2 - 1”.

到目前为止，你已经学习了如何解释像加法或减法，类似于 “7 + 3” 或者 “12 - 9” 一样的算术表达式。
今天，我想说的是如何计息多个数字或操作符的算术表达式，例如 “7 - 3 + 2 - 1” 。

Graphically, the arithmetic expressions in this article can be 
represented with the following syntax diagram:

从图形上看，本文的算术表达式可以用下面的语法图来表示。

![lsbasi_part3_syntax_diagram](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/lsbasi_part3_syntax_diagram.hch32rqxppk.png)

What is a syntax diagram? A syntax diagram is a graphical representation 
of a programming language’s syntax rules. Basically, a syntax diagram 
visually shows you which statements are allowed in your programming language 
and which are not.

什么是语法图?语法图是编程语言语法规则的图形表示。
基本上，语法图直观地显示了你的编程语言中哪些语句是允许的，哪些是不允许的。

Syntax diagrams are pretty easy to read: just follow the paths 
indicated by the arrows. Some paths indicate choices. And some 
paths indicate loops.

语法图是相当容易阅读的：只需按照箭头指示的路径。有些路径表示选择。而有些路径表示循环。

You can read the above syntax diagram as following: a term optionally 
followed by a plus or minus sign, followed by another term, which in 
turn is optionally followed by a plus or minus sign followed by another 
term and so on. You get the picture, literally. You might wonder what 
a “term” is. For the purpose of this article a “term” is just an integer.

你可以阅读上面的语法图：term 后面可选择加号或减号，后面还有一个 term。
后面还有一个加号或减号，然后是另一个术语，以此类推。你懂的，字面意思。
你可能在想什么是 term 。在这篇文章中 term 就是整数。

Syntax diagrams serve two main purposes:

语法图主要有两个作用。

* They graphically represent the specification 
(grammar) of a programming language.

它们以图形化的方式表示编程语言的规范（语法）。

* They can be used to help you write your parser 
- you can map a diagram to code by following simple rules.

它们可以用来帮助你编写你的解析器。您可以通过以下简单的规则将图表映射到代码中。

You’ve learned that the process of recognizing a phrase in 
the stream of tokens is called parsing. And the part of an 
interpreter or compiler that performs that job is called a 
parser. Parsing is also called syntax analysis, and the parser 
is also aptly called, you guessed it right, a syntax analyzer.

你已经学到了在 tokens 流中识别一个短语的过程，这个过程叫做解析。
在解释器或编译器中完成这项工程被称为 parser 。Parsing 也叫语法分析，
这个名字很贴切，你猜对了，就是语法分析器。

According to the syntax diagram above, all of the following 
arithmetic expressions are valid:

根据上面的语法图。以下所有的算术表达式都有效。

* 3
* 3 + 4
* 7 - 3 + 2 - 1

Because syntax rules for arithmetic expressions 
in different programming languages are very similar 
we can use a Python shell to “test” our syntax diagram. 
Launch your Python shell and see for yourself:

由于不同编程语言中算术表达式的语法规则非常相似，
我们可以使用Python shell来 "测试 "我们的语法图。
启动你的Python shell，自己看看吧。

```python
>>> 3
3
>>> 3 + 4
7
>>> 7 - 3 + 2 - 1
5
```

No surprises here.

这里没有惊喜。

The expression “3 + ” is not a valid arithmetic 
expression though because according to the syntax 
diagram the plus sign must be followed by a term (integer), 
otherwise it’s a syntax error. Again, try it with 
a Python shell and see for yourself:

不过表达式 "3+"不是一个有效的算术表达式，
因为根据语法图，加号后面必须有一个项（整数），否则就是语法错误。
同样，用Python shell试试，自己看看。

```python
>>> 3 +
  File "<stdin>", line 1
    3 +
      ^
SyntaxError: invalid syntax
```

It’s great to be able to use a Python shell to do 
some testing but let’s map the above syntax diagram 
to code and use our own interpreter for testing, all right?

能用Python shell来做一些测试是很好的，但是我们把上面的语法图映射成代码，
用我们自己的解释器来测试，好吗？

You know from the previous articles (Part 1 and Part 2) 
that the expr method is where both our parser and interpreter live. 
Again, the parser just recognizes the structure making sure that it 
corresponds to some specifications and the interpreter actually evaluates 
the expression once the parser has successfully recognized (parsed) it.

从前面的文章（第一部分和第二部分）中你知道，expr方法是我们的解析器和解释器的所在。
同样，解析器只是识别结构，确保它符合一些规范，而一旦解析器成功识别（解析）了表达式，解释器就会实际评估它。

The following code snippet shows the parser code corresponding to the diagram. 
The rectangular box from the syntax diagram (term) becomes a term method that parses an integer and the expr method just follows the syntax diagram flow:

下面的代码片段显示了与图对应的解析器代码。

```python
def term(self):
    self.eat(INTEGER)

def expr(self):
    # set current token to the first token taken from the input
    self.current_token = self.get_next_token()

    self.term()
    while self.current_token.type in (PLUS, MINUS):
        token = self.current_token
        if token.type == PLUS:
            self.eat(PLUS)
            self.term()
        elif token.type == MINUS:
            self.eat(MINUS)
            self.term()
```

You can see that expr first calls the term method. 
Then the expr method has a while loop which can 
execute zero or more times. And inside the loop 
the parser makes a choice based on the token 
(whether it’s a plus or minus sign). 
Spend some time proving to yourself that the 
code above does indeed follow the syntax diagram 
flow for arithmetic expressions.

你可以看到，expr首先调用术语方法。那么expr方法有一个while循环，可以执行零次或多次。
而在循环里面，解析器会根据token（是加号还是减号）做出选择。
花点时间证明上面的代码确实遵循了算术表达式的语法图流程。

The parser itself does not interpret anything though:
 if it recognizes an expression it’s silent and if it doesn’t, 
 it throws out a syntax error. Let’s modify the expr method and add the interpreter code:

不过解析器本身并没有解释任何东西。 
如果它识别到一个表达式，它就会沉默，如果没有，它就会抛出一个语法错误。
我们来修改expr方法，并添加解释器代码。

```python
def term(self):
    """Return an INTEGER token value"""
    token = self.current_token
    self.eat(INTEGER)
    return token.value

def expr(self):
    """Parser / Interpreter """
    # set current token to the first token taken from the input
    self.current_token = self.get_next_token()

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
```

Because the interpreter needs to evaluate an expression 
the term method was modified to return an integer value 
and the expr method was modified to perform addition and 
subtraction at the appropriate places and return the result 
of interpretation. Even though the code is pretty straightforward 
I recommend spending some time studying it.

因为解释器需要计算表达式，所以修改了 term 方法并返回一个整数值，也修改了 expr 方法
在适当的地方做加减法并通过返回解释器的计算结果。尽管代码的直接了当的，但是我建议你花一些时间来学习它。

Le’s get moving and see the complete code of the interpreter now, okay?

勒，我们现在就动起来，看看解释器的完整代码，好吗？

Here is the source code for your new version of the calculator that can 
handle valid arithmetic expressions containing integers and any number 
of addition and subtraction operators:

这里是新版计算器的源代码，它可以处理包含整数和任意数量加减运算符的有效算术表达式。

```python
# Token types
#
# EOF (end-of-file) token is used to indicate that
# there is no more input left for lexical analysis
INTEGER, PLUS, MINUS, EOF = 'INTEGER', 'PLUS', 'MINUS', 'EOF'


class Token(object):
    def __init__(self, type, value):
        # token type: INTEGER, PLUS, MINUS, or EOF
        self.type = type
        # token value: non-negative integer value, '+', '-', or None
        self.value = value

    def __str__(self):
        """String representation of the class instance.

        Examples:
            Token(INTEGER, 3)
            Token(PLUS, '+')
        """
        return 'Token({type}, {value})'.format(
            type=self.type,
            value=repr(self.value)
        )

    def __repr__(self):
        return self.__str__()


class Interpreter(object):
    def __init__(self, text):
        # client string input, e.g. "3 + 5", "12 - 5 + 3", etc
        self.text = text
        # self.pos is an index into self.text
        self.pos = 0
        # current token instance
        self.current_token = None
        self.current_char = self.text[self.pos]

    ##########################################################
    # Lexer code                                             #
    ##########################################################
    def error(self):
        raise Exception('Invalid syntax')

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

            self.error()

        return Token(EOF, None)

    ##########################################################
    # Parser / Interpreter code                              #
    ##########################################################
    def eat(self, token_type):
        # compare the current token type with the passed token
        # type and if they match then "eat" the current token
        # and assign the next token to the self.current_token,
        # otherwise raise an exception.
        if self.current_token.type == token_type:
            self.current_token = self.get_next_token()
        else:
            self.error()

    def term(self):
        """Return an INTEGER token value."""
        token = self.current_token
        self.eat(INTEGER)
        return token.value

    def expr(self):
        """Arithmetic expression parser / interpreter."""
        # set current token to the first token taken from the input
        self.current_token = self.get_next_token()

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
        interpreter = Interpreter(text)
        result = interpreter.expr()
        print(result)


if __name__ == '__main__':
    main()
```

Save the above code into the calc3.py file or download it directly from GitHub. 
Try it out. See for yourself that it can handle arithmetic expressions that 
you can derive from the syntax diagram I showed you earlier.

将以上代码保存到calc3.py文件中，或者直接从GitHub上下载。
试试吧。它可以处理算术表达式，你可以从我之前给你看的语法图中推导出来。

Here is a sample session that I ran on my laptop:

下面是我在笔记本电脑上运行的一个示例会话。

```python
$ python calc3.py
calc> 3
3
calc> 7 - 4
3
calc> 10 + 5
15
calc> 7 - 3 + 2 - 1
5
calc> 10 + 1 + 2 - 3 + 4 + 6 - 15
5
calc> 3 +
Traceback (most recent call last):
  File "calc3.py", line 147, in <module>
    main()
  File "calc3.py", line 142, in main
    result = interpreter.expr()
  File "calc3.py", line 123, in expr
    result = result + self.term()
  File "calc3.py", line 110, in term
    self.eat(INTEGER)
  File "calc3.py", line 105, in eat
    self.error()
  File "calc3.py", line 45, in error
    raise Exception('Invalid syntax')
Exception: Invalid syntax
```

Remember those exercises I mentioned at the beginning of the article: 
here they are, as promised :)

记得我在文章开头提到的那些练习：在这里，他们如约而至 :)

![lsbasi_part3_exercises](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/lsbasi_part3_exercises.64qujyh6r0c0.png)

* Draw a syntax diagram for arithmetic expressions that contain only multiplication and division, for example “7 * 4 / 2 * 3”. Seriously, just grab a pen or a pencil and try to draw one.

为只包含乘法和除法的算术表达式画一个语法图，例如 "7 * 4 / 2 * 3"。说真的，拿起笔或铅笔试着画一个吧。

* Modify the source code of the calculator to interpret arithmetic expressions that contain only multiplication and division, for example “7 * 4 / 2 * 3”.

修改计算器的源代码，解释只包含乘法和除法的算术表达式，例如 "7 * 4 / 2 * 3"。

* Write an interpreter that handles arithmetic expressions like “7 - 3 + 2 - 1” from scratch. Use any programming language you’re comfortable with and write it off the top of your head without looking at the examples. When you do that, think about components involved: a lexer that takes an input and converts it into a stream of tokens, a parser that feeds off the stream of the tokens provided by the lexer and tries to recognize a structure in that stream, and an interpreter that generates results after the parser has successfully parsed (recognized) a valid arithmetic expression. String those pieces together. Spend some time translating the knowledge you’ve acquired into a working interpreter for arithmetic expressions.

从头开始写一个处理 "7 - 3 + 2 - 1 "等算术表达式的解释器。使用任何你熟悉的编程语言，不看例子就随手写出来。当你这样做的时候，想想所涉及的组件：一个接受输入并将其转换为标记流的词法器，一个从词法器提供的标记流中获取信息并试图识别该流中的结构的解析器，以及一个在解析器成功解析（识别）一个有效的算术表达式后生成结果的解释器。将这些部件串联起来。花一些时间将你所学到的知识转化为算术表达式的工作解释器。

Check your understanding.

检查你的理解。

1. What is a syntax diagram?

 什么是语法图？

2. What is syntax analysis?

什么是语法分析？

3. What is a syntax analyzer?

什么是语法分析器？

Hey, look! You read all the way to the end. 
Thanks for hanging out here today and don’t forget to do the exercises. 
:) I’ll be back next time with a new article - stay tuned.

嘿，看！你一直读到最后。谢谢你今天来这里玩，别忘了做练习。:) 下次我会带着新的文章回来--敬请期待。

Here is a list of books I recommend that will help you in your study of interpreters and compilers:

下面是我推荐的书单，对你学习解释器和编译器会有所帮助。

1. [Language Implementation Patterns: Create Your Own Domain-Specific and General Programming Languages (Pragmatic Programmers)](https://www.amazon.com/gp/product/193435645X/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=193435645X&linkCode=as2&tag=russblo0b-20&linkId=MP4DCXDV6DJMEJBL)
2. [Writing Compilers and Interpreters: A Software Engineering Approach](https://www.amazon.com/gp/product/0470177071/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0470177071&linkCode=as2&tag=russblo0b-20&linkId=UCLGQTPIYSWYKRRM)
3. [Modern Compiler Implementation in Java](https://www.amazon.com/gp/product/052182060X/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=052182060X&linkCode=as2&tag=russblo0b-20&linkId=ZSKKZMV7YWR22NMW)
4. [Modern Compiler Design](https://www.amazon.com/gp/product/1461446988/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1461446988&linkCode=as2&tag=russblo0b-20&linkId=PAXWJP5WCPZ7RKRD)
5. [Compilers: Principles, Techniques, and Tools (2nd Edition)](https://www.amazon.com/gp/product/0321486811/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0321486811&linkCode=as2&tag=russblo0b-20&linkId=GOEGDQG4HIHU56FQ)