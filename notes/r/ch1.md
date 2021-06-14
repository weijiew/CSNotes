
# 基础语法

## 命令行

* 直接键入表达式可以得到结果。

```r
> 3 + 4
[1] 7
```

## 运算符

共有 6 种运算符，加（+）减（-）乘（*）幂（^）取余（%%）。

## 给变量赋值

使用 <- 操作符给变量 x 赋值，<- 左边是变量，右边是待赋的值。

```r
# Assign the value 42 to x
x <- 42

# Print out the value of the variable x
x
```

赋值练习

```r
# Assign the value 5 to the variable my_apples
my_apples <- 5

# Print out the value of the variable my_apples
my_apples
```

## 运算符和赋值的结合

```r
# Assign a value to the variables my_apples and my_oranges
my_apples <- 5
my_oranges <- 6

# Add these two variables together
my_apples + my_oranges

# Create the variable my_fruit
my_fruit <- my_apples + my_oranges
```

注意变量类型要一致，如果不一致会导致报错。

```r
# Assign a value to the variable my_apples
my_apples <- 5 

# Fix the assignment of my_oranges
my_oranges <- "six"  

# Create the variable my_fruit and print it out
my_fruit <- my_apples + my_oranges 
my_fruit
```

修改为：

```r
# Assign a value to the variable my_apples
my_apples <- 5 

# Fix the assignment of my_oranges
my_oranges <- 6 

# Create the variable my_fruit and print it out
my_fruit <- my_apples + my_oranges 
my_fruit
```

## 查看变量类型

通过函数 class() 可以查看变量类型。

```r
# Declare variables of different types
my_numeric <- 42
my_character <- "universe"
my_logical <- FALSE 

# Check class of my_numeric
class(my_numeric)

# Check class of my_character
class(my_character)

# Check class of my_logical
class(my_logical)
```

# 小提琴图

小提琴图允许可视化一个或多个数字变量的分布。

每一个小提琴图代表一个或一组变量。

形状代表变量的密度估计：特定范围内的点越多代表该范围内的音域大。

它和箱线图很像，但是更能深入的理解分布。

小提琴图依靠 ggplot2 中的 geom_violin() 函数实现。


```r
# 加载包
library(ggplot2)
# 创建数据表
data <- data.frame(
  name=c( rep("A",500), rep("B",500), rep("B",500), rep("C",20), rep('D', 100)  ),
  value=c( rnorm(500, 10, 5), rnorm(500, 13, 1), rnorm(500, 18, 1), rnorm(20, 25, 4), rnorm(100, 12, 1) )
)
# 绑定并显示数据 
p <- ggplot(data, aes(x=name, y=value, fill=name)) + geom_violin()
# 显示图片
p
```

大致思路是：首先加载 ggplot2 这个包。然后创建数据表格。最后采用 ggplot2 对数据表格进行绑定。

x 轴绑定的是字符 (A,B...), y 轴绑定的是生成的正太分布数字。

API: 

* rep() 函数全称是 reputation 也就是重复的意思 rep("A",500) 就是将字符 A 重复 500 次,后续同理。
* rnorm(n,mean,std) 函数表示生成正太分布, n 表示数据个数, mean 表示均值, std 则表示方差。
* c() 函数的全称是 connect 也就是来连接的意思. 例如下面的例子 :

```r
> c("patient","age","weight","bp", "rating","test")
[1] "patient" "age"     "weight"  "bp"      "rating" 
[6] "test"  
```

* data.frame() 是创建数据表格,在本例中,name 和 value 是自定义的,需要保证二者维度一致。
* ggplot(data,aes()) 函数中 data 表示绑定的数据源,而后面的 aes 则是指定了 x 轴和 y 轴, fill = name 则是表示自动填充颜色,如果不写默认黑白。
* geom_violin() 则表示以小提琴图展示。


# 密度图

> 描述:又称为「密度曲线图」。密度图 (Density Plot) 用于显示数据在连续时间段内的分布状况。这种图表是直方图的变种，使用平滑曲线来绘制数值水平，从而得出更平滑的分布。密度图的峰值显示数值在该时间段内最为高度集中的位置。密度图其中一个比直方图优胜的地方，是由于它们不受所使用分组数量（典型直方图中所使用的条形）的影响，所以能更好地界定分布形状 。举个例子，仅用 4 个条形的直方图所产生的分布形状，总不及使用 20 个条形的直方图般容易解读。然而，密度图则不会有这种问题。

```r
# Libraries
library(ggplot2)
library(dplyr)

# Load dataset from github
data <- read.table("https://raw.githubusercontent.com/holtzy/data_to_viz/master/Example_dataset/1_OneNum.csv", header=TRUE)

# Make the histogram
data %>%
  filter( price<300 ) %>%
  ggplot( aes(x=price)) +
    geom_density(fill="#69b3a2", color="#e9ecef", alpha=0.8)library(ggplot2)
```

使用主题 theme_ipsum 。

```r
# Libraries
library(ggplot2)
library(dplyr)
library(hrbrthemes)

# Load dataset from github
data <- read.table("https://raw.githubusercontent.com/holtzy/data_to_viz/master/Example_dataset/1_OneNum.csv", header=TRUE)

# Make the histogram
data %>%
  filter( price<300 ) %>%
  ggplot( aes(x=price)) +
    geom_density(fill="#69b3a2", color="#e9ecef", alpha=0.8) +
    ggtitle("Night price distribution of Airbnb appartements") +
    theme_ipsum()
```

# 直方图

> 直方图是数字变量分布的精确图形表示。它仅将数字变量作为输入。变量被切成几个箱，每个箱的观察次数由条形的高度表示。

```r
library(ggplot2)
data = data.frame(value=rnorm(100))
p <- ggplot(data,aes(x=value)) + geom_histogram()
p
```

bin 表示箱子，也据说柱子。修改 binwidth 可以控制柱宽。

```r
library(tidyverse)
library(hrbrthemes)

data <- read.table("https://raw.githubusercontent.com/holtzy/data_to_viz/master/Example_dataset/1_OneNum.csv", header=TRUE)

p <- data %>%
  filter( price<300 ) %>%
  ggplot( aes(x=price)) +
    geom_histogram( binwidth=3, fill="#69b3a2", color="#e9ecef", alpha=0.9) +
    ggtitle("Bin size = 3") +
    theme_ipsum() +
    theme(
      plot.title = element_text(size=15)
    )
p
```

因为形状像箱子所以称之为**箱线图**，也称为**盒须图**。

箱线图可以显示一组数据的**最大值**，**最小值**，**中位数**以及**上下四分位数**。

![](https://gitee.com/weijiew/pic/raw/master/img/1435.png)

* 将方框分成两部分的线是中位数，中位数意味着左右两边的数据个数相等。
* 盒子的首尾分别代表上四分位数（Q3）和下四分位数（Q1），也就是整体数据的 75% 和 25% 。
* 上四分位数（Q3）和下四分位数（Q1）之间的距离代表四分位距。
* 除此之外其余部分的点代表极端的异常值。

```r
# Libraries
library(tidyverse)
library(hrbrthemes)
library(viridis)

# create a dataset
data <- data.frame(
  name=c( rep("A",500), rep("B",500), rep("B",500), rep("C",20), rep('D', 100)  ),
  value=c( rnorm(500, 10, 5), rnorm(500, 13, 1), rnorm(500, 18, 1), rnorm(20, 25, 4), rnorm(100, 12, 1) )
)

# Plot
data %>%
  ggplot( aes(x=name, y=value, fill=name)) +
    geom_boxplot() +
    scale_fill_viridis(discrete = TRUE, alpha=0.6) +
    geom_jitter(color="black", size=0.4, alpha=0.9) +
    theme_ipsum() +
    theme(
      legend.position="none",
      plot.title = element_text(size=11)
    ) +
    ggtitle("A boxplot with jitter") +
    xlab("")
```

下面的图将散点去除了。

```r
# Boxplot basic
data %>%
  ggplot( aes(x=name, y=value, fill=name)) +
    geom_boxplot() +
    scale_fill_viridis(discrete = TRUE, alpha=0.6, option="A") +
    theme_ipsum() +
    theme(
      legend.position="none",
      plot.title = element_text(size=11)
    ) +
    ggtitle("Basic boxplot") +
    xlab("")

# Violin basic
data %>%
  ggplot( aes(x=name, y=value, fill=name)) +
    geom_violin() +
    scale_fill_viridis(discrete = TRUE, alpha=0.6, option="A") +
    theme_ipsum() +
    theme(
      legend.position="none",
      plot.title = element_text(size=11)
    ) +
    ggtitle("Violin chart") +
    xlab("")
```

# 脊线图 

> 脊线图（有时称为Joyplot）显示了几个组的数值分布。可以使用直方图或密度图来表示分布，所有直方图或密度图都对齐到相同的水平刻度，并且呈现出轻微的重叠。

```r
# library
library(ggridges)
library(ggplot2)
 
# Diamonds dataset is provided by R natively
#head(diamonds)
 
# basic example
ggplot(diamonds, aes(x = price, y = cut, fill = cut)) +
  geom_density_ridges() +
  theme_ridges() + 
  theme(legend.position = "none")
```

# 轴

```r
# 导入
library(ggplot2)

# 绑定数据，设定采用 geom_point() 绘图。
basic <- ggplot(mtcars,aes(x=mpg,y=wt)) + geom_point()

# 显示图像
basic

# xlab 设定了 x 轴的标题， xlim 设定了 x 轴的范围
basic + xlab("mpg value") + xlim(0,50)
```

## 自定义轴

函数 theme() 可以**自定义** ggplot2 图标的所有部分。

`axis.title` 控制了轴标题外貌。因为是文本，所以将其采用 element_text 进行包装。

```r
basic + theme(axis.title = element_text(angle = 90,color = "red", size = 15, face = 3))
```

angle 表示标题旋转 90 度， color 即为颜色， size 表示尺寸，face 表示位置。

```r
basic + theme(axis.title.x = element_text( angle = 90, color="red", size=15, face=3))
```

`axis.title.x` 表示只修改 x 轴。

## axis.text

自定义坐标轴刻度。

```r
basic + theme(axis.text = element_text(angle = 90 , color = "blue", size = 15, face = 3))
```

## axis.ticks 和 axis.line

```r
basic + theme(axis.ticks = element_line(size = 2, color = "red"),axis.ticks.length = unit(.5,"cm"))
```

修如下部分：

![](https://gitee.com/weijiew/pic/raw/master/img/20200905163842.png)

注意如果是以 line 未单位进行修改，那么采用 element_line 以线为单位进行包装。

```r
basic + theme(axis.line = element_line(size = 3, colour =  "green",linetype = 2))
```

```r
ggplot(mtcars,aes(x=mpg,y=wt)) + 
    geom_point() + 
    theme(
        axis.title = element_text(
            color = "red",
            size = 15,
            face = 2),
        axis.line = element_line(
            size = 3,
            colour = "green",
            linetype = 2),
        axis.text = element_text(
            angle = 90,
            color = "blue",
            size=15,
            face=2)
        )
```

# 颜色

```r
ggplot(mtcars,aes(x=drat)) + 
    geom_density(
        color="purple",
        fill= "#69b3a2",
        size=2
    )
```

自动上色。

```r
ggplot(iris, aes(x=Sepal.Length, y=Sepal.Width, color=Species)) +
  geom_point(size=6)
```
# 参考 

1. [free-introduction-to-r](https://learn.datacamp.com/courses/free-introduction-to-r)