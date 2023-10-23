# 第五章 if 语句

## 5.1一个简单示例

下面是一个简短的示例，演示了如何使用if 语句来正确地处理特殊情形。假设你有一个汽车列表，并想将其中每辆汽车的名称打印出来。对于大多数汽车，都应以首字母大写的方式打印其名称，但对于汽车名'bmw' ，应以全大写的方式打印。下面的代码遍历一个列表，并以首字母大写的方式打印其中的汽车名，但对于汽车名'bmw' ，以全大写的方式打印：

cars = ['audi', 'bmw', 'subaru', 'toyota']

for car in cars: 

​	if car == 'bmw':

​		print(car.upper())

​	else:

​		print(car.title())

首先检查当前的汽车名是否是'bmw'。如果是，就以全大写的方式打印它；否则就以首字母大写的方式打印：

## 5.2条件测试

每条if 语句的核心都是一个值为True 或False 的表达式，这种表达式被称为条件测试。Python根据条件测试的值为True 还是False 来决定是否执行if 语句中的代码。如果条件测试的值为True ，Python就执行紧跟在if 语句后面的代码；如果为False ，Python就忽略这些代码。

### 5.2.1检查是否相等

大多数条件测试都将一个变量的当前值同特定值进行比较。最简单的条件测试检查变量的值是否与特定值相等：

\>>> car = 'bmw' 

\>\>> car == 'bmw'

True

我们首先使用一个等号将car 的值设置为'bmw' ，这种做法你已见过很多次。接下来，使用两个等号（== ）检查car 的值是否为'bmw' 。这个相等运算符在它两边的值相等时返回True ，否则返回False 。在这个示例中，两边的值相等，因此Python返回True 

如果变量car 的值不是'bmw' ，上述测试将返回False 

\>>> car = 'audi'

\>\>\>car == 'bmw'

False

一个等号是陈述；可解读为“将变量car 的值设置为'audi' ”。两个等号是发问；可解读为“变量car 的值是'bmw' 吗？”。大多数编程语言使用等号的方式都与这里演示的相同。

### 5.2.2检查是否相等，不考虑大小写

在Python中检查是否相等时区分大小写，例如，两个大小写不同的值会被视为不相等：

\>>> car = 'Audi'

\>>> car == 'audi'

False

如果大小写很重要，这种行为有其优点。但如果大小写无关紧要，而只想检查变量的值，可将变量的值转换为小写，再进行比较：

\>>> car = 'Audi'

\>>> car.lower() == 'audi'

True

无论值'Audi' 的大小写如何，上述测试都将返回True ，因为该测试不区分大小写。函数lower() 不会修改存储在变量car 中的值，因此进行这样的比较时不会影响原来的变量：

\>>> car = 'Audi' 

\>>> car.lower() == 'audi'

True 

\>>> car

'Audi'

我们将首字母大写的字符串'Audi' 存储在变量car 中；我们获取变量car 的值并将其转换为小写，再将结果与字符串'audi' 进行比较。这两个字符串相同，因此Python返回True 。这个条件测试并没有影响存储在变量car 中的值。

网站采用类似的方式让用户输入的数据符合特定的格式。例如，网站可能使用类似的测试来确保用户名是独一无二的，而并非只是与另一个用户名的大小写不同。用户提交新的用户名时，将把它转换为小写，并与所有既有用户名的小写版本进行比较。执行这种检查时，如果已经有用户名'john' （不管大小写如何），则用户提交用户名'John' 时将遭到拒绝。

### 5.2.3检查是否不相等

要判断两个值是否不等，可结合使用惊叹号和等号（!= ），其中的惊叹号表示不 ，在很多编程语言中都如此。

下面再使用一条if 语句来演示如何使用不等运算符。我们将把要求的比萨配料存储在一个变量中，再打印一条消息，指出顾客要求的配料是否是意式小银鱼（anchovies）：

requested_topping = 'mushrooms'

if requested_topping != 'anchovies':

​		print("Hold the anchovies!")

将requested_topping 的值与'anchovies' 进行比较，如果它们不相等，Python将返回True ，进而执行紧跟在if 语句后面的代码；如果这两个值相等，Python将返回False ，因此不执行紧跟在if 语句后面的代码。

由于requested_topping 的值不是'anchovies' ，因此执行print 语句：

Hold the anchovies!

你编写的大多数条件表达式都检查两个值是否相等，但有时候检查两个值是否不等的效率更高。

### 5.2.4比较数字

检查数值非常简单，例如，下面的代码检查一个人是否是18岁：

\>>> age = 18

\>>> age == 18

True

你还可以检查两个数字是否不等，例如，下面的代码在提供的答案不正确时打印一条消息：

answer = 17

if answer != 42:

print("That is not the correct answer. Please try again!")

answer （17 ）不是42 ，条件得到满足，因此缩进的代码块得以执行：That is not the correct answer. Please try again!

条件语句中可包含各种数学比较，如小于、小于等于、大于、大于等于：

\>>> age = 19

\>>> age < 21

True

\>>> age <= 21

True

\>>> age > 21

False

\>>> age >= 21

False

在if 语句中可使用各种数学比较，这让你能够直接检查关心的条件。

### 5.2.5检查多个条件

你可能想同时检查多个条件，例如，有时候你需要在两个条件都为True 时才执行相应的操作，而有时候你只要求一个条件为True 时就执行相应的操作。在这些情况下，关键字and 和or 可助你一臂之力。

#### 1.使用and检查多个条件

要检查是否两个条件都为True ，可使用关键字and 将两个条件测试合而为一；如果每个测试都通过了，整个表达式就为True ；如果至少有一个测试没有通过，整个表达式就为False 

例如，要检查是否两个人都不小于21岁，可使用下面的测试：

\>>> age_0 = 22

\>>> age_1 = 18 

\>\>\>age_0 >= 21 and age_1 >= 21

False 

\>>> age_1 = 22

\>>> age_0 >= 21 and age_1 >= 21

True

我们定义了两个用于存储年龄的变量：age_0 和age_1 。我们检查这两个变量是否都大于或等于21；左边的测试通过了，但右边的测试没有通过，因此整个条件表达式的结果为False 。我们将age_1 改为22，这样age_1 的值大于21，因此两个测试都通过了，导致整个条件表达式的结果为True 。

为改善可读性，可将每个测试都分别放在一对括号内，但并非必须这样做。如果你使用括号，测试将类似于下面这样：

(age_0 >= 21) and (age_1 >= 21)

#### 2.使用or 检查多个条件

关键字or 也能够让你检查多个条件，但只要至少有一个条件满足，就能通过整个测试。仅当两个测试都没有通过时，使用or 的表达式才为False 。

下面再次检查两个人的年龄，但检查的条件是至少有一个人的年龄不小于21岁：

\>>> age_0 = 22

\>>> age_1 = 18

\>> age_0 >= 21 or age_1 >= 21

True 

\>>> age_0 = 18

\>>> age_0 >= 21 or age_1 >= 21

False

同样，我们首先定义了两个用于存储年龄的变量。由于对age_0 的测试通过了，因此整个表达式的结果为True 。接下来，我们将age_0 减小为18；两个测试都没有通过，因此整个表达式的结果为False 

### 5.2.6检查特定值是否包含在列表中

有时候，执行操作前必须检查列表是否包含特定的值。例如，结束用户的注册过程前，可能需要检查他提供的用户名是否已包含在用户名列表中。在地图程序中，可能需要检查用户提交的位置是否包含在已知位置列表中。

要判断特定的值是否已包含在列表中，可使用关键字in 。来看你可能为比萨店编写的一些代码；这些代码首先创建一个列表，其中包含用户点的比萨配料，然后检查特定的配料是否包含在该列表中。

\>>> requested_toppings = ['mushrooms', 'onions', 'pineapple'] 

\>>> 'mushrooms' in requested_toppings

True

\>>> 'pepperoni' in requested_toppings

False

关键字in 让Python检查列表requested_toppings 是否含'mushrooms' 和'pepperoni' 。这种技术很有用，它让你能够在创建一个列表后，轻松地检查其中是否包含特定的值。

### 5.2.7检查特定值不包含在列表中

还有些时候，确定特定的值未包含在列表中很重要；在这种情况下，可使用关键字not in 。例如，如果有一个列表，其中包含被禁止在论坛上发表评论的用户，就可在允许用户提交评论前检查他是否被禁言：

banned_users = ['andrew', 'carolina', 'david']

user = 'marie'

if user not in banned_users:

​		print(user.title() + ", you can post a response if you wish.")

如果user 的值未包含在列表banned_users 中，Python将返回True ，进而执行缩进的代码行。

用户'marie' 未包含在列表banned_users 中，因此她将看到一条邀请她发表评论的消息：

Marie, you can post a response if you wish.

### 5.2.8布尔表达式

随着你对编程的了解越来越深入，将遇到术语布尔表达式，它不过是条件测试的别名。与条件表达式一样，布尔表达式的结果要么为True ，要么为False 。

布尔值通常用于记录条件，如游戏是否正在运行，或用户是否可以编辑网站的特定内容：

game_active = True

can_edit = False

在跟踪程序状态或程序中重要的条件方面，布尔值提供了一种高效的方式。

## 5.3  if语句

### 5.3.1简单的if语句

最简单的if 语句只有一个测试和一个操作：

if conditional_test:

​				do something

在第1行中，可包含任何条件测试，而在紧跟在测试后面的缩进代码块中，可执行任何操作。如果条件测试的结果为True ，Python就会执行紧跟在if 语句后面的代码；否则Python将忽略这些代码。

age = 19 

if age >= 18: 

​		print("You are old enough to vote!")

Python检查变量age 的值是否大于或等于18；答案是肯定的，因此Python执行缩进的print 语句：

You are old enough to vote!

在if 语句中，缩进的作用与for 循环中相同。如果测试通过了，将执行if 语句后面所有缩进的代码行，否则将忽略它们。

在紧跟在if 语句后面的代码块中，可根据需要包含任意数量的代码行。下面在一个人够投票的年龄时再打印一行输出，问他是否登记了：

age = 19

if age >= 18:

​			print("You are old enough to vote!")

​			print("Have you registered to vote yet?")

条件测试通过了，而两条print 语句都缩进了，因此它们都将执行：

You are old enough to vote!

Have you registered to vote yet?

如果age 的值小于18，这个程序将不会有任何输出。

### 5.3.2if-else语句

经常需要在条件测试通过了时执行一个操作，并在没有通过时执行另一个操作；在这种情况下，可使用Python提供的if-else 语句。if-else 语句块类似于简单的if 语句，但其中的else 语句让你能够指定条件测试未通过时要执行的操作。

下面的代码在一个人够投票的年龄时显示与前面相同的消息，同时在这个人不够投票的年龄时也显示一条消息：

age = 17 

if age >= 18:

​		print("You are old enough to vote!")

​		print("Have you registered to vote yet?") 

else:

​		print("Sorry, you are too young to vote.")

​		print("Please register to vote as soon as you turn 18!")

if代码块条件测试通过了，就执行第一个缩进的print 语句块；如果测试结果为False ，就执行else 代码块。这次age 小于18，条件测试未通过，因此执行else 代码块中的代码：	

Sorry, you are too young to vote.

Please register to vote as soon as you turn 18!

### 5.3.3if-elif-else结构

经常需要检查超过两个的情形，为此可使用Python提供的if-elif-else 结构。Python只执行if-elif-else 结构中的一个代码块，它依次检查每个条件测试，直到遇到通过了的条件测试。测试通过后，Python将执行紧跟在它后面的代码，并跳过余下的测试。

在现实世界中，很多情况下需要考虑的情形都超过两个。例如，来看一个根据年龄段收费的游乐场：

4岁以下免费；

4~18岁收费5美元；

18岁（含）以上收费10美元。

如果只使用一条if 语句，如何确定门票价格呢？下面的代码确定一个人所属的年龄段，并打印一条包含门票价格的消息：

age = 12

if age < 4:

​			print("Your admission cost is $0.") 

elif age < 18:

​			print("Your admission cost is $5.")

else:

​			print("Your admission cost is $10.")

if 测试检查一个人是否不满4岁，如果是这样，Python就打印一条合适的消息，并跳过余下的测试。elif 代码行其实是另一个if 测试，它仅在前面的测试未通过时才会运行。在这里，我们知道这个人不小于4岁，因为第一个测试未通过。如果这个人未满18岁，Python将打印相应的消息，并跳过else 代码块。如果if 测试和elif 测试都未通过，Python将运行else 代码块中的代码。

Your admission cost is $5.

只要年龄超过17岁，前两个测试就都不能通过。在这种情况下，将执行else 代码块，指出门票价格为10美元。

为让代码更简洁，可不在if-elif-else 代码块中打印门票价格，而只在其中设置门票价格，并在它后面添加一条简单的print 语句：

age = 12

if age < 4:

​	price = 0

elif age < 18: 

​	price = 5

else: 

​	price = 10

print("Your admission cost is $" + str(price) + ".")

这些代码大都未变。第二个elif 代码块通过检查确定年龄不到65岁后，才将门票价格设置为全票价格——10美元。请注意，在else 代码块中，必须将所赋的值改为5，因为仅当年龄超过65（含）时，才会执行这个代码块。

### 5.3.5省略else代码块

Python并不要求if-elif 结构后面必须有else 代码块。在有些情况下，else 代码块很有用；而在其他一些情况下，使用一条elif 语句来处理特定的情形更清晰：

age = 12

if age < 4:

​	price = 0

elif age < 18:

​	price = 5

elif age < 65:

​	price = 10 

elif age >= 65:

​	price = 5

print("Your admission cost is $" + str(price) + ".")

因为age<18和age>=65的代码块结果一样进行合并.

age = 12

if age < 4:

​	price = 0

elif age < 18   and  age  >=65:

​	price = 5

elif age < 65:

​	price = 10 

print("Your admission cost is $" + str(price) + ".")

else 是一条包罗万象的语句，只要不满足任何if 或elif 中的条件测试，其中的代码就会执行，这可能会引入无效甚至恶意的数据。如果知道最终要测试的条件，应考虑使用一个elif 代码块来代替else 代码块。这样，你就可以肯定，仅当满足相应的条件时，你的代码才会执行。

### 5.3.6 测试多个条件

if-elif-else 结构功能强大，但仅适合用于只有一个条件满足的情况：遇到通过了的测试后，Python就跳过余下的测试。这种行为很好，效率很高，让你能够测试一个特定的条件。

然而，有时候必须检查你关心的所有条件。在这种情况下，应使用一系列不包含elif 和else 代码块的简单if 语句。在可能有多个条件为True ，且你需要在每个条件为True 时都采取相应措施时，适合使用这种方法。

如果顾客点了两种配料，就需要确保在其比萨中包含这些配料：

requested_toppings = ['mushrooms', 'extra cheese']

if 'mushrooms' in requested_toppings:

​		print("Adding mushrooms.") 

if 'pepperoni' in requested_toppings:

​		print("Adding pepperoni.")

if 'extra cheese' in requested_toppings:

​		print("Adding extra cheese.")

print("\nFinished making your pizza!")

我们首先创建了一个列表，其中包含顾客点的配料。if 语句检查顾客是否点了配料蘑菇（'mushrooms' ），如果点了，就打印一条确认消息。检查配料辣香肠（'pepperoni' ）的代码也是一个简单的if 语句，而不是elif 或else 语句；因此不管前一个测试是否通过，都将进行这个测试。代码检查顾客是否要求多加芝士（'extra cheese' ）；不管前两个测试的结果如何，都会执行这些代码。每当这个程序运行时，都会进行这三个独立的测试。

在这个示例中，会检查每个条件，因此将在比萨中添加蘑菇并多加芝士：

Adding mushrooms.

Adding extra cheese.

Finished making your pizza!

如果像下面这样转而使用if-elif-else 结构，代码将不能正确地运行，因为有一个测试通过后，就会跳过余下的测试：

requested_toppings = ['mushrooms', 'extra cheese']

if 'mushrooms' in requested_toppings:

​		print("Adding mushrooms.")

elif 'pepperoni' in requested_toppings:

​		print("Adding pepperoni.")

elif 'extra cheese' in requested_toppings:

​		print("Adding extra cheese.")

print("\nFinished making your pizza!")

第一个测试检查列表中是否包含'mushrooms' ，它通过了，因此将在比萨中添加蘑菇。然而，Python将跳过if-elif-else 结构中余下的测试，不再检查列表中是否包含'extra cheese' 和'pepperoni' 。其结果是，将添加顾客点的第一种配料，但不会添加其他的配料：

总之，如果你只想执行一个代码块，就使用if-elif-else 结构；如果要运行多个代码块，就使用一系列独立的if 语句。

## 5.4使用if语句处理列表

通过结合使用if 语句和列表，可完成一些有趣的任务：对列表中特定的值做特殊处理；高效地管理不断变化的情形，如餐馆是否还有特定的食材；证明代码在各种情形下都将按预期那样运行。

### 5.4.1检查特殊元素

这家比萨店在制作比萨时，每添加一种配料都打印一条消息。通过创建一个列表，在其中包含顾客点的配料，并使用一个循环来指出添加到比萨中的配料，可以以极高的效率编写这样的代码：

requested_toppings = ['mushrooms', 'green peppers', 'extra cheese']

for requested_topping in requested_toppings:

​		print("Adding " + requested_topping + ".")

print("\nFinished making your pizza!")

输出很简单，因为上述代码不过是一个简单的for 循环：

Adding mushrooms.

Adding green peppers.

Adding extra cheese.

Finished making your pizza!

然而，如果比萨店的青椒用完了，该如何处理呢？为妥善地处理这种情况，可在for 循环中包含一条if 语句：

requested_toppings = ['mushrooms', 'green peppers', 'extra cheese']

for requested_topping in requested_toppings: 

​	if requested_topping == 'green peppers':

​			print("Sorry, we are out of green peppers right now.") 

​	else:

​			print("Adding " + requested_topping + ".")	

print("\nFinished making your pizza!")

这里在比萨中添加每种配料前都进行检查。if代码检查顾客点的是否是青椒，如果是，就显示一条消息，指出不能点青椒的原因。else 代码块确保其他配料都将添加到比萨中。

输出表明，妥善地处理了顾客点的每种配料：

Adding mushrooms.

Sorry, we are out of green peppers right now.

Adding extra cheese.

Finished making your pizza!

### 5.4.3确定列表不是空的

到目前为止，对于处理的每个列表都做了一个简单的假设，即假设它们都至少包含一个元素。我们马上就要让用户来提供存储在列表中的信息，因此不能再假设循环运行时列表不是空的。有鉴于此，在运行for 循环前确定列表是否为空很重要。

requested_toppings = []

if requested_toppings:

​	for requested_topping in requested_toppings:

​			print("Adding " + requested_topping + ".")

​	print("\nFinished making your pizza!") 

else:

​	print("Are you sure you want a plain pizza?")

在这里，我们首先创建了一个空列表，其中不包含任何配料。在❷处我们进行了简单检查，而不是直接执行for 循环。在if 语句中将列表名用在条件表达式中时，Python将在列表至少包含一个元素时返回True ，并在列表为空时返回False 。如果requested_toppings 不为空，就运行与前一个示例相同的for 循环；否则，就打印一条消息，询问顾客是否确实要点不加任何配料的普通比萨。

在这里，这个列表为空，因此输出如下——询问顾客是否确实要点普通比萨：

Are you sure you want a plain pizza?

如果这个列表不为空，将显示在比萨中添加的各种配料的输出。

### 5.4.3使用多个列表

顾客的要求往往五花八门，在比萨配料方面尤其如此。如果顾客要在比萨中添加炸薯条，该怎么办呢？可使用列表和if 语句来确定能否满足顾客的要求。

来看看在制作比萨前如何拒绝怪异的配料要求。下面的示例定义了两个列表，其中第一个列表包含比萨店供应的配料，而第二个列表包含顾客点的配料。这次对

于requested_toppings 中的每个元素，都检查它是否是比萨店供应的配料，再决定是否在比萨中添加它：

available_toppings = ['mushrooms', 'olives', 'green peppers',

'pepperoni', 'pineapple', 'extra cheese']

requested_toppings = ['mushrooms', 'french fries', 'extra cheese']

for requested_topping in requested_toppings:

​	if requested_topping in available_toppings:

​			print("Adding " + requested_topping + ".")

​	else:

​			print("Sorry, we don't have " + requested_topping + ".")

print("\nFinished making your pizza!")

我们定义了一个列表，其中包含比萨店供应的配料。请注意，如果比萨店供应的配料是固定的，也可使用一个元组来存储它们。我们又创建了一个列表，其中包含顾客点的配料，请注意那个不同寻常的配料——'french fries' 。我们遍历顾客点的配料列表。在这个循环中，对于顾客点的每种配料，我们都检查它是否包含在供应的配料列表中；如果答案是肯定的，就将其加入到比萨中，否则将运行else 代码块：打印一条消息，告诉顾客不供应这种配料。

Adding mushrooms.

Sorry, we don't have french fries.

Adding extra cheese.

Finished making your pizza!

## 5.5设置if语句的格式

在条件测试的格式设置方面，PEP 8提供的唯一建议是，在诸如== 、>= 和<= 等比较运算符两边各添加一个空格，例如，if  age < 4: 要比if     age   <     4: 好。

这样的空格不会影响Python对代码的解读，而只是让代码阅读起来更容易。