# Part 1.

> "If you don’t know how compilers work, then you don’t know how computers work. If you’re not 100% sure whether you know how compilers work, then you don’t know how they work." — Steve Yegge

”如果你不理解编译器如何工作，那么你就不知道计算机如何工作。如果你不能 100% 确定你是否知道编译器如何工作，那么你就不知道它们是如何工作的。“ — Steve Yegge

There you have it. Think about it. It doesn’t really matter whether you’re a newbie or a seasoned software developer: if you don’t know how compilers and interpreters work, then you don’t know how computers work. It’s that simple.

你明白这个道理后，仔细思考。无论你是个新手或者是个老练的软件开发者者都是不重要的：如果你不知道编译器和解释器是如何工作的，那么你就不知道计算机是如何工作的。就是这么简单。

So, do you know how compilers and interpreters work? And I mean, are you 100% sure that you know how they work? If you don’t.

所以，你知道编译器和解释器是如何工作的吗？我的意思是，你 100% 确定你知道它们是如何工作的？如果你不知道。

![lsbasi_part1_i_dont_know](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/lsbasi_part1_i_dont_know.3nino5ffhvq0.png)

Or if you don’t and you’re really agitated about it.

或者你不知道，或者对于此你很烦躁。

![lsbasi_part1_omg](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/lsbasi_part1_omg.uch2bc2id9c.png)

Do not worry. If you stick around and work through the series and build an interpreter and a compiler with me you will know how they work in the end. And you will become a confident happy camper too. At least I hope so.

不要担心。如果你能坚持下来和我一起学习这个系列，最终构建出一个解释器，知道它们最终是如何工作的。你将会变的自信幸福。至少我希望如此。

![lsbasi_part1_i_know](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/lsbasi_part1_i_know.63dwu3zw61w0.png)

Why would you study interpreters and compilers? I will give you three reasons.

为什么你学习解释器和编译器？我认为有三个理由。

1. To write an interpreter or a compiler you have to have a lot of technical skills that you need to use together. Writing an interpreter or a compiler will help you improve those skills and become a better software developer. As well, the skills you will learn are useful in writing any software, not just interpreters or compilers.

写一个解释器或者编译器你需要很多的技术能力，这些能力需要你一起使用。写一个解释器或者编译器可以帮助你提高这些技能并且成为一个更好的软件开发者。此外，你学到的这些技能再写任何软件时是非常有用的，不仅仅是在解释器和编译器上。

2. You really want to know how computers work. Often interpreters and compilers look like magic. And you shouldn’t be comfortable with that magic. You want to demystify the process of building an interpreter and a compiler, understand how they work, and get in control of things.

你真的想知道计算机是如何工作的。通常解释器和编译器看起来像魔法。并且你对于这种魔法感到不满足。你想要通过构建解释器和编译器来解惑，理解它们是如何工作并控制这些事情。

3. You want to create your own programming language or domain specific language. If you create one, you will also need to create either an interpreter or a compiler for it. Recently, there has been a resurgence of interest in new programming languages. And you can see a new programming language pop up almost every day: Elixir, Go, Rust just to name a few.

你想要创建自己的变成语言或者特定领域的语言。如果你创建了一个，对于该语言你还需要一个解释器或编译器。最近，人们对新的变成语言兴趣再度高涨。而且你几乎每天都能看到一种新的编程语言出现。Elixir, Go, Rust等等。

Okay, but what are interpreters and compilers?

但什么是解释器和编译器？

The goal of an interpreter or a compiler is to translate a source program in some high-level language into some other form. Pretty vague, isn’t it? Just bear with me, later in the series you will learn exactly what the source program is translated into.

一个解释器或者编译器的目标是翻译某个高级语言的源程序为其他形式。这很模糊，不是吗？先忍一忍，后续你将会确切的理解源程序是如何被翻译的。

At this point you may also wonder what the difference is between an interpreter and a compiler. For the purpose of this series, let’s agree that if a translator translates a source program into machine language, it is a compiler. If a translator processes and executes the source program without translating it into machine language first, it is an interpreter. Visually it looks something like this:

到目前为止，你可能会好奇解释器和编译器有什么不同。在本系列中，我们编译器定义为源代码翻译为机器码。解释器定义为执行一个翻译程序的源码而不需要先将其翻译为机器码。看起来像下面这样。

![lsbasi_part1_compiler_interpreter](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/lsbasi_part1_compiler_interpreter.3lejm4gaa6w0.png)

I hope that by now you’re convinced that you really want to study and build an interpreter and a compiler. What can you expect from this series on interpreters?

我希望现在相信自己真的想要学习并构建一个解释器或者编译器。除此之外你还期待什么？

Here is the deal. You and I are going to create a simple interpreter for a large subset of Pascal language. At the end of this series you will have a working Pascal interpreter and a source-level debugger like Python’s pdb.

我们将会创建一个简单的 Pascal 语言子集的解释器。最终，在这个系列中你将会实现一个源码级别的调试器，类似于 python 的 pdb 。

You might ask, why Pascal? For one thing, it’s not a made-up language that I came up with just for this series: it’s a real programming language that has many important language constructs. And some old, but useful, CS books use Pascal programming language in their examples (I understand that that’s not a particularly compelling reason to choose a language to build an interpreter for, but I thought it would be nice for a change to learn a non-mainstream language :)

你可能会问，为什么是 Pascal？一来，这不是我为这个系列而虚构出来的语言，这是真实存在的语言，拥有许多重要语言结构。除此之外，它是非常有用的，一些 CS 书籍在例子中使用 Pascal 编程语言。（我知道选择这个语言来构建一个解释器比较勉强，但是我想学习一门非主流的语言是一个有趣的改变。：）

Here is an example of a factorial function in Pascal that you will be able to interpret with your own interpreter and debug with the interactive source-level debugger that you will create along the way:

下面是使用 Pascal 写的斐波那契函数的例子，你将会使用自己的解释器来解释该函数并交互式的使用源码级别的调试器来调试。

```pascal
program factorial;

function factorial(n: integer): longint;
begin
    if n = 0 then
        factorial := 1
    else
        factorial := n * factorial(n - 1);
end;

var
    n: integer;

begin
    for n := 0 to 16 do
        writeln(n, '! = ', factorial(n));
end.
```

The implementation language of the Pascal interpreter will be Python, but you can use any language you want because the ideas presented don’t depend on any particular implementation language. Okay, let’s get down to business. Ready, set, go!

Pascal 解释器的语言实现将会采用 Python，你可以使用任何你想使用的语言，因为想法的实现并不依赖于特定语言。好了，言归正传，准备开始吧！

You will start your first foray into interpreters and compilers by writing a simple interpreter of arithmetic expressions, also known as a calculator. Today the goal is pretty minimalistic: to make your calculator handle the addition of two single digit integers like `3+5`. Here is the source code for your calculator, sorry, interpreter:

你将开始解释器和编译器的第一次尝试，通过写一个简单的算术表达式，也称为计算器。今天的目标是非常简约的：使你的计算器处理类似于 `3+5` 一样的两个整数。下面是解释器的源码：

```python
# Token 类型
#
# EOF (end-of-file) 输入结束的标志
INTEGER, PLUS, EOF = 'INTEGER', 'PLUS', 'EOF'


class Token(object):
    def __init__(self, type, value):
        # token 类型: INTEGER, PLUS, or EOF
        self.type = type
        # token 值: 0, 1, 2. 3, 4, 5, 6, 7, 8, 9, '+', or None
        self.value = value

    def __str__(self):
        """可视化 Token

        Examples:
            Token(INTEGER, 3)
            Token(PLUS '+')
        """
        return 'Token({type}, {value})'.format(
            type=self.type,
            value=repr(self.value)
        )

    def __repr__(self):
        return self.__str__()


class Interpreter(object):
    def __init__(self, text):
        # 要解释的内容，源代码，例如："3+5"
        self.text = text
        # self.pos 表示指向当前字符的位置。
        self.pos = 0
        # 当前的 Token
        self.current_token = None

    def error(self):
        raise Exception('Error parsing input')

    def get_next_token(self):
        """词法分析器（也叫扫描器或者标记器）

        这个方法负责将句子拆分成 Token
        一次一个 Token 。
        """
        text = self.text

        # 如果 self.pos 索引到了 self.text 的末尾
        # 返回 EOF 表示输入字符均被转换成了 Tokens
        if self.pos > len(text) - 1:
            return Token(EOF, None)

        # 获取当前位置的字符串，基于当前的单个字符来创建 Token
        current_char = text[self.pos]

        # 如果当前字符是数字，那么将其转换为整数，创建 INTEGER token
        # 然后 self.pos 索引自增，指向下一个字符，并返回创建好的 token。
        if current_char.isdigit():
            token = Token(INTEGER, int(current_char))
            self.pos += 1
            return token

        if current_char == '+':
            token = Token(PLUS, current_char)
            self.pos += 1
            return token

        self.error()

    def eat(self, token_type):
        # 比较当前的 token 类型和已经扫描过的 token 类型，
        # 如果二者类型一致，那么 "eat" 当前的 token 。
        # 并下一个 token 赋给当前 token 。
        # 否则返回 token 。
        if self.current_token.type == token_type:
            self.current_token = self.get_next_token()
        else:
            self.error()

    def expr(self):
        """expr -> INTEGER PLUS INTEGER"""
        # 将输入的第一个 token 设置为当前的 token 。
        self.current_token = self.get_next_token()

        # 当前的 token 是一个一位数的整数。
        left = self.current_token
        self.eat(INTEGER)

        # 当前的 token 是一个 “+” 号。
        op = self.current_token
        self.eat(PLUS)

        # 当前的 token 是一个一位数的整数。
        right = self.current_token
        self.eat(INTEGER)
        # 上述内容被调用后，
        # self.current_token 被设置为 EOF token

        # 此时，整数加整数的 token 序列已经成功的被识别到，
        # 该方法可以成功返回两个整数相加后的结果，
        # 从而有效的解释输入。
        result = left.value + right.value
        return result


def main():
    while True:
        try:
            # 如果在 Python3 下运行，
            # 需要将 'raw_input' 替换为 'input'
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

Save the above code into calc1.py file or download it directly from [GitHub](https://github.com/rspivak/lsbasi/blob/master/part1/calc1.py). Before you start digging deeper into the code, run the calculator on the command line and see it in action. Play with it! Here is a sample session on my laptop (if you want to run the calculator under Python3 you will need to replace raw_input with input):

将以上代码保存至 calc1.py 文件中，或者直接从 Github 中下载。在你开始研究代码之前，先在命令行中运行查看结果。玩吧！下面是一些样例。（如果想在 Python3 中运行代码，你需要将 raw_input 修改为 input）。

```python
$ python calc1.py
calc> 3+4
7
calc> 3+5
8
calc> 3+9
12
calc>
```

For your simple calculator to work properly without throwing an exception, your input needs to follow certain rules:

为了让你的简易计算器正确的运行且不抛出异常，你需的输入需要按照如下的规则：

* Only single digit integers are allowed in the input

输入只有一个整数。

* The only arithmetic operation supported at the moment is addition

目前唯一支持的运算符是加号 “+” 。

* No whitespace characters are allowed anywhere in the input

输入不能有空格。

Those restrictions are necessary to make the calculator simple. Don’t worry, you’ll make it pretty complex pretty soon.

为了使得计算器边简单，这些措施是必要的。不用担心，这个很快就会复杂起来。

Okay, now let’s dive in and see how your interpreter works and how it evaluates arithmetic expressions.

好了，让我们开始切分并查看解释器是如何工作的，如何计算算术表达式。

When you enter an expression `3+5` on the command line your interpreter gets a string “3+5”. In order for the interpreter to actually understand what to do with that string it first needs to break the input “3+5” into components called tokens. A token is an object that has a type and a value. For example, for the string “3” the type of the token will be INTEGER and the corresponding value will be integer 3.

当你在命令行中输入一个 `3+5` 的表达式时，你的解释器将会得到一个字符串 “3+5” 。为了让解释器真实的理解字符串该如何处理，首先需要将 “3+5” 分解成 tokens 。token 是一个有 type 和 value 的对象。例如，字符 “3” 的 token 类型是 INTEGER，与之相应的 value 则是整数 3 。

The process of breaking the input string into tokens is called lexical analysis. So, the first step your interpreter needs to do is read the input of characters and convert it into a stream of tokens. The part of the interpreter that does it is called a lexical analyzer, or lexer for short. You might also encounter other names for the same component, like scanner or tokenizer. They all mean the same: the part of your interpreter or compiler that turns the input of characters into a stream of tokens.

将字符串分解成 token 的处理过程称为词法分析。所以，解释器的第一步需要将读取到输入字符串转换为 tokens 。解释器的这部分叫做词法分析，简称 lexer 。你可能遇到具备类似功能的不同名字，例如 scanner 或者 tokenizer 。这些内容的意思是相同的，你的解释器或者编译器的一部分将输入的字符转换为 tokens 流。

The method get_next_token of the Interpreter class is your lexical analyzer. Every time you call it, you get the next token created from the input of characters passed to the interpreter. Let’s take a closer look at the method itself and see how it actually does its job of converting characters into tokens. The input is stored in the variable text that holds the input string and pos is an index into that string (think of the string as an array of characters). pos is initially set to 0 and points to the character ‘3’. The method first checks whether the character is a digit and if so, it increments pos and returns a token instance with the type INTEGER and the value set to the integer value of the string ‘3’, which is an integer 3:

解释器类的 get_next_token 方法是你的词法分析。每次你调用它时，你将会从输入到解释器的输入字符中得到下一个被创建的 token 。接下来仔细查看这个方法本身并且理解如何将字符转换为 tokens 。输入被存放在变量文本中，pos 是该字符串的索引。（将字符串想象成一个字符数组）。pos 被初始化为 0 指向字符串 ’3‘。这个方法首先检查字符是否是数字，如果是那么 pos 自增并且返回当i去哪 token 的实现，type 是 “INTEGER” value 被设置为字符串 ’3‘。

![lsbasi_part1_lexer1](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/lsbasi_part1_lexer1.4lr89zjbskq0.png)

The pos now points to the ‘+’ character in the text. The next time you call the method, it tests if a character at the position pos is a digit and then it tests if the character is a plus sign, which it is. As a result the method increments pos and returns a newly created token with the type PLUS and value ‘+’:

现在文本的索引 pos 指向了 ’+‘ 。下次你调用这个方法时，它会检测当前位置的字符是整数然后再检测字符是否是加法符号。这个方法的结果是 pos 增加并返回一个新创建的 token，类型为 PLUS 值为 “+”：

![lsbasi_part1_lexer2](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/lsbasi_part1_lexer2.1vkgxy92xgkg.png)

The pos now points to character ‘5’. When you call the get_next_token method again the method checks if it’s a digit, which it is, so it increments pos and returns a new INTEGER token with the value of the token set to integer 5:

pos 当前指向的位置是 ’5‘。当你开始调用 get_next_token 方法时会再次调用方法检查该值是否是一个整数。pos 增加并返回一个 token，该 token 的 type 是 INTEGER，value 是 5 。

![lsbasi_part1_lexer3](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/lsbasi_part1_lexer3.31s9g9d41bi0.png)

Because the pos index is now past the end of the string “3+5” the get_next_token method returns the EOF token every time you call it:

因为 pos 索引现在已经过了字符串 "3+5 "的结尾，所以后续再调用 get_next_token 方法都会返回 EOF 标记。

![lsbasi_part1_lexer4](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/lsbasi_part1_lexer4.29kpdfh7xk7w.png)

Try it out and see for yourself how the lexer component of your calculator works:

尝试并查看 lexer 组件在计算机中是如何工作的。

```python
>>> from calc1 import Interpreter
>>>
>>> interpreter = Interpreter('3+5')
>>> interpreter.get_next_token()
Token(INTEGER, 3)
>>>
>>> interpreter.get_next_token()
Token(PLUS, '+')
>>>
>>> interpreter.get_next_token()
Token(INTEGER, 5)
>>>
>>> interpreter.get_next_token()
Token(EOF, None)
>>>
```

So now that your interpreter has access to the stream of tokens made from the input characters, the interpreter needs to do something with it: it needs to find the structure in the flat stream of tokens it gets from the lexer get_next_token. Your interpreter expects to find the following structure in that stream: INTEGER -> PLUS -> INTEGER. That is, it tries to find a sequence of tokens: integer followed by a plus sign followed by an integer.

所以，目前为止，你的解释器可以访问由输入字符所组成的 tokens 序列。解释器需要对它做一些事情：它需要根据 lexer get_next_token 从 tokens 流中找到结构。解释器希望在该流中找到以下结构。INTEGER -> PLUS -> INTEGER. 也就是说，它试图找到一个 token 序列：整数后面跟着一个加号，然后是一个整数。

The method responsible for finding and interpreting that structure is expr. This method verifies that the sequence of tokens does indeed correspond to the expected sequence of tokens, i.e INTEGER -> PLUS -> INTEGER. After it’s successfully confirmed the structure, it generates the result by adding the value of the token on the left side of the PLUS and the right side of the PLUS, thus successfully interpreting the arithmetic expression you passed to the interpreter.

负责寻址和解释该结构的方法是 expr。该方法验证了 tokens 序列，这个方法验证了 token 的序列确实符合预期的 token 序列，即INTEGER -> PLUS -> INTEGER。之后它可以成功的验证该结构，通过累加左边 token 和右边 token 的值来生成结果，因为成功的解释

The expr method itself uses the helper method eat to verify that the token type passed to the eat method matches the current token type. After matching the passed token type the eat method gets the next token and assigns it to the current_token variable, thus effectively “eating” the currently matched token and advancing the imaginary pointer in the stream of tokens. If the structure in the stream of tokens doesn’t correspond to the expected INTEGER PLUS INTEGER sequence of tokens the eat method throws an exception.

expr方法本身使用helper方法eat来验证传递给eat方法的token类型是否与当前的token类型匹配。在匹配了传递的令牌类型后，eat方法获取下一个令牌，并将其赋值给current_token变量，从而有效地 "吃掉 "当前匹配的令牌，并推进令牌流中的虚指针。如果令牌流中的结构与预期的INTEGER PLUS INTEGER序列的令牌不一致，eat方法就会抛出一个异常。

Let’s recap what your interpreter does to evaluate an arithmetic expression:

让我们来会议一下，你的解释器在计算算术表达式的流程。

* The interpreter accepts an input string, let’s say “3+5”

解释器结构一个字符串的输入，“3+5” 。

* The interpreter calls the expr method to find a structure in the stream of tokens returned by the lexical analyzer get_next_token. The structure it tries to find is of the form INTEGER PLUS INTEGER. After it’s confirmed the structure, it interprets the input by adding the values of two INTEGER tokens because it’s clear to the interpreter at that point that what it needs to do is add two integers, 3 and 5.

解释器调用expr方法，在词法分析器 get_next_token 返回的 tokens 流中找到一个结构。它试图找到的结构形式是INTEGER PLUS INTEGER。在它确认了这个结构之后，它通过添加两个INTEGER token 值来解释输入的结构，因为此时解释器很清楚，它需要做的是添加两个整数，3 和 5。

Congratulate yourself. You’ve just learned how to build your very first interpreter!

祝贺你，你已经完成了你的第一个解释器。

Now it’s time for exercises.

是时候做一些练习了。

![lsbasi_exercises2](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/lsbasi_exercises2.1zbrd742oc8w.png)

You didn’t think you would just read this article and that would be enough, did you? Okay, get your hands dirty and do the following exercises:

你不会认为仅仅阅读这篇文章就足够掌握了吧？你需要动手实践一下下面的练习：

1. Modify the code to allow multiple-digit integers in the input, for example “12+3”

修改代码，允许多位整数输入，例如 “12+3”。

2. Add a method that skips whitespace characters so that your calculator can handle inputs with whitespace characters like ”12 + 3”

增加可以跳过空格字符的函数，你的计算可以处理输入中包含空格的情况，例如“12 + 3”

3. Modify the code and instead of ‘+’ handle ‘-‘ to evaluate subtractions like “7-5”

修改代码，使其支持减法运算，例如 “7-5”。

Check your understanding

检查你的理解

1. What is an interpreter?

什么是解释器？

2. What is a compiler?

什么是编译器？

3. What’s the difference between an interpreter and a compiler?

解释器和编译器有什么不同？

4. What is a token?

什么是 token ？


5. What is the name of the process that breaks input apart into tokens?

将输入分解成 tokens 的流程被称为什么？

6. What is the part of the interpreter that does lexical analysis called?

解释器做词法分析的部分叫做什么？

7. What are the other common names for that part of an interpreter or a compiler?

在解释器和编译器中，对于词法分析部分还有哪些常用的名字。

Before I finish this article, I really want you to commit to studying interpreters and compilers. And I want you to do it right now. Don’t put it on the back burner. Don’t wait. If you’ve skimmed the article, start over. If you’ve read it carefully but haven’t done exercises - do them now. If you’ve done only some of them, finish the rest. You get the idea. And you know what? Sign the commitment pledge to start learning about interpreters and compilers today! 

读完本文之前，我真的希望你可以努力的学习解释器和编译器。希望你现在就去做。不要把它放在后面。不要等。如果你略读这篇文章，重新开始看。如果你仔细看但还没有开始练习，现在开始练习。如果你只是完成了一部分，那么把剩下的完成。你理解了思想，但是你知道为什么吗？签署承诺书吧，今天就开始学习解释器和编译器! 

> I, ________, of being sound mind and body, do hereby pledge to commit to studying interpreters and compilers starting today and get to a point where I know 100% how they work!
> Signature:
> Date:


> 本人, ________, 作为一个身心健全的人， 在此承诺，从今天开始，我将致力于研究解释器和编译器，并达到100%知道它们是如何工作的程度。
> 签字：
> 日期：


![lsbasi_part1_commitment_pledge](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/lsbasi_part1_commitment_pledge.29wwwzm43y3o.png)

Sign it, date it, and put it somewhere where you can see it every day to make sure that you stick to your commitment. And keep in mind the definition of commitment:

签署它，日期，并把它放在某个地方，你可以看到它的每一天，以确保你坚持你的承诺。并牢记承诺的定义。

> “Commitment is doing the thing you said you were going to do long after the mood you said it in has left you.” — Darren Hardy

Okay, that’s it for today. In the next article of the mini series you will extend your calculator to handle more arithmetic expressions. Stay tuned.

好了，今天就到这里。在下一篇迷你系列的文章中，你将扩展你的计算器来处理更多的算术表达式。敬请期待。

If you can’t wait for the second article and are chomping at the bit to start digging deeper into interpreters and compilers, here is a list of books I recommend that will help you along the way:

如果你等不及第二篇文章，正在咬牙切齿地开始深入研究解释器和编译器，这里有一份我推荐的书单，会对你的学习有所帮助。

1. [Language Implementation Patterns: Create Your Own Domain-Specific and General Programming Languages (Pragmatic Programmers)](http://www.amazon.com/gp/product/193435645X/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=193435645X&linkCode=as2&tag=russblo0b-20&linkId=MP4DCXDV6DJMEJBL)
2. [Writing Compilers and Interpreters: A Software Engineering Approach](https://www.amazon.com/gp/product/0470177071/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0470177071&linkCode=as2&tag=russblo0b-20&linkId=UCLGQTPIYSWYKRRM)
3. [Modern Compiler Implementation in Java](https://www.amazon.com/gp/product/052182060X/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=052182060X&linkCode=as2&tag=russblo0b-20&linkId=ZSKKZMV7YWR22NMW)
4. [Modern Compiler Design](https://www.amazon.com/gp/product/1461446988/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1461446988&linkCode=as2&tag=russblo0b-20&linkId=PAXWJP5WCPZ7RKRD)
5. [Compilers: Principles, Techniques, and Tools (2nd Edition)](https://www.amazon.com/gp/product/0321486811/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0321486811&linkCode=as2&tag=russblo0b-20&linkId=GOEGDQG4HIHU56FQ)


> 译者注：对于初学者来说略过空格和减法是很简单的，多位整数的支持有难度！想不出来就直接看下一章吧。