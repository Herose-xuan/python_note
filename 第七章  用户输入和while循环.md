# 第七章  用户输入和while循环

大多数程序都旨在解决最终用户的问题，为此通常需要从用户那里获取一些信息。例如，假设有人要判断自己是否到了投票的年龄，要编写回答这个问题的程序，就需要知道用户的年龄，这样才能给出答案。因此，这种程序需要让用户输入其年龄，再将其与投票年龄进行比较，以判断用户是否到了投票的年龄，再给出结果。

通过获取用户输入并学会控制程序的运行时间，可编写出交互式程序。

## 7.1函数input（）的工作原理

函数input() 让程序暂停运行，等待用户输入一些文本。获取用户输入后，Python将其存储在一个变量中，以方便你使用。

例如，下面的程序让用户输入一些文本，再将这些文本呈现给用户：

message = input("Tell me something, and I will repeat it back to you: ")

print(message)

函数input() 接受一个参数：即要向用户显示的提示或说明，让用户知道该如何做。在这个示例中，Python运行第1行代码时，用户将看到提示Tell me something, and I will repeat it back to you: 。程序等待用户输入，并在用户按回车键后继续运行。输入存储在变量message 中，接下来的print(message) 将输入呈现给用户：

Tell me something, and I will repeat it back to you: Hello everyone!

Hello everyone!

### 7.1.1编写清晰的程序

每当你使用函数input() 时，都应指定清晰而易于明白的提示，准确地指出你希望用户提供什么样的信息——指出用户该输入任何信息的提示都行，如下所示：

name = input("Please enter your name: ")

print("Hello, " + name + "!")

通过在提示末尾（这里是冒号后面）包含一个空格，可将提示与用户输入分开，让用户清楚地知道其输入始于何处，如下所示：

Please enter your name: Eric

Hello, Eric!

有时候，提示可能超过一行，例如，你可能需要指出获取特定输入的原因。在这种情况下，可将提示存储在一个变量中，再将该变量传递给函数input() 。这样，即便提示超过一行，input() 语句也非常清晰。

prompt = "If you tell us who you are, we can personalize the messages you see."

prompt += "\nWhat is your first name? "

name = input(prompt)

print("\nHello, " + name + "!")

If you tell us who you are, we can personalize the messages you see.

What is your first name?  Eric

Hello, Eric!

### 7.1.2用int（）来获取数值输入

使用函数input() 时，Python将用户输入解读为字符串。请看下面让用户输入其年龄的解释器会话：

\>>> age = input("How old are you? ")

How old are you? 21

\>>> age

'21'

用户输入的是数字21，但我们请求Python提供变量age 的值时，它返回的是'21' ——用户输入的数值的字符串表示。我们怎么知道Python将输入解读成了字符串呢？因为这个数字用引号括起了。如果我们只想打印输入，这一点问题都没有；但如果你试图将输入作为数字使用，就会引发错误：

\>>> age = input("How old are you? ")

How old are you? 21

\>>> age >= 18

Traceback (most recent call last):

File "<stdin>", line 1, in <module> 

TypeError: unorderable types: str() >= int() #（不可排序（比较）的类型）

你试图将输入用于数值比较时，Python会引发错误，因为它无法将字符串和整数进行比较：不能将存储在age 中的字符串'21' 与数值18 进行比较。

为解决这个问题，可使用函数int() ，它让Python将输入视为数值。函数int() 将数字的字符串表示转换为数值表示，如下所示：

\>>> age = input("How old are you? ")

How old are you? 21 

\>>> age = int(age)

\>>> age >= 18

True

我们在提示时输入21 后，Python将这个数字解读为字符串，但随后int() 将这个字符串转换成了数值表示。这样Python就能运行条件测试了：将变量age （它现在包含数值21）同18 进行比较，看它是否大于或等于18。测试结果为True 

请看下面的程序，它判断一个人是否满足坐过山车的身高要求：

height = input("How tall are you, in inches? ")

height = int(height)

if height >= 36:

​	print("\nYou're tall enough to ride!")

else:

​	print("\nYou'll be able to ride when you're a little older.")

在这个程序中，为何可以将height 同36 进行比较呢？因为在比较前，height = int(height) 将输入转换成了数值表示。如果输入的数字大于或等于36，我们就告诉用户他满足身高条件：

How tall are you, in inches? 71

You're tall enough to ride!

### 7.1.3求模运算符（取余）

处理数值信息时，求模运算符（%）是一个很有用的工具，它将两个数相除并返回余数：

\>>> 4 % 3

1

\>>> 5 % 3

2

\>>> 6 % 3

0

\>>> 7 % 3

1

求模运算符不会指出一个数是另一个数的多少倍，而只指出余数是多少。

如果一个数可被另一个数整除，余数就为0，因此求模运算符将返回0。你可利用这一点来判断一个数是奇数还是偶数：

number = input("Enter a number, and I'll tell you if it's even or odd: ")

number = int(number)

if number % 2 == 0:

​	print("\nThe number " + str(number) + " is even.")

else:

​	print("\nThe number " + str(number) + " is odd.")

偶数都能被2整除，因此对一个数（number ）和2执行求模运算的结果为零，即number % 2 == 0 ，那么这个数就是偶数；否则就是奇数。

Enter a number, and I'll tell you if it's even or odd: 42

The number 42 is even.

### 7.1.4在python2.7中获取输入

如果你使用的是Python 2.7，应使用函数raw_input() 来提示用户输入。这个函数与Python 3中的input() 一样，也将输入解读为字符串。

Python 2.7也包含函数input() ，但它将用户输入解读为Python代码，并尝试运行它们。因此，最好的结果是出现错误，指出Python不明白输入的代码；而最糟的结果是，将运行你原本无意运行的代码。如果你使用的是Python 2.7，请使用raw_input() 而不是input() 来获取输入。

## 7.2 while循环

for 循环用于针对集合中的每个元素都一个代码块，而while 循环不断地运行，直到指定的条件不满足为止。

### 7.2.1使用while循环

你可以使用while 循环来数数，例如，下面的while 循环从1数到5：

current_number = 1

while current_number <= 5:

print(current_number)

current_number += 1

在第1行，我们将current_number 设置为1，从而指定从1开始数。接下来的while 循环被设置成这样：只要current_number 小于或等于5，就接着运行这个循环。循环中的代码打印current_number 的值，再使用代码current_number += 1 （代码current_number = current_number + 1 的简写）将其值加1。

只要满足条件current_number <= 5 ，Python就接着运行这个循环。由于1小于5，因此Python打印1 ，并将current_number 加1，使其为2 ；由于2小于5，因此Python打印2 ，并将current_number 加1 ，使其为3 ，以此类推。一旦current_number 大于5，循环将停止，整个程序也将到此结束：

1

2

3

4

5

你每天使用的程序很可能就包含while 循环。例如，游戏使用while 循环，确保在玩家想玩时不断运行，并在玩家想退出时停止运行。如果程序在用户没有让它停止时停止运行，或者在用户要退出时还继续运行，那就太没有意思了；有鉴于此，while 循环很有用。

### 7.2.2 让用户选择何时退出

可使用while 循环让程序在用户愿意时不断地运行，我们在其中定义了一个退出值，只要用户输入的不是这个值，程序就接着运行：

prompt = "\nTell me something, and I will repeat it back to you:"

prompt += "\nEnter 'quit' to end the program. 

message = "" 

while message != 'quit':

​	message = input(prompt)

​	print(message)

我们定义了一条提示消息，告诉用户他有两个选择：要么输入一条消息，要么输入退出值（这里为'quit' ）。接下来，我们创建了一个变量——message ，用于存储用户输入的值。我们将变量message 的初始值设置为空字符串"" ，让Python首次执行while 代码行时有可供检查的东西。Python首次执行while 语句时，需要将message 的值与'quit' 进行比较，但此时用户还没有输入。如果没有可供比较的东西，Python将无法继续运行程序。为解决这个问题，我们必须给变量message 指定一个初始值。虽然这个初始值只是一个空字符串，但符合要求，让Python能够执行while 循环所需的比较。只要message 的值不是'quit' ，这个循环就会不断运行。

首次遇到这个循环时，message 是一个空字符串，因此Python进入这个循环。执行到代码行message = input(prompt) 时，Python显示提示消息，并等待用户输入。不管用户输入是什么，都将存储到变量message 中并打印出来；接下来，Python重新检查while 语句中的条件。只要用户输入的不是单词'quit' ，Python就会再次显示提示消息并等待用户输入。等到用户终于输入'quit' 后，Python停止执行while 循环，而整个程序也到此结束：

Tell me something, and I will repeat it back to you:

Enter 'quit' to end the program. Hello everyone!

Hello everyone!

Tell me something, and I will repeat it back to you:

Enter 'quit' to end the program. Hello again.

Hello again.

Tell me something, and I will repeat it back to you:

Enter 'quit' to end the program. quit

quit

这个程序很好，唯一美中不足的是，它将单词'quit' 也作为一条消息打印了出来。为修复这种问题，只需使用一个简单的if 测试：

prompt = "\nTell me something, and I will repeat it back to you:"

prompt += "\nEnter 'quit' to end the program. "

message = ""

while message != 'quit':

​	message = input(prompt)

​	if message != 'quit':

​		print(message)

现在，程序在显示消息前将做简单的检查，仅在消息不是退出值时才打印它：

Tell me something, and I will repeat it back to you:

Enter 'quit' to end the program. Hello everyone!

Hello everyone!

Tell me something, and I will repeat it back to you:

Enter 'quit' to end the program. Hello again.

Hello again.

Tell me something, and I will repeat it back to you:

Enter 'quit' to end the program. quit

### 7.2.3使用标志

在前一个示例中，我们让程序在满足指定条件时就执行特定的任务。但在更复杂的程序中，很多不同的事件都会导致程序停止运行；在这种情况下，该怎么办呢？

例如，在游戏中，多种事件都可能导致游戏结束，如玩家一艘飞船都没有了或要保护的城市都被摧毁了。导致程序结束的事件有很多时，如果在一条while 语句中检查所有这些条件，将既复杂又困难。

在要求很多条件都满足才继续运行的程序中，可定义一个变量，用于判断整个程序是否处于活动状态。这个变量被称为标志，充当了程序的交通信号灯。你可让程序在标志为True 时继续运行，并在任何事件导致标志的值为False 时让程序停止运行。这样，在while 语句中就只需检查一个条件——标志的当前值是否为True ，并将所有测试（是

否发生了应将标志设置为False 的事件）都放在其他地方，从而让程序变得更为整洁。

。我们把这个标志命名为active （可给它指定任何名称），它将用于判断程序是否应继续运行

prompt = "\nTell me something, and I will repeat it back to you:"

prompt += "\nEnter 'quit' to end the program. "

active = True 

while active:

​	message = input(prompt)

​	if message == 'quit':

​			active = False

​	else:

​			print(message)

我们将变量active 设置成了True ，让程序最初处于活动状态。这样做简化了while 语句，因为不需要在其中做任何比较——相关的逻辑由程序的其他部分处理。只要变量active 为True ，循环就将继续运行。

这个程序的输出与前一个示例相同。在前一个示例中，我们将条件测试直接放在了while 语句中，而在这个程序中，我们使用了一个标志来指出程序是否处于活动状态，这样如果要添加测试（如elif 语句）以检查是否发生了其他导致active 变为False 的事件，将很容易。在复杂的程序中，如很多事件都会导致程序停止运行的游戏中，标志很有用：在其中的任何一个事件导致活动标志变成False 时，主游戏循环将退出，此时可显示一条游戏结束消息，并让用户选择是否要重新玩。

### 7.2.4使用break退出循环

要立即退出while 循环，不再运行循环中余下的代码，也不管条件测试的结果如何，可使用break 语句。break 语句用于控制程序流程，可使用它来控制哪些代码行将执行，哪些代码行不执行，从而让程序按你的要求执行你要执行的代码。

来看一个让用户指出他到过哪些地方的程序。在这个程序中，我们可以在用户输入'quit' 后使用break 语句立即退出while 循环：

prompt = "\nPlease enter the name of a city you have visited:"

prompt += "\n(Enter 'quit' when you are finished.) "

while True:

​	city = input(prompt)

​	if city == 'quit':

​		break

​	else:

​		print("I'd love to go to " + city.title() + "!")

以while True 打头的循环将不断运行，直到遇到break 语句。这个程序中的循环不断输入用户到过的城市的名字，直到他输入'quit' 为止。用户输入'quit' 后，将执行break 语句，导致Python退出循环：

Please enter the name of a city you have visited:

(Enter 'quit' when you are finished.) New York

I'd love to go to New York!

Please enter the name of a city you have visited:

(Enter 'quit' when you are finished.) San Francisco

I'd love to go to San Francisco!

Please enter the name of a city you have visited:

(Enter 'quit' when you are finished.) quit

在任何Python循环中都可使用break 语句。例如，可使用break 语句来退出遍历列表或字典的for 循环。

### 7.2.5在循环中使用continue

要返回到循环开头，并根据条件测试结果决定是否继续执行循环，可使用continue 语句，它不像break 语句那样不再执行余下的代码并退出整个循环。例如，来看一个从1数到10，但只打印其中偶数的循环：

current_number = 0

while current_number < 10: 

​	current_number += 1

​	if current_number % 2 ！= 0:

​		continue

​	print(current_number)

我们首先将current_number 设置成了0，由于它小于10，Python进入while 循环。进入循环后，我们以步长1的方式往上数，因此current_number 为1。接下来，if 语句检查current_number 与2的求模运算结果。如果结果为0（意味着current_number 可被2整除），就执行continue 语句，让Python忽略余下的代码，并返回

到循环的开头。如果当前的数字不能被2整除，就执行循环中余下的代码，Python将这个数字打印出来：

2
4
6
8
10

### 7.2.6避免无限循环

每个while 循环都必须有停止运行的途径，这样才不会没完没了地执行下去。例如，下面的循环从1数到5：

x = 1

while x <= 5:

​	print(x)

​	x += 1

但如果你像下面这样不小心遗漏了代码行x += 1 ，这个循环将没完没了地运行：

\# 这个循环将没完没了地运行！

x = 1

while x <= 5:

​	print(x)

在这里，x 的初始值为1 ，但根本不会变，因此条件测试x <= 5 始终为True ，导致while 循环没完没了地打印1 ，如下所示：

1

1

1

1

--snip--

每个程序员都会偶尔因不小心而编写出无限循环，在循环的退出条件比较微妙时尤其如此。如果程序陷入无限循环，可按Ctrl+ C，也可关闭显示程序输出的终端窗口。

要避免编写无限循环，务必对每个while 循环进行测试，确保它按预期那样结束。如果你希望程序在用户输入特定值时结束，可运行程序并输入这样的值；如果在这种情况下程序没有结束，请检查程序处理这个值的方式，确认程序至少有一个这样的地方能让循环条件为False 或让break 语句得以执行。

## 7.3使用while循环来处理列表和字典

到目前为止，我们每次都只处理了一项用户信息：获取用户的输入，再将输入打印出来或作出应答；循环再次运行时，我们获悉另一个输入值并作出响应。然而，要记录大量的用户和信息，需要在while 循环中使用列表和字典。

for 循环是一种遍历列表的有效方式，但在for 循环中不应修改列表，否则将导致Python难以跟踪其中的元素。要在遍历列表的同时对其进行修改，可使用while 循环。通过将while 循环同列表和字典结合起来使用，可收集、存储并组织大量输入，供以后查看和显示。

### 7.3.1在列表之间移动元素

假设有一个列表，其中包含新注册但还未验证的网站用户；验证这些用户后，如何将他们移到另一个已验证用户列表中呢？一种办法是使用一个while 循环，在验证用户的同时将其从未验证用户列表中提取出来，再将其加入到另一个已验证用户列表中。代码可能类似于下面这样：

\# 首先，创建一个待验证用户列表

\# 和一个用于存储已验证用户的空列表

unconfirmed_users = ['alice', ‘brian', 'candace']

confirmed_users = []

\# 验证每个用户，直到没有未验证用户为止

\# 将每个经过验证的列表都移到已验证用户列表中

while unconfirmed_users: 

​	current_user = unconfirmed_users.pop()

​	print("Verifying user: " + current_user.title()) 

​	confirmed_users.append(current_user)

\# 显示所有已验证的用户

print("\nThe following users have been confirmed:")

for confirmed_user in confirmed_users:

​	print(confirmed_user.title())

我们首先创建了一个未验证用户列表，其中包含用户Alice、Brian和Candace，还创建了一个空列表，用于存储已验证的用户。while 循环将不断地运行，直到列表unconfirmed_users 变成空的。在这个循环中，函数pop() 以每次一个的方式从列表unconfirmed_users 末尾删除未验证的用户。由于Candace位于列表unconfirmed_users 末尾，因此其名字将首先被删除、存储到变量current_user 中并加入到列表confirmed_users 中。接下来是Brian，然后是Alice。

为模拟用户验证过程，我们打印一条验证消息并将用户加入到已验证用户列表中。未验证用户列表越来越短，而已验证用户列表越来越长。未验证用户列表为空后结束循环，再打印已验证用户列表：

Verifying user: Candace

Verifying user: Brian

Verifying user: Alice

The following users have been confirmed:

Candace

Brian

Alice

### 7.3.2删除包含特定值的所有列表元素

假设你有一个宠物列表，其中包含多个值为'cat' 的元素。要删除所有这些元素，可不断运行一个while 循环，直到列表中不再包含值'cat' ，如下所示：

pets = ['dog', 'cat', 'dog', 'goldfish', 'cat', 'rabbit', 'cat']

print(pets)

while 'cat' in pets:

​	pets.remove('cat')

print(pets)

我们首先创建了一个列表，其中包含多个值为'cat' 的元素。打印这个列表后，Python进入while 循环，因为它发现'cat' 在列表中至少出现了一次。进入这个循环后，Python 删除第一个'cat' 并返回到while 代码行，然后发现'cat' 还包含在列表中，因此再次进入循环。它不断删除'cat' ，直到这个值不再包含在列表中，然后退出循环并再次打

印列表：

['dog', 'cat', 'dog', 'goldfish', 'cat', 'rabbit', 'cat']

['dog', 'dog', 'goldfish', 'rabbit']

### 7.3.3使用用户输入来填充字典

可使用while循环提示用户输入任意数量的信息。下面来创建一个调查程序，其中的循环每次执行时都提示输入被调查者的名字和回答。我们将收集的数据存储在一个字典中，以便将回答同被调查者关联起来：

responses = {}

\# 设置一个标志，指出调查是否继续

polling_active = True

while polling_active:

\# 提示输入被调查者的名字和回答

​	name = input("\nWhat is your name? ")

​	response = input("Which mountain would you like to climb 		someday? ")

\# 将答卷存储在字典中

​	responses[name] = response

\# 看看是否还有人要参与调查

​	repeat = input("Would you like to let another person respond? 	(yes/ no) ")

​	if repeat == 'no':

​			polling_active = False

\# 调查结束，显示结果

print("\n--- Poll Results ---") 

for name, response in responses.items():

​		print(name + " would like to climb " + response + ".")	

这个程序首先定义了一个空字典（responses ），并设置了一个标志（polling_active ），用于指出调查是否继续。只要polling_active 为True ，Python就运行while 循环中的代码。

在这个循环中，提示用户输入其用户名及其喜欢爬哪座山。将这些信息存储在字典responses 中，然后询问用户调查是否继续。如果用户输入yes ，程序将再次进入while 循环；如果用户输入no ，标志polling_active 将被设置为False ，而while 循环将就此结束。最后一个代码块显示调查结果。

What is your name? Eric

Which mountain would you like to climb someday? Denali

Would you like to let another person respond? (yes/ no) yes

What is your name? Lynn

Which mountain would you like to climb someday? Devil's Thumb

Would you like to let another person respond? (yes/ no) no

--- Poll Results ---

Lynn would like to climb Devil's Thumb.

Eric would like to climb Denali.