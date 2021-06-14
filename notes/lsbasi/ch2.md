# Part 2.

In their amazing book “The 5 Elements of Effective Thinking” the 
authors Burger and Starbird share a story about how they observed Tony Plog, 
an internationally acclaimed trumpet virtuoso, 
conduct a master class for accomplished trumpet players. 
The students first played complex music phrases, 
which they played perfectly well. 
But then they were asked to play very basic, simple notes. 
When they played the notes, 
the notes sounded childish compared to the previously played complex phrases. 
After they finished playing, the master teacher also played the same notes, 
but when he played them, they did not sound childish. 
The difference was stunning. 
Tony explained that mastering the performance of simple notes allows one to play
complex pieces with greater control. 
The lesson was clear - to build true virtuosity one must focus on mastering simple, 
basic ideas.[1](http://www.amazon.com/gp/product/0691156662/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0691156662&linkCode=as2&tag=russblo0b-20&linkId=B7GSVLONUPCIBIVY)

在这本非常好的书中《The 5 Elements of Effective Thinking》，
作者 Burger 和 Starbird 分享了一个关于如何观察 Tony Plog 的故事。
此人是在国际上是一个非常又名的小号演奏家，并为一些优秀的小号手开课。
这些学生可以完美的演奏复杂的音乐。但是它们却被要求演奏简单基本的音符。
这些音符和之前演奏的复杂音乐相比非常简单。当它们完成演奏后，师傅也弹起了相同的音符，
但是当他弹起这些音符之时，听起来并不幼稚。这个差别是惊人的。
Tony 解释说，对简单音符掌握的越好，那么就更容易演奏复杂的音乐。
这个教训是明显的，真才实学必须掌握简单的事情。[1](http://www.amazon.com/gp/product/0691156662/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0691156662&linkCode=as2&tag=russblo0b-20&linkId=B7GSVLONUPCIBIVY)

The lesson in the story clearly applies not only to music but also to software development. 
The story is a good reminder to all of us to not lose sight of the importance of deep work on simple,
basic ideas even if it sometimes feels like a step back. 
While it is important to be proficient with a tool or framework you use, 
it is also extremely important to know the principles behind them. 
As Ralph Waldo Emerson said:

从这个故事中得到的教训不仅仅应用于音乐方面，还可以体现在软件开发方面。
这个故事很好的提醒了我们，不要忽视在简单领域上深耕的重要性。
基本理念，即使有时感觉像退步。
然而对你而言精通一个使用的工具或框架是非常重要的。
了解它们背后的原理也是非常重要的。
就像 Ralph Waldo Emerson 所说：

> “If you learn only methods, 
> you’ll be tied to your methods. 
> But if you learn principles, you can devise your own methods.”

你如果学到的仅仅是方法，你将会被方法所束缚。但是如果你学到的是原理，那么你可以设计你的方法。

On that note, let’s dive into interpreters and compilers again.

在这点上，让我们来再次深入了解解释器和编译器。

Today I will show you a new version of the calculator from Part 1 that will be able to:

今天我将会根据第一部分来展示一个新版本的计算器，它将包含以下几部分：

1. Handle whitespace characters anywhere in the input string

可以根据如输入字符串处理任何地方的空格。

2. Consume multi-digit integers from the input

处理输入的多位整数。

3. Subtract two integers (currently it can only add integers)

整数减法（目前仅能处理整数加法）。

Here is the source code for your new version of the calculator that can do all of the above:

下面是满足以上要求新版本计算器的源代码。

```python
# Token types
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
        # client string input, e.g. "3 + 5", "12 - 5", etc
        self.text = text
        # self.pos is an index into self.text
        self.pos = 0
        # current token instance
        self.current_token = None
        self.current_char = self.text[self.pos]

    def error(self):
        raise Exception('Error parsing input')

    def advance(self):
        """Advance the 'pos' pointer and set the 'current_char' variable."""
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
        apart into tokens.
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

    def eat(self, token_type):
        # compare the current token type with the passed token
        # type and if they match then "eat" the current token
        # and assign the next token to the self.current_token,
        # otherwise raise an exception.
        if self.current_token.type == token_type:
            self.current_token = self.get_next_token()
        else:
            self.error()

    def expr(self):
        """Parser / Interpreter

        expr -> INTEGER PLUS INTEGER
        expr -> INTEGER MINUS INTEGER
        """
        # set current token to the first token taken from the input
        self.current_token = self.get_next_token()

        # we expect the current token to be an integer
        left = self.current_token
        self.eat(INTEGER)

        # we expect the current token to be either a '+' or '-'
        op = self.current_token
        if op.type == PLUS:
            self.eat(PLUS)
        else:
            self.eat(MINUS)

        # we expect the current token to be an integer
        right = self.current_token
        self.eat(INTEGER)
        # after the above call the self.current_token is set to
        # EOF token

        # at this point either the INTEGER PLUS INTEGER or
        # the INTEGER MINUS INTEGER sequence of tokens
        # has been successfully found and the method can just
        # return the result of adding or subtracting two integers,
        # thus effectively interpreting client input
        if op.type == PLUS:
            result = left.value + right.value
        else:
            result = left.value - right.value
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

Save the above code into the calc2.py file or download it directly from GitHub. 
Try it out. See for yourself that it works as expected: 
it can handle whitespace characters anywhere in the input; 
it can accept multi-digit integers, 
and it can also subtract two integers as well as add two integers.

将以上的代码保存至 `calc2.py` 文件中，或者直接从 Github 中下载。
尝试一下，看看是否想期待的那样工作。它可以处理从任何地方输入的字符。可以接受多位整数。
可以像两个整数做加法操作一样做减法操作。

Here is a sample session that I ran on my laptop:

下面是在我的笔记本上允许的一个例子。

```python
$ python calc2.py
calc> 27 + 3
30
calc> 27 - 7
20
calc>
```

The major code changes compared with the version from Part 1 are:

和 Part 1 版本相比的主要代码变化如下：

1. The get_next_token method was refactored a bit. 
The logic to increment the pos pointer was factored into a separate method advance.

get_next_token 方法重构了。递增指针（pos）被单独的写入了一个方法之中。

2. Two more methods were added: 
skip_whitespace to ignore whitespace characters and 
integer to handle multi-digit integers in the input.

增加了两个方法：skip_whitespace 是跳过空格和处理输入的多位整数。

3. The expr method was modified to recognize INTEGER -> MINUS -> INTEGER 
phrase in addition to INTEGER -> PLUS -> INTEGER phrase. 
The method now also interprets both addition and subtraction after having 
successfully recognized the corresponding phrase.

expr 方法被修改

In Part 1 you learned two important concepts, namely that of a token and 
a lexical analyzer. Today I would like to talk a little bit about lexemes, 
parsing, and parsers.

在 Part 1 中你学到了两个重要概念，token 和词法分析。今天我将会介绍一些关于 lexemes，parsing 和 parsers。

You already know about tokens. But in order for me to round out the discussion 
of tokens I need to mention lexemes. What is a lexeme? A lexeme is a sequence 
of characters that form a token. In the following picture you can see some 
examples of tokens and sample lexemes and hopefully it will make the relationship 
between them clear:

你已经知道了 tokens 。但是为了使得讨论完善，我需要提到 lexemes 。什么是 lexeme ？
lexeme 是由字符组成的 token 序列。下面的图片中你可以看到一些 token 和 lexemes 样本的例子。
这能够清晰的展示它们之间的关系。 

![lsbasi_part2_lexemes](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/lsbasi_part2_lexemes.6cuzowb60ko0.png)

Now, remember our friend, the expr method? I said before that that’s where 
the interpretation of an arithmetic expression actually happens. But before 
you can interpret an expression you first need to recognize what kind of phrase 
it is, whether it is addition or subtraction, for example. That’s what the 
expr method essentially does: it finds the structure in the stream of tokens 
it gets from the get_next_token method and then it interprets the phrase that 
is has recognized, generating the result of the arithmetic expression.

现在，朋友还记得 expr 方法吗？我之前说过，这就是解释算术表达式的实际流程。
但在解释一个表达方式之前，你首先要认识到这是一个什么样的短语，比如说是加法还是减法。
这就是 expr 方法的本质作用：它在从 get_next_token 方法得到的 token 流中找到结构，
然后对已识别的短语进行解释，生成算术表达式的结果。

The process of finding the structure in the stream of tokens, or put differently, 
the process of recognizing a phrase in the stream of tokens is called parsing. 
The part of an interpreter or compiler that performs that job is called a parser.

在 tokens 流中寻找结构的过程，或者换句话说，在标记流中识别一个短语的过程称为解析。
解释器或编译器中执行这项工作的部分称为解析器。

So now you know that the expr method is the part of your interpreter where both 
parsing and interpreting happens - the expr method first tries to recognize (parse) 
the INTEGER -> PLUS -> INTEGER or the INTEGER -> MINUS -> INTEGER phrase in the 
stream of tokens and after it has successfully recognized (parsed) one of those phrases, 
the method interprets it and returns the result of either addition or subtraction of 
two integers to the caller.

所以现在你知道 expr 方法是你的解释器中解析和解释的部分 --expr 方法首先尝试识别（解析）
标记流中的 INTEGER -> PLUS -> INTEGER 或 INTEGER -> MINUS -> INTEGER 短语，
在成功识别（解析）其中一个短语后，该方法对其进行解释，
并将两个整数的加法或减法结果返回给调用者。

And now it’s time for exercises again.

现在又到了练习的时间。

1. Extend the calculator to handle multiplication of two integers

扩展计算器以处理两个整数的乘法。

2. Extend the calculator to handle division of two integers

扩展计算器以处理两个整数的除法。

3. Modify the code to interpret expressions containing an arbitrary number of additions and subtractions, for example “9 - 5 + 3 + 11”

修改代码以解释包含任意数量加减法的表达式，例如 "9-5+3+11"

Check your understanding.

检查你的理解。

1. What is a lexeme?

什么是  lexeme ？

2. What is the name of the process that finds the structure in the stream of tokens, 
or put differently, what is the name of the process that recognizes a certain phrase 
in that stream of tokens?

从 tokens 流中寻找结构的程序叫什么名字，换句话说，从 tokens 流中理解一个具体单词的程序叫什么名字。

3. What is the name of the part of the interpreter (compiler) that does parsing?

解释器（编译器）中进行解析部分叫什么名字？

I hope you liked today’s material. In the next article of the series you will extend your calculator to handle more complex arithmetic expressions. Stay tuned.

我希望你喜欢今天的材料，在系列的下一篇文章中，你将扩展你的计算器，处理更复杂的算术表达式。
在本系列的下一篇文章中，你将扩展你的计算器来处理更复杂的算术表达式。敬请期待。

And here is a list of books I recommend that will help you in your study of interpreters and compilers:

下面是我推荐的书单，对你学习解释器和编译器有帮助。

1. [Language Implementation Patterns: Create Your Own Domain-Specific and General Programming Languages (Pragmatic Programmers)](https://www.amazon.com/gp/product/193435645X/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=193435645X&linkCode=as2&tag=russblo0b-20&linkId=MP4DCXDV6DJMEJBL)
2. [Writing Compilers and Interpreters: A Software Engineering Approach](https://www.amazon.com/gp/product/0470177071/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0470177071&linkCode=as2&tag=russblo0b-20&linkId=UCLGQTPIYSWYKRRM)
3. [Modern Compiler Implementation in Java](https://www.amazon.com/gp/product/052182060X/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=052182060X&linkCode=as2&tag=russblo0b-20&linkId=ZSKKZMV7YWR22NMW)
4. [Modern Compiler Design](https://www.amazon.com/gp/product/1461446988/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1461446988&linkCode=as2&tag=russblo0b-20&linkId=PAXWJP5WCPZ7RKRD)
5. [Compilers: Principles, Techniques, and Tools (2nd Edition)](https://www.amazon.com/gp/product/0321486811/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0321486811&linkCode=as2&tag=russblo0b-20&linkId=GOEGDQG4HIHU56FQ)