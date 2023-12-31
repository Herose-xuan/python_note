# 第十七章 使用 API

## 17.1 使用 Web  API

Web API是网站的一部分，用于与使用非常具体的URL请求特定信息的程序交互。这种请求称为API调用。请求的数据将以易于处理的格式（如JSON或CSV）返回。依赖于外部数据源的大多数应用程序都依赖于API调用，如集成社交媒体网站的应用程序。

### 17.1.1 Git 和 GitHub

可视化将基于来自GitHub的信息，这是一个让程序员能够协作开发项目的网站。我们将使用GitHub的API来请求有关该网站中Python项目的信息，然后使用Pygal生成交互式可视化，以呈现这些项目的受欢迎程度。

GitHub（https://github.com/ ）的名字源自Git，Git是一个分布式版本控制系统，让程序员团队能够协作开发项目。Git帮助大家管理为项目所做的工作，避免一个人所做的修改影响其他人所做的修改。你在项目中实现新功能时，Git将跟踪你对每个文件所做的修改。确定代码可行后，你提交所做的修改，而Git将记录项目最新的状态。如果你犯了错，想撤销所做的修改，可轻松地返回以前的任何可行状态（要更深入地了解如何使用Git进行版本控制，请参阅附录D）。GitHub上的项目都存储在仓库中，后者包含与项目相关联的一切：代码、项目参与者的信息、问题或bug报告等。

对于喜欢的项目，GitHub用户可给它加星（star）以表示支持，用户还可跟踪他可能想使用的项目。我们将编写一个程序，它自动下载GitHub上星级最高的Python项目的信息，并对这些信息进行可视化。

### 17.1.2 使用API调用请求数据

GitHub的API让你能够通过API调用来请求各种信息。要知道API调用是什么样的，请在浏览器的地址栏中输入如下地址并按回车键：

https://api.github.com/search/repositories?q=language:python&sort=stars

这个调用返回GitHub当前托管了多少个Python项目，还有有关最受欢迎的Python仓库的信息。下面来仔细研究这个调用。第一部分（https://api.github.com/ ）将请求发送到GitHub网站中响应API调用的部分；接下来的一部分（search/repositories ）让API搜索GitHub上的所有仓库。

repositories 后面的问号指出我们要传递一个实参。q 表示查询，而等号让我们能够开始指定查询（q= ）。通过使用language:python ，我们指出只想获取主要语言为Python的仓库的信息。最后一部分（&sort=stars ）指定将项目按其获得的星级进行排序。

下面显示了响应的前几行。从响应可知，该URL并不适合人工输入。

```
{
  "total_count": 8380120,
  "incomplete_results": true,
  "items": [
    {
      "id": 33015583,
      "node_id": "MDEwOlJlcG9zaXRvcnkzMzAxNTU4Mw==",
      "name": "keras",
      "full_name": "keras-team/keras",
      "private": false,
      "owner": {
        "login": "keras-team",
        "id": 34455048,
        "node_id": "MDEyOk9yZ2FuaXphdGlvbjM0NDU1MDQ4",
        "avatar_url": "https://avatars.githubusercontent.com/u/34455048?v=4",
        "gravatar_id": "",
        "url": "https://api.github.com/users/keras-team",
        "html_url": "https://github.com/keras-team",
        "followers_url": "https://api.github.com/users/keras-team/followers",
        "following_url": "https://api.github.com/users/keras-team/following{/other_user}",
        "gists_url": "https://api.github.com/users/keras-team/gists{/gist_id}",
        "starred_url": "https://api.github.com/users/keras-team/starred{/owner}{/repo}",
        "subscriptions_url": "https://api.github.com/users/keras-team/subscriptions",
        "organizations_url": "https://api.github.com/users/keras-team/orgs",
        "repos_url": "https://api.github.com/users/keras-team/repos",
        "events_url": "https://api.github.com/users/keras-team/events{/privacy}",
        "received_events_url": "https://api.github.com/users/keras-team/received_events",
        "type": "Organization",
        "site_admin": false
      }
```

从第二行输出可知，编写本书时，GitHub总共有33015583个Python项目。"incomplete_results" 的值为false ，据此我们知道请求是成功的（它并非不完整的）。倘若GitHub无法全面处理该API，它返回的这个值将为true 。接下来的列表中显示了返回的"items" ，其中包含GitHub上最受欢迎的Python项目的详细信息。

### 17.1.3  安装 requests

requests包让Python程序能够轻松地向网站请求信，息以及检查返回的响应。要安装requests，请执行类似于下面的命令：

$ pip install --user requests

### 17.1.4  处理API响应

下面来编写一个程序，它执行API调用并处理结果，找出GitHub上星级最高的Python项目：

**python_repos.py**

```
import requests
# 执行API调用并存储响应
url = 'https://api.github.com/search/repositories?q=language:python&sort=stars'
r=requests.get(url)
print("Status code:", r.status_code)
# 将API响应存储在一个变量中
response_dict = r.json()
# 处理结果
print(response_dict.keys())
```



我们导入了模块requests 。我们存储API调用的URL，然后使用requests 来执行调用。我们调用get() 并将URL传递给它，再将响应对象存储在变量r 中。响应对象包含一个名为status_code 的属性，它让我们知道请求是否成功了（状态码200表示请求成功）。我们打印status_code ，核实调用是否成功了。

这个API返回JSON格式的信息，因此我们使用方法json() 将这些信息转换为一个Python字典。我们将转换得到的字典存储在response_dict 中。

最后，我们打印response_dict 中的键。输出如下：

Status code: 200

dict_keys(['items', 'total_count', 'incomplete_results'])

状态码为200，因此我们知道请求成功了。响应字典只包含三个键：'items' 、'total_count' 和'incomplete_results' 

**像这样简单的调用应该会返回完整的结果集，因此完全可以忽略与'incomplete_results' 相关联的值。但执行更复杂的API调用时，程序应检查这个值。**

### 17.1.5  处理响应字典

将API调用返回的信息存储到字典中后，就可以处理这个字典中的数据了。下面来生成一些概述这些信息的输出。这是一种不错的方式，可确认收到了期望的信息，进而可以开始研究感兴趣的信息：

#### **python_repos.p**y

```
import requests
# 执行API调用并存储响应
url = 'https://api.github.com/search/repositories?q=language:python&sort=stars'
r=requests.get(url)
print("Status code:", r.status_code)

# 将API响应存储在一个变量中
response_dict = r.json()
print("Total repositories:", response_dict['total_count'])
# 探索有关仓库的信息
repo_dicts = response_dict['items']
print("Repositories returned:", len(repo_dicts))
# 研究第一个仓库
repo_dict = repo_dicts[0]
print("\nKeys:", len(repo_dict))
for key in sorted(repo_dict.keys()):
   print(key)
```

我们打印了与'total_count' 相关联的值，它指出了GitHub总共包含多少个Python仓库。与'items' 相关联的值是一个列表，其中包含很多字典，而每个字典都包含有关一个Python仓库的信息。我们将这个字典列表存储在repo_dicts 中。接下来，我们打印repo_dicts 的长度，以获悉我们获得了多少个仓库的信息。

为更深入地了解返回的有关每个仓库的信息，我们提取了repo_dicts 中的第一个字典，并将其存储在repo_dict 中接下来，我们打印这个字典包含的键数，看看其中有多少信息。我们打印这个字典的所有键，看看其中包含哪些信息。

输出让我们对实际包含的数据有了更清晰的认识：

Status code: 200
Total repositories: 9654868
Repositories returned: 30

Keys: 80
allow_forking
archive_url
archived
assignees_url
blobs_url
branches_url
clone_url
collaborators_url
comments_url
commits_url
compare_url
contents_url
contributors_url
created_at
default_branch
deployments_url
description
disabled
downloads_url
events_url
fork
forks
forks_count
forks_url
full_name
git_commits_url
git_refs_url
git_tags_url
git_url
has_discussions
has_downloads
has_issues
has_pages
has_projects
has_wiki
homepage
hooks_url
html_url
id
is_template
issue_comment_url
issue_events_url
issues_url
keys_url
labels_url
language
languages_url
license
merges_url
milestones_url
mirror_url
name
node_id
notifications_url
open_issues
open_issues_count
owner
private
pulls_url
pushed_at
releases_url
score
size
ssh_url
stargazers_count
stargazers_url
statuses_url
subscribers_url
subscription_url
svn_url
tags_url
teams_url
topics
trees_url
updated_at
url
visibility
watchers
watchers_count
web_commit_signoff_required

GitHub的API返回有关每个仓库的大量信息：repo_dict 包含80个键。通过仔细查看这些键，可大致知道可提取有关项目的哪些信息（要准确地获悉API将返回哪些信息，要么阅读文档，要么像此处这样使用代码来查看这些信息）。

下面来提取repo_dict 中与一些键相关联的值：

```
# 探索有关仓库的信息
repo_dicts = response_dict['items']
print("Repositories returned:", len(repo_dicts))
# 研究第一个仓库
repo_dict = repo_dicts[0]
print("\nSelected information about first repository:")
print('Name:', repo_dict['name'])
print('Owner:', repo_dict['owner']['login'])
print('Stars:', repo_dict['stargazers_count'])
print('Repository:', repo_dict['html_url'])
print('Created:', repo_dict['created_at'])
print('Updated:', repo_dict['updated_at'])
print('Description:', repo_dict['description'])
```

在这里，我们打印了表示第一个仓库的字典中与很多键相关联的值。我们打印了项目的名称。项目所有者是用一个字典表示的，因此，我们使用键owner 来访问表示所有者的字典，再使用键key 来获取所有者的登录名。，我们打印项目获得了多少个星的评级，以及项目在GitHub仓库的URL。接下来，我们显示项目的创建时间和最后一次更新的时间）。最后，我们打印仓库的描述。输出类似于下面这样：

Status code: 200
Total repositories: 9119545
Repositories returned: 30

Selected information about first repository:
Name: youtube-dl
Owner: ytdl-org
Stars: 116540
Repository: https://github.com/ytdl-org/youtube-dl
Created: 2010-10-31T14:35:07Z
Updated: 2023-01-14T03:26:22Z
Description: Command-line program to download videos from YouTube.com and other video sites

Process finished with exit code 0

### 17.1.6 概述最受欢迎的仓库

对这些数据进行可视化时，我们需要涵盖多个仓库。下面就来编写一个循环，打印API调用返回的每个仓库的特定信息，以便能够在可视化中包含所有这些信息：

**python_repos.py**

```
# 探索有关仓库的信息
repo_dicts = response_dict['items']
print("Repositories returned:", len(repo_dicts))

print("\nSelected information about first repository:")
for repo_dict in repo_dicts:
   print('Name:', repo_dict['name'])
   print('Owner:', repo_dict['owner']['login'])
   print('Stars:', repo_dict['stargazers_count'])
   print('Repository:', repo_dict['html_url'])
   print('Description:', repo_dict['description'])
```

，我们打印了一条说明性消息。我们遍历repo_dicts 中的所有字典。在这个循环中，我们打印每个项目的名称、所有者、星级、在GitHub上的URL以及描述：

Status code: 200
Total repositories: 9101208
Repositories returned: 30

Selected information about first repository:
Name: youtube-dl
Owner: ytdl-org
Stars: 116540
Repository: https://github.com/ytdl-org/youtube-dl
Description: Command-line program to download videos from YouTube.com and other video sites
Name: ansible
Owner: ansible
Stars: 55977
Repository: https://github.com/ansible/ansible
Description: Ansible is a radically simple IT automation platform that makes your applications and systems easier to deploy and maintain. Automate everything from code deployment to network configuration to cloud management, in a language that approaches plain English, using SSH, with no agents to install on remote systems. https://docs.ansible.com.
Name: big-list-of-naughty-strings
Owner: minimaxir
Stars: 44348
Repository: https://github.com/minimaxir/big-list-of-naughty-strings
Description: The Big List of Naughty Strings is a list of strings which have a high probability of causing issues when used as user-input data.
Name: pandas
Owner: pandas-dev
Stars: 36521
Repository: https://github.com/pandas-dev/pandas
Description: Flexible and powerful data analysis / manipulation library for Python, providing labeled data structures similar to R data.frame objects, statistical functions, and much more
Name: python-fire
Owner: google
Stars: 23677
Repository: https://github.com/google/python-fire
Description: Python Fire is a library for automatically generating command line interfaces (CLIs) from absolutely any Python object.
Name: CheatSheetSeries
Owner: OWASP
Stars: 22644
Repository: https://github.com/OWASP/CheatSheetSeries
Description: The OWASP Cheat Sheet Series was created to provide a concise collection of high value information on specific application security topics.
Name: numpy
Owner: numpy
Stars: 22453
Repository: https://github.com/numpy/numpy
Description: The fundamental package for scientific computing with Python.
Name: locust
Owner: locustio
Stars: 20497
Repository: https://github.com/locustio/locust
Description: Scalable load testing tool written in Python
Name: vnpy
Owner: vnpy
Stars: 19754
Repository: https://github.com/vnpy/vnpy
Description: 基于Python的开源量化交易平台开发框架
Name: pytorch-CycleGAN-and-pix2pix
Owner: junyanz
Stars: 19030
Repository: https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix
Description: Image-to-Image Translation in PyTorch
Name: magenta
Owner: magenta
Stars: 18119
Repository: https://github.com/magenta/magenta
Description: Magenta: Music and Art Generation with Machine Intelligence
Name: matplotlib
Owner: matplotlib
Stars: 16703
Repository: https://github.com/matplotlib/matplotlib
Description: matplotlib: plotting with Python
Name: wagtail
Owner: wagtail
Stars: 13912
Repository: https://github.com/wagtail/wagtail
Description: A Django content management system focused on flexibility and user experience
Name: gensim
Owner: RaRe-Technologies
Stars: 13859
Repository: https://github.com/RaRe-Technologies/gensim
Description: Topic Modelling for Humans
Name: wechat_jump_game
Owner: wangshub
Stars: 13855
Repository: https://github.com/wangshub/wechat_jump_game
Description: 微信《跳一跳》Python 辅助
Name: python-mini-projects
Owner: Python-World
Stars: 12080
Repository: https://github.com/Python-World/python-mini-projects
Description: A collection of simple python mini projects to enhance your python skills
Name: FastPhotoStyle
Owner: NVIDIA
Stars: 10939
Repository: https://github.com/NVIDIA/FastPhotoStyle
Description: Style transfer, deep learning, feature transform
Name: howdoi
Owner: gleitz
Stars: 9831
Repository: https://github.com/gleitz/howdoi
Description: instant coding answers via the command line
Name: PaddleDetection
Owner: PaddlePaddle
Stars: 9347
Repository: https://github.com/PaddlePaddle/PaddleDetection
Description: Object Detection toolkit based on PaddlePaddle. It supports object detection, instance segmentation, multiple object tracking and real-time multi-person keypoint detection.
Name: dirsearch
Owner: maurosoria
Stars: 9077
Repository: https://github.com/maurosoria/dirsearch
Description: Web path scanner
Name: maskrcnn-benchmark
Owner: facebookresearch
Stars: 9033
Repository: https://github.com/facebookresearch/maskrcnn-benchmark
Description: Fast, modular reference implementation of Instance Segmentation and Object Detection algorithms in PyTorch.
Name: tpot
Owner: EpistasisLab
Stars: 8885
Repository: https://github.com/EpistasisLab/tpot
Description: A Python Automated Machine Learning tool that optimizes machine learning pipelines using genetic programming.
Name: DALLE2-pytorch
Owner: lucidrains
Stars: 8709
Repository: https://github.com/lucidrains/DALLE2-pytorch
Description: Implementation of DALL-E 2, OpenAI's updated text-to-image synthesis neural network,  in Pytorch
Name: DeepSpeed
Owner: microsoft
Stars: 8512
Repository: https://github.com/microsoft/DeepSpeed
Description: DeepSpeed is a deep learning optimization library that makes distributed training and inference easy, efficient, and effective.
Name: practical-python
Owner: dabeaz-course
Stars: 8320
Repository: https://github.com/dabeaz-course/practical-python
Description: Practical Python Programming (course by @dabeaz)
Name: mvt
Owner: mvt-project
Stars: 8278
Repository: https://github.com/mvt-project/mvt
Description: MVT (Mobile Verification Toolkit) helps with conducting forensics of mobile devices in order to find signs of a potential compromise.
Name: calibre-web
Owner: janeczku
Stars: 8271
Repository: https://github.com/janeczku/calibre-web
Description: :books: Web app for browsing, reading and downloading eBooks stored in a Calibre database
Name: fsociety
Owner: Manisso
Stars: 8240
Repository: https://github.com/Manisso/fsociety
Description: fsociety Hacking Tools Pack – A Penetration Testing Framework
Name: django-allauth
Owner: pennersr
Stars: 7689
Repository: https://github.com/pennersr/django-allauth
Description: Integrated set of Django applications addressing authentication, registration, account management as well as 3rd party (social) account authentication.
Name: supervisor
Owner: Supervisor
Stars: 7607
Repository: https://github.com/Supervisor/supervisor
Description: Supervisor process control system for Unix (supervisord)

上述输出中有一些有趣的项目，可能值得再看一眼。但不要在这上面花费太多时间，因为我们即将创建的可视化可让你更容易地看清结果。

### 17.1.7 监视API的速率限制

大多数API都存在速率限制，即你在特定时间内可执行的请求数存在限制。要获悉你是否接近了GitHub的限制，请在浏览器中输入https://api.github.com/rate_limit ，你将看到类似于下面的响应：

```
{
"resources": {
"core": {
"limit": 60,
"remaining": 58,
"reset": 1426082320
},
   "search": { 
         "limit": 10, 
         "remaining": 8,
         "reset": 1426078803
}
         },
         "rate": {
         "limit": 60,
         "remaining": 58,
      "reset": 1426082320
   }
   }
```

我们关心的信息是搜索API的速率限制。极限为每分钟10个请求，而在当前这一分钟内，我们还可执行8个请求。reset 值指的是配额将重置的Unix时间或（1970年1月1日午夜后多少秒）。用完配额后，你将收到一条简单的响应，由此知道已到达API极限。到达极限后，你必须等待配额重置。

## 17.2  使用pygal可视化仓库

有了一些有趣的数据后，我们来进行可视化，呈现GitHub上Python项目的受欢迎程度。我们将创建一个交互式条形图：条形的高度表示项目获得了多少颗星。单击条形将带你进入项目在GitHub上的主页。下面是首次尝试这样做：

**python_repos.py**

```
import requests
import  pygal
from pygal.style import LightColorizedStyle as LCS, LightenStyle as LS

# 执行API调用并存储响应
url = 'https://api.github.com/search/repositories?q=language:python&sort=stars'
r=requests.get(url)
print("Status code:", r.status_code)

# 将API响应存储在一个变量中
response_dict = r.json()
print("Total repositories:", response_dict['total_count'])
# 探索有关仓库的信息
repo_dicts = response_dict['items']
names, stars = [], []
for repo_dict in repo_dicts:
   names.append(repo_dict['name'])
   stars.append(repo_dict['stargazers_count'])

# 可视化
my_style = LS('#333366', base_style=LCS)
chart = pygal.Bar(style=my_style, x_label_rotation=45, show_legend=False)
chart.title = 'Most-Starred Python Projects on GitHub'
chart.x_labels = names
chart.add('', stars)
chart.render_to_file('data\\python_repos.svg')
```

我们首先导入了pygal 以及要应用于图表的Pygal样式。接下来，打印API调用响应的状态以及找到的仓库总数，以便获悉API调用是否出现了问题。我们不再打印返回的有关项目的信息，因为将通过可视化来呈现这些信息。

我们创建了两个空列表，用于存储将包含在图表中的信息。我们需要每个项目的名称，用于给条形加上标签，我们还需要知道项目获得了多少个星，用于确定条形的高度。在循环中，我们将项目的名称和获得的星数附加到这些列表的末尾。

接下来，我们使用LightenStyle 类（别名LS ）定义了一种样式，并将其基色设置为深蓝色。我们还传递了实参base_style ，以使用LightColorizedStyle 

类（别名LCS ）。然后，我们使用Bar() 创建一个简单的条形图，并向它传递了my_style。我们还传递了另外两个样式实参：让标签绕*x* 轴旋转45度（x_label_rotation=45 ），并隐藏了图例（show_legend=False ），因为我们只在图表中绘制一个数据系列。接下来，我们给图表指定了标题，并将属性x_labels 设置为列表names 由于我们不需要给这个数据系列添加标签，因此添加数据时，将标签设置成了空字符串。前几个项目的受欢迎程度比其他项目高得多，但所有这些项目在Python生态系统中都很重要。

### 17.2.1 改进pygal图表

下面来改进这个图表的样式。我们将进行多个方面的定制，因此先来稍微调整代码的结构，创建一个配置对象，在其中包含要传递给Bar() 的所有定制：

**python_repos.py**

```
# 可视化

my_style = LS('#333366', base_style=LCS)
my_config = pygal.Config()
my_config.x_label_rotation = 45
my_config.show_legend = False
my_config.title_font_size = 24
my_config.label_font_size = 14
my_config.major_label_font_size = 18
my_config.truncate_label = 15
my_config.show_y_guides = False
my_config.width = 1000

chart = pygal.Bar(my_config, style=my_style)
chart.title = 'Most-Starred Python Projects on GitHub'
chart.x_labels = names
chart.add('', stars)
chart.render_to_file('data\\python_repos2.svg')
```

我们创建了一个Pygal类Config 的实例，并将其命名为my_config 。通过修改my_config 的属性，可定制图表的外观。我们设置了两个属性 ——x_label_rotation 和show_legend ，它们原来是在创建Bar 实例时以关键字实参的方式传递的。我们设置了图表标题、副标签和主标签的字体大小。在这个图表中，副标签是 *x* 轴上的项目名以及 *y* 轴上的大部分数字。主标签是 *y* 轴上为5000整数倍的刻度；这些标签应更大，以与副标签区分开来。我们使truncate_label 将较长的项目名缩短为15个字符（如果你将鼠标指向屏幕上被截短的项目名，将显示完整的项目名）。接下来，我们将show_y_guides 设置为False ，以隐藏图表中的水平线。最后，设置了自定义宽度，让图表更充分地利用浏览器中的可用空间。

创建Bar 实例时，我们将my_config 作为第一个实参，从而通过一个实参传递了所有的配置设置。我们可以通过my_config 做任意数量的样式和配置修改，代码行将保持不变。

### 17.2.2  添加自定义工具提示

在Pygal中，将鼠标指向条形将显示它表示的信息，这通常称为工具提示。在这个示例中，当前显示的是项目获得了多少个星。下面来创建一个自定义工具提示，以同时显示项目的描述。

来看一个简单的示例，它可视化前三个项目，并给每个项目对应的条形都指定自定义标签。为此，我们向add() 传递一个字典列表，而不是值列表：

**bar_descriptions.py**

```
import pygal
from pygal.style import LightColorizedStyle as LCS, LightenStyle as LS
my_style = LS('#333366', base_style=LCS)
chart = pygal.Bar(style=my_style, x_label_rotation=45, show_legend=False)
chart.title = 'Python Projects'
chart.x_labels = ['httpie', 'django', 'flask']
plot_dicts = [
         {'value': 16101, 'label': 'Description of httpie.'},
         {'value': 15028, 'label': 'Description of django.'},
         {'value': 14798, 'label': 'Description of flask.'},
         ]
chart.add('', plot_dicts)
chart.render_to_file('data\\bar_descriptions.svg')
```

我们定义了一个名为plot_dicts 的列表，其中包含三个字典，分别针对项目HTTPie、Django和Flask。每个字典都包含两个键：'value' 和'label' 。Pygal根据与键'value' 相关联的数字来确定条形的高度，并使用与'label' 相关联的字符串给条形创建工具提示。例如，第一个字典将创建一个条形，用于表示一个获得了16 101颗星、工具提示为Description of httpie的项目。

方法add() 接受一个字符串和一个列表。这里调用add() 时，我们传入了一个由表示条形的字典组成的列表（plot_dicts ）一个工具提示：除默认工具提示（获得的星数）外，Pygal还显示了我们传入的自定义提示。

### 17.2.3  根据数据绘图

为根据数据绘图，我们将自动生成plot_dicts ，其中包含API调用返回的30个项目的信息。

完成这种工作的代码如下：

**python_repos.py**

```
# 探索有关仓库的信息
repo_dicts = response_dict['items']
print("Number of items:", len(repo_dicts))
names, plot_dicts = [], []
for repo_dict in repo_dicts:
   names.append(repo_dict['name'])
   plot_dict = {
      'value': repo_dict['stargazers_count'],
      'label': repo_dict['description'],
   }
   plot_dicts.append(plot_dict)

# 可视化

my_style = LS('#333366', base_style=LCS)
my_config = pygal.Config()
my_config.x_label_rotation = 45
my_config.show_legend = False
my_config.title_font_size = 24
my_config.label_font_size = 14
my_config.major_label_font_size = 18
my_config.truncate_label = 15
my_config.show_y_guides = False
my_config.width = 1000

chart = pygal.Bar(my_config, style=my_style)
chart.title = 'Most-Starred Python Projects on GitHub'
chart.x_labels = names
chart.add('', plot_dicts)
chart.render_to_file('data\\python_repos3.svg')
```

我们创建了两个空列表names 和plot_dicts 。为生成*x* 轴上的标签，我们依然需要列表names 。

在循环内部，对于每个项目，我们都创建了字典plot_dict 。在这个字典中，我们使用键'value' 存储了星数，并使用键'label' 存储了项目描述。接下来，我们将字典plot_dict 附加到plot_dicts 末尾。我们将列表plot_dicts 传递给了add() 。

### 17.2.4  在图表中添加可单击的链接

Pygal还允许你将图表中的每个条形用作网站的链接。为此，只需添加一行代码，在为每个项目创建的字典中，添加一个键为'xlink' 的键—值对：

**python_repos.py**

```
# 探索有关仓库的信息
repo_dicts = response_dict['items']
print("Number of items:", len(repo_dicts))
names, plot_dicts = [], []
for repo_dict in repo_dicts:
   names.append(repo_dict['name'])
   plot_dict = {
      'value': repo_dict['stargazers_count'],
      'label': repo_dict['description'],
      'xlink': repo_dict['html_url'],
   }
   plot_dicts.append(plot_dict)
```

Pygal根据与键'xlink' 相关联的URL将每个条形都转换为活跃的链接。单击图表中的任何条形时，都将在浏览器中打开一个新的标签页，并在其中显示相应项目的GitHub页面。

至此，你对API获取的数据进行了可视化，它是交互性的，包含丰富的信息！

## 17.3   **Hacker News API**

为探索如何使用其他网站的API调用，我们来看看Hacker News（

http://news.ycombinator.com/ ）。在Hacker News网站，用户分享编程和技术方面的文章，并就这些文章展开积极的讨论。Hacker News的API让你能够访问有关该网站所有文章和评论的信息，且不要求你通过注册获得密钥。



https://hacker-news.firebaseio.com/v0/item/9884165.json

响应是一个字典，包含ID为9884165的文章的信息：

```
{
"id":9884165,
"kids":[9885099,9884723,9885165,9884789,9885604,9884137,9886151,9885220,9885790,9884661,9885844,9885029,9884817,9887342,9884545,9884372,9884499,9884881,9884109,9886496,9884342,9887832,9885023,9884334,9884707,9887008,9885348,9885131,9887539,9885880,9884196,9884640,9886534,9885152],
"score":558,"time":1436875181,
"title":"New Horizons: Nasa spacecraft speeds past Pluto"，
"type":"story",
"url":"http://www.bbc.co.uk/news/science-environment-33524589"}
```

这个字典包含很多键，如'url' 和'title'。与键'descendants' 相关联的值是文章被评论的次数。与键'kids' 相关联的值包含对文章所做的所有评论的ID。每个评论自己也可能有kid，因此文章的后代（descendant）数量可能比其kid数量多。

下面来执行一个API调用，返回Hacker News上当前热门文章的ID，再查看每篇排名靠前的文章：

**hn_submissions.py**

```
import requests
from operator import itemgette
# 执行API调用并存储响应
url = 'https://hacker-news.firebaseio.com/v0/topstories.json'
r = requests.get(url)
print("Status code:", r.status_code)
# 处理有关每篇文章的信息
submission_ids = r.json()
submission_dicts = []
for submission_id in submission_ids[:30]:
   # 对于每篇文章，都执行一个API调用
   url = ('https://hacker-news.firebaseio.com/v0/item/' +
          str(submission_id) + '.json')
   submission_r = requests.get(url)
   print(submission_r.status_code)
   response_dict = submission_r.json()
   submission_dict = {
      'title': response_dict['title'],
      'link': 'http://news.ycombinator.com/item?id=' + str(submission_id),
      'comments': response_dict.get(
      'descendants', 0)
   }
   submission_dicts.append(submission_dict)
   submission_dicts = sorted(submission_dicts, key=itemgetter('comments'),
                             reverse=True)
   for submission_dict in submission_dicts:
      print("\nTitle:", submission_dict['title'])
   print("Discussion link:", submission_dict['link'])
   print("Comments:", submission_dict['comments'])
```

首先，我们执行了一个API调用，并打印了响应的状态。这个API调用返回一个列表，其中包含Hacker News上当前最热门的500篇文章的ID。接下来，我们将响应文本转换为一个Python列表，并将其存储在submission_ids 中。我们将使用这些ID来创建一系列字典，其中每个字典都存储了一篇文章的信息。

，我们创建了一个名为submission_dicts 的空列表，用于存储前面所说的字典。接下来，我们遍历前30篇文章的ID。对于每篇文章，我们都执行一个API调用，其中的URL包含submission_id 的当前值。我们打印每次请求的状态，以便知道请求是否成功了。

我们为当前处理的文章创建一个字典，并在其中存储文章的标题以及到其讨论页面的链接。我们在这个字典中存储了评论数。如果文章还没有评论，响应字典

中将没有键'descendants' 。不确定某个键是否包含在字典中时，可使用方法dict.get() ，它在指定的键存在时返回与之相关联的值，并在指定的键不存在时返回你指定的值（这里是0）。最后，我们将submission_dict 附加到submission_dicts 末尾。

Hacker News上的文章是根据总体得分排名的，而总体得分取决于很多因素，其中包含被推荐的次数、评论数以及发表的时间。我们要根据评论数对字典列

表submission_dicts 进行排序，为此，使用了模块operator 中的函数itemgetter() 。我们向这个函数传递了键'comments' ，因此它将从这个列表的每个字典中提取与键'comments' 相关联的值。这样，函数sorted() 将根据这种值对列表进行排序。我们将列表按降序排列，即评论最多的文章位于最前面。

对列表排序后，我们遍历这个列表，对于每篇热门文章，都打印其三项信息：标题、到讨论页面的链接以及文章现有的评论数：

Status code: 200

200

200

200

--snip--

Title: Firefox deactivates Flash by default

Discussion link: http://news.ycombinator.com/item?id=9883246

Comments: 231

Title: New Horizons: Nasa spacecraft speeds past Pluto

Discussion link: http://news.ycombinator.com/item?id=9884165

Comments: 142

Title: Iran Nuclear Deal Is Reached With World Powers

Discussion link: http://news.ycombinator.com/item?id=9884005

Comments: 141

Title: Match Group Buys PlentyOfFish for $575M

Discussion link: http://news.ycombinator.com/item?id=9884417

Comments: 75

Title: Our Nexus 4 devices are about to explode

Discussion link: http://news.ycombinator.com/item?id=9885625

Comments: 14

--snip--

使用任何API来访问和分析信息时，流程都与此类似。有了这些数据后，你就可以进行可视化，指出最近哪些文章引发了最激烈的讨论。
