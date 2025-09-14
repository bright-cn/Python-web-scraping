# Python 网页抓取指南

[![推广](https://github.com/bright-cn/LinkedIn-Scraper/blob/main/Proxies%20and%20scrapers%20GitHub%20bonus%20banner.png)](https://www.bright.cn/products/web-scraper) 

在这个 Python 网页抓取仓库中，你将找到入门所需的一切内容。我们将讲解网页抓取的工作原理、深入了解在 Python 中的多种实现方式，并在最后给出完整示例。

由于语法简洁且拥有大量开源库，Python 被广泛认为是[最适合网页抓取的语言之一](https://www.bright.cn/blog/web-data/best-languages-web-scraping)。现在就来深入了解吧！

## 目录
- [Python 中的网页抓取逻辑](#web-scraping-logic-in-python)
- [Python 网页抓取库](#python-web-scraping-libraries)
   * [HTTP 客户端](#http-clients)
   * [HTML 解析器](#html-parsers)
   * [浏览器自动化](#browser-automation)
   * [一体化抓取](#all-in-one-scraping)
- [Python 常见抓取技术栈](#common-stacks-for-scraping-in-python)
- [前置条件](#prerequisites)
- [使用 Requests 与 Beautiful Soup 进行网页抓取](#web-scraping-with-requests-and-beautiful-soup)
   * [功能](#features)
      + [Requests](#requests)
      + [Beautiful Soup](#beautiful-soup)
   * [安装](#setup)
   * [方法](#methods)
      + [连接到网页](#connect-to-a-web-page)
      + [解析 HTML 字符串](#parse-an-html-string)
      + [基本的节点选择与数据提取方法](#basic-node-selection-and-data-extraction-methods)
      + [查找节点](#find-nodes)
      + [选择节点](#select-nodes)
- [导出抓取的数据](#export-the-scraped-data)
   * [导出为 CSV](#csv-export)
   * [导出为 JSON](#json-export)
- [Python 网页抓取示例](#web-scraping-examples-in-python)
   * [1. Requests + Beautiful Soup](#1-requests-beautiful-soup)
   * [2. Selenium](#2-selenium)
   * [Scrapy](#scrapy)
- [Python 网页抓取的挑战](#challenges-of-python-web-scraping)
- [使用 Web Scraper API 简化抓取](#simplified-web-scraping-with-web-scraper-api)

## Python 中的网页抓取逻辑
任何[Python 网页抓取](https://brightdata.com/blog/how-tos/web-scraping-with-python)脚本的主要构成步骤为：
1. 获取目标页面的 HTML。
2. 将 HTML 解析为 Python 对象。
3. 从已解析的 HTML 中提取数据。
4. 将提取的数据导出为人类可读的格式，如 CSV 或 JSON。

前两步的实现取决于目标页面是静态还是动态：
- 静态站点：使用 [Requests](https://requests.readthedocs.io/en/latest/) 等 HTTP 客户端直接向服务器请求 HTML。收到响应后，用 [Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) 等 HTML 解析器解析。
- 动态站点：使用浏览器自动化工具（如 [Selenium](https://selenium-python.readthedocs.io/)）在无头浏览器中加载页面。这样可以在解析前渲染动态内容。

第 3 步中，数据提取的高层逻辑取决于页面的 DOM 结构，但具体实现会根据你所选工具及其提供的节点选择与数据提取方法而有所不同。

最后，别忘了还有 [Scrapy](https://scrapy.org/) 等一体化抓取工具。这类库通过将上述四步整合到同一平台，极大简化并规范了抓取流程。

## Python 网页抓取库
了解一些用于 Python 网页抓取的高实用性库。更多完整清单可参考我们的 [Awesome Web Scraping 仓库](https://github.com/bright-cn/Awesome-Web-Scraping/blob/main/python.md)。

### HTTP 客户端
- [`requests`](https://github.com/kennethreitz/requests)：简单而优雅的 HTTP 库。
- [`httpx`](https://github.com/encode/httpx)：下一代 Python HTTP 客户端。
- [`aiohttp`](https://github.com/aio-libs/aiohttp)：基于 `asyncio` 的异步 HTTP 客户端/服务端框架。

### HTML 解析器
- [`beautifulsoup4`](https://www.crummy.com/software/BeautifulSoup/)：为屏幕抓取而设计的 HTML 解析程序。
- [`lxml`](https://github.com/lxml/lxml/)：功能丰富、易用的 XML/HTML 处理库。
- [`html5lib`](https://github.com/html5lib/html5lib-python)：符合标准的 Python HTML 解析与序列化库。

### 浏览器自动化
- [`selenium`](https://github.com/SeleniumHQ/selenium)：浏览器自动化框架与生态。
- [`playwright`](https://github.com/microsoft/playwright-python)：Playwright 测试与自动化库的 Python 版本。
- [`pyppeteer`](https://github.com/pyppeteer/pyppeteer)：Headless Chrome/Chromium 自动化库（Puppeteer 的非官方移植）。

### 一体化抓取
- [`scrapy`](https://github.com/scrapy/scrapy)：快速的高级网页爬取与抓取框架。
- [`autoscraper`](https://github.com/alirezamika/autoscraper)：智能、自动、快速且轻量的 Python 抓取工具。
- [`requests-html`](https://github.com/psf/requests-html)：旨在让 HTML 解析（如网页抓取）更简单直观的库。

## Python 常见抓取技术栈
下表比较三种最常见的 Python 网页抓取技术栈：
|                               | **Requests + Beautiful Soup**                                              | **Selenium**                                                                 | **Scrapy**                                                                                                                      |
| ----------------------------- | ---------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| **描述**                      | 使用 Requests 获取网页 HTML，使用 Beautiful Soup 解析并提取数据              | 自动化浏览器，与动态网站交互、抓取数据并模拟用户行为                          | 用于大规模网页抓取任务的强大框架                                                                                                |
| **依赖**                      | - `requests`<br>- `beautifulsoup4`                                          | - `selenium`<br>- Chrome/Firefox/Edge 等浏览器                               | - `scrapy`                                                                                                                      |
| **静态页面支持**              | 是                                                                           | 是                                                                            | 是                                                                                                                              |
| **动态页面支持**              | 否                                                                           | 是                                                                            | 默认不支持，但可通过 [Scrapy Splash 插件](https://github.com/scrapy-plugins/scrapy-splash) 实现                                  |
| **最适合的场景**              | 简单抓取任务                                                                 | 需要用户交互的动态网站抓取                                                    | 涉及网页爬取的大型抓取项目                                                                                                      |

接下来将演示如何使用 Requests + Beautiful Soup 进行网页抓取，这是 Python 中最常见的方法。

## 前置条件
要在 Python 中进行网页抓取，你需要：
- 在你的机器上安装 Python 3+。
- 使用[虚拟环境](https://docs.python.org/3/library/venv.html)搭建一个 Python 项目，并在其中安装所需抓取库。

此外，使用带 Python 插件的 [Visual Studio Code](https://code.visualstudio.com/docs/languages/python) 或 [PyCharm](https://www.jetbrains.com/pycharm/) 等 Python IDE 会让你更轻松地编写与管理代码。

## 使用 Requests 与 Beautiful Soup 进行网页抓取
现在让我们使用 Requests 作为 HTTP 客户端，结合 Beautiful Soup 开始网页抓取。

注意：你可以很容易地将下述示例扩展到基于 Selenium 或 Scrapy 的网页抓取。

专门教程可参考我们的[Beautiful Soup 网页抓取指南](https://www.bright.cn/blog/how-tos/beautiful-soup-web-scraping)。

### 功能
了解 Requests 与 Beautiful Soup 提供的主要能力。

#### Requests
- 支持任意方法的 HTTP 请求（`GET`、`POST`、`PUT`、`DELETE`、`PATCH`、`HEAD`、`OPTIONS`）。
- 轻松处理 HTTP 头、Cookie 与查询参数。
- 支持 SSL/TLS 校验，保障连接安全。
- 根据服务器提供的编码自动解码响应内容。
- 内置会话管理，便于维持 Cookie 与认证状态。
- 针对 HTTP 错误（[`4xx` 与 `5xx`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)）的简化错误处理。

#### Beautiful Soup
- 将 HTML 与 XML 文档解析为 Python 可读格式。
- 提供通过标签、属性与文本搜索元素的方法。
- 支持多种解析器，包括快速的内置解析器 [`html.parser`](https://docs.python.org/3/library/html.parser.html) 和外部解析器 `lxml`。
- 能够优雅地处理格式不佳或损坏的 HTML。
- 便于导航（与修改）已解析的 HTML 内容。
- 与 Requests 以及任意[Python HTTP 客户端](https://www.bright.cn/blog/web-data/best-python-http-clients)无缝集成，支持完整抓取流程。

### 安装
在已激活的虚拟环境中安装 Requests 与 Beautiful Soup：
```bash
pip install requests beautifulsoup4
```
然后在代码中导入它们：
```python
import requests
from bs4 import BeautifulSoup
```

### 方法
以下演示使用 Requests 与 Beautiful Soup 进行 Python 网页抓取的常见操作。

#### 连接到网页
使用 Requests 暴露的 [`get()`](https://requests.readthedocs.io/en/latest/api/#requests.get) 方法获取网页 HTML：
```python
url = "https://en.wikipedia.org/wiki/Web_scraping"
response = requests.get(url)
```
将 `url` 替换为你的目标页面地址。

在幕后，Requests 会对该 URL 发起 HTTP `GET` 请求。网站服务器返回包含页面 HTML 的响应对象。打印 `response` 验证一下：
```python
print(response)
```
输出应类似于：
```
<Response [200]>
```
[`200`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/200) 状态码表示请求成功。

从响应对象中提取 HTML 内容，访问其 `text` 属性：
```python
print(response.text)
```
这会打印页面的 HTML 内容：
```html
<!DOCTYPE html>
<html
  class="client-nojs vector-feature-language-in-header-enabled vector-feature-language-in-main-page-header-disabled vector-feature-sticky-header-disabled vector-feature-page-tools-pinned-disabled vector-feature-toc-pinned-clientpref-1 vector-feature-main-menu-pinned-disabled vector-feature-limited-width-clientpref-1 vector-feature-limited-width-content-enabled vector-feature-custom-font-size-clientpref-1 vector-feature-appearance-pinned-clientpref-1 vector-feature-night-mode-enabled skin-theme-clientpref-day vector-toc-available"
  lang="en"
  dir="ltr"
>
  <head>
    <meta charset="UTF-8">
    <title>Web scraping - Wikipedia</title>
    <!-- 省略... -->
  </head>
</html>
```
输出是包含页面原始 HTML 的字符串。

#### 解析 HTML 字符串
可将 HTML 字符串传给 Beautiful Soup 构造函数进行解析：
```python
soup = BeautifulSoup(html, "html.parser")
```
第一个参数是包含原始 HTML 字符串的变量；第二个参数指定解析器，这里使用 Python 标准库提供的默认 HTML 解析器 `html.parser`。

得到的 `soup` 对象提供了选择 HTML 节点、修改 DOM、访问和处理所选节点内数据的方法与属性。

#### 基本的节点选择与数据提取方法
获取页面的 `<title>` 元素：
```python
print(soup.title)

# 输出:
# <title>Web scraping - Wikipedia</title>
```
要提取 `<title>` 元素内的文本，使用 `text` 属性：
```python
print(soup.title.text)

# 输出:
# Web scraping - Wikipedia
```
同样，你可以这样访问 `<h1>` 元素：
```python
print(soup.h1)

# 输出:
# <h1 class="firstHeading mw-first-heading" id="firstHeading">
#   <span class="mw-page-title-main">Web scraping</span>
# </h1>
```
要获取某个 HTML 属性值（例如 `id`），像字典键一样访问该属性名：
```python
print(soup.h1["id"])

# 输出:
# firstHeading
```

#### 查找节点
Beautiful Soup 中两个最常用的查找 HTML 元素的方法是[`find()` 与 `find_all()`](https://www.crummy.com/software/BeautifulSoup/bs4/doc/#calling-a-tag-is-like-calling-find-all)。它们可以在 HTML 文档中定位特定元素或元素集合。

快速概览：
- `find()`：找到第一个匹配的元素。
- `find_all()`：返回所有匹配元素的列表。

`find()` 的签名如下：
```python
find(name=None, attrs={}, recursive=True, text=None, **kwargs)
```
这表明 `find()` 可以通过标签名、属性或文本内容进行搜索。要按类名或 ID 查找，`**kwargs` 参数让筛选更容易。

例如，获取 `id="firstHeading"` 的 `<h1>` 元素：
```python
h1 = soup.find("h1", id="firstHeading")
print(h1)

# 输出:
# <h1 class="firstHeading mw-first-heading" id="firstHeading">
#   <span class="mw-page-title-main">Web scraping</span>
# </h1>
```
尽管 `id` 不是 `find()` 的显式参数，但由于 `**kwargs` 的灵活性，上述写法依然生效。

如果你需要获取某一类元素的全部实例（如所有标题或链接），`find_all()` 更合适。例如，获取所有 `<a>` 标签：
```python
links = soup.find_all("a")
print(links)

# 输出:
# [
#     <a class="mw-jump-link" href="#bodyContent">Jump to content</a>,
#     <a accesskey="z" href="/wiki/Main_Page" title="Visit the main page [z]">
#         <span>Main page</span>
#     </a>,
#     <a href="/wiki/Wikipedia:Contents" title="Guides to browsing Wikipedia">
#         <span>Contents</span>
#     </a>,
#     省略...
# ]
```
该方法返回匹配元素的列表，便于遍历并执行操作，例如提取文本内容：
```python
links = soup.find_all("a")
for link in links:
  print(link.text)

# 输出:
# Jump to content
# Main page
# Contents
# 省略...
```
你也可以通过标签名与属性过滤组合来缩小范围。例如，查找所有 `class="mw-editsection"` 的 `<span>` 标签：
```python
specific_spans = soup.find_all("span", {"class": "mw-editsection"})
```
或者等价地：
```python
specific_spans = soup.find_all("span", class_="mw-editsection")
```
由于 `class` 是 Python 的保留字，因此不能直接作为参数名使用，而应使用 `class_`。

两种方式都会返回满足条件的 `<span>` 元素列表：
```
[
    <span class="mw-editsection">
        <span class="mw-editsection-bracket">[</span>
        <a href="/w/index.php?title=Web_scraping&amp;action=edit&amp;section=1" title="Edit section: History">
            <span>edit</span>
        </a>
        <span class="mw-editsection-bracket">]</span>
    </span>,
    <span class="mw-editsection">
        <span class="mw-editsection-bracket">[</span>
        <a href="/w/index.php?title=Web_scraping&amp;action=edit&amp;section=2" title="Edit section: Techniques">
            <span>edit</span>
        </a>
        <span class="mw-editsection-bracket">]</span>
    </span>,
    <!-- 省略... -->
]
```
注意，被 `find()` 或 `find_all()` 返回的对象自身也暴露这两个方法。当在结果节点上调用它们时，查找范围会限定在该节点的子节点内。
例如，按类名选择包含参考文献的 HTML 元素：
```python
references_element = soup.find(class_="references")
```
然后获取其中所有 `<li>` 标签并打印内容：
```python
list_items = references_element.find_all("li")
for item in list_items:
    print(item.text)

# 输出:
# ^ Thapelo, Tsaone Swaabow; Namoshe, Molaletsa; Matsebe, Oduetse; Motshegwa, Tshiamo; Bopape, Mary-Jane Morongwa (2021-07-28). "SASSCAL WebSAPI: A Web Scraping Application Programming Interface to Support Access to SASSCAL's Weather Data". Data Science Journal. 20: 24. doi:10.5334/dsj-2021-024. ISSN 1683-1470. S2CID 237719804.
# ^ "Search Engine History.com". Search Engine History. Retrieved November 26, 2019.
# ^ Song, Ruihua; Microsoft Research (Sep 14, 2007). "Joint optimization of wrapper generation and template detection" (PDF). Proceedings of the 13th ACM SIGKDD international conference on Knowledge discovery and data mining. p. 894. doi:10.1145/1281192.1281287. ISBN 9781595936097. S2CID 833565. Archived from the original (PDF) on October 11, 2016.
# 省略...
```

#### 选择节点
Beautiful Soup 中另一个用于定位元素的有用方法是 [`select()`](https://www.crummy.com/software/BeautifulSoup/bs4/doc/#css-selectors-through-the-css-property)。它支持使用[CSS 选择器](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_selectors)查找元素，能基于标签、类名、ID 和其他属性以强大而灵活的方式定位目标。

例如，获取 `.references` 节点内的所有 `<li>` 元素：
```python
li_elements = soup.select(".references > li")
print(len(li_elements))

# 输出:
# 31
```

## 导出抓取的数据
完成对目标页面的数据抓取后，通常会得到一个包含提取结果的字典列表。比如，假设你的 `titles` 字典列表包含如下数据：
```
[
    {'tag': 'h1', 'title': 'Web scraping'},
    {'tag': 'h2', 'title': 'Contents'},
    {'tag': 'h2', 'title': 'History'},
    {'tag': 'h2', 'title': 'Techniques'},
    {'tag': 'h2', 'title': 'Legal issues'},
    {'tag': 'h2', 'title': 'Methods to prevent web scraping'},
    # ...
    {'tag': 'h3', 'title': 'India'}
]
```
通常你会希望把这些数据导出为 CSV 或 JSON 等人类可读格式，以便团队成员轻松访问和使用。下面演示如何导出为这两种格式。

注意：以下导出逻辑适用于未使用 Scrapy 等一体化抓取方案的脚本。因为这类方案通常内置了[通过特定配置直接导出为目标格式的功能](https://docs.scrapy.org/en/latest/topics/feed-exports.html)。

### 导出为 CSV
要导出为 CSV，可以使用 Python 内置的 [`csv`](https://docs.python.org/3/library/csv.html) 模块。它可以将数据写入 `.csv` 文件，其中字典中的每个键对应一列表头，值对应相应的数据行。

首先使用 [`open()`](https://docs.python.org/3/library/functions.html#open) 函数打开名为 `titles.csv` 的文件以写入：
```python
with open("titles.csv", mode="w", newline="", encoding="utf-8") as file:
```
其中 `"w"` 表示写入，`newline=""` 可避免行间出现多余空行。注意使用了[`with` 语句](https://docs.python.org/3/reference/compound_stmts.html#with)，可确保操作完成后文件被正确关闭。

接着，使用 [`DictWriter`](https://docs.python.org/3/library/csv.html#csv.DictWriter) 写入表头（对应字典键）：
```python
writer = csv.DictWriter(file, fieldnames=["tag", "title"])
writer.writeheader()
```
随后通过遍历字典列表，使用 `writer.writerow()` 写入数据：
```python
for row in titles:
    writer.writerow(row)
```
上述逻辑会写入整行，其中键对应列，值为具体数据。

最后，`with` 语句会在写入完成后自动关闭文件。

输出类似于：
```csv
tag,title
h1,Web scraping
h2,Contents
h2,History
h2,Techniques
h2,Legal issues
h2,Methods to prevent web scraping
h2,See also
h2,References
h3,Human copy-and-paste
h3,Text pattern matching
h3,HTTP programming
h3,HTML parsing
h3,DOM parsing
h3,Vertical aggregation
h3,Semantic annotation recognizing
h3,Computer vision web-page analysis
h3,AI-powered document understanding
h3,United States
h3,European Union
h3,Australia
h3,India
```
你可以在仓库中的 `titles.csv` 文件里看到相同结果。

### 导出为 JSON
要导出为 JSON，可使用 Python 内置的 [`json`](https://docs.python.org/3/library/json.html) 模块，它可以将 Python 字典转换为 JSON 格式字符串并写入 `.json` 文件。

先用 `open()` 打开名为 `titles.json` 的文件以写入：
```python
with open("titles.json", mode="w", encoding="utf-8") as file:
```
同样，`with` 语句会在操作完成后自动关闭文件。

接下来使用 `json.dump()` 将字典转换为 JSON 并写入文件：
```python
json.dump(titles, file, indent=4)
```
`indent=4` 表示以 4 个空格缩进进行美化打印，便于阅读。

输出类似于：
```json
[
    {
        "tag": "h1",
        "title": "Web scraping"
    },
    {
        "tag": "h2",
        "title": "Contents"
    },
    {
        "tag": "h2",
        "title": "History"
    },
    {
        "tag": "h2",
        "title": "Techniques"
    },
    {
        "tag": "h2",
        "title": "Legal issues"
    },
    {
        "tag": "h2",
        "title": "Methods to prevent web scraping"
    },
    {
        "tag": "h2",
        "title": "See also"
    },
    {
        "tag": "h2",
        "title": "References"
    },
    {
        "tag": "h3",
        "title": "Human copy-and-paste"
    },
    {
        "tag": "h3",
        "title": "Text pattern matching"
    },
    {
        "tag": "h3",
        "title": "HTTP programming"
    },
    {
        "tag": "h3",
        "title": "HTML parsing"
    },
    {
        "tag": "h3",
        "title": "DOM parsing"
    },
    {
        "tag": "h3",
        "title": "Vertical aggregation"
    },
    {
        "tag": "h3",
        "title": "Semantic annotation recognizing"
    },
    {
        "tag": "h3",
        "title": "Computer vision web-page analysis"
    },
    {
        "tag": "h3",
        "title": "AI-powered document understanding"
    },
    {
        "tag": "h3",
        "title": "United States"
    },
    {
        "tag": "h3",
        "title": "European Union"
    },
    {
        "tag": "h3",
        "title": "Australia"
    },
    {
        "tag": "h3",
        "title": "India"
    }
]
```
你可以在仓库中的 `titles.json` 文件里看到相同结果。

## Python 网页抓取示例
假设你想从[“Web Scraping” 维基百科页面](https://en.wikipedia.org/wiki/Web_scraping)抓取所有 `<hX>`（其中 `X` 为 `1`、`2`、`3`、`4` 或 `5`）标题元素，并导出为 CSV。下面展示三种实现：
1. Requests + Beautiful Soup
2. Selenium
3. Scrapy

开始动手吧！

### 1. Requests + Beautiful Soup
```python
import requests
from bs4 import BeautifulSoup
import csv

# 要抓取的页面 URL
url = "https://en.wikipedia.org/wiki/Web_scraping"

# 发送 GET 请求并获取响应
response = requests.get(url)

# 获取页面 HTML 内容
html = response.text

# 使用 BeautifulSoup 解析 HTML
soup = BeautifulSoup(html, "html.parser")

# 用于存储抓取到的标题
titles = []

# 标题级别列表（h1, h2, h3, h4, h5）
title_level_list = [1, 2, 3, 4, 5]

# 遍历每个标题级别
for title_level in title_level_list:
    # 查找当前标题级别的所有元素
    title_elements = soup.find_all(f"h{title_level}")

    # 遍历每个标题元素
    for title_element in title_elements:
        # 数据提取逻辑
        tag = title_element.name
        text = title_element.text

        # 构建包含标签与标题文本的字典
        title = {
            "tag": tag,
            "title": text,
        }

        # 追加到列表
        titles.append(title)

# 打开 CSV 文件写入数据
with open("titles.csv", mode="w", newline="", encoding="utf-8") as file:
    # 创建 CSV 写入器并指定表头
    writer = csv.DictWriter(file, fieldnames=["tag", "title"])

    # 写入表头
    writer.writeheader()

    # 写入每一行数据
    for row in titles:
        writer.writerow(row)
```
相同逻辑可在 `requests-beautifulsoup-scraper.py` 文件中找到。

### 2. Selenium
```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.by import By
import csv

# 设置无头模式的 WebDriver
options = Options()
options.add_argument("--headless")
driver = webdriver.Chrome(service=Service(), options=options)

# 要抓取的页面 URL
url = "https://en.wikipedia.org/wiki/Web_scraping"

# 在浏览器中打开页面
driver.get(url)

# 用于存储抓取到的标题
titles = []

# 标题级别列表（h1, h2, h3, h4, h5）
title_level_list = [1, 2, 3, 4, 5]

# 遍历每个标题级别
for title_level in title_level_list:
    # 使用 CSS 选择器查找当前级别的所有元素
    title_elements = driver.find_elements(By.CSS_SELECTOR, f"h{title_level}")

    # 遍历每个标题元素
    for title_element in title_elements:
        # 数据提取逻辑
        tag = title_element.tag_name
        text = title_element.text

        # 构建包含标签与标题文本的字典
        title = {
            "tag": tag,
            "title": text,
        }

        # 追加到列表
        titles.append(title)

# 关闭浏览器
driver.quit()

# 打开 CSV 文件写入数据
with open("titles.csv", mode="w", newline="", encoding="utf-8") as file:
    # 创建 CSV 写入器并指定表头
    writer = csv.DictWriter(file, fieldnames=["tag", "title"])

    # 写入表头
    writer.writeheader()

    # 写入每一行数据
    for row in titles:
        writer.writerow(row)
```
相同逻辑可在 `selenium-scraper.py` 文件中找到。

### Scrapy
首先，初始化一个 Scrapy 项目：
```bash
scrapy startproject scrapy_scraping
```
然后进入 `scrapy_scraping` 文件夹，创建一个名为 “wikipedia” 的 Spider，目标为指定页面：
```bash
cd scrapy_scraping
scrapy genspider wikipedia https://en.wikipedia.org/wiki/Web_scraping
```
`scrapy_scraping/scrapy_scraping/spiders` 文件夹下会出现一个 `wikipedia.py` 文件：
```python
import scrapy


class WikipediaSpider(scrapy.Spider):
    name = "wikipedia"
    allowed_domains = ["en.wikipedia.org"]
    start_urls = ["https://en.wikipedia.org/wiki/Web_scraping"]

    def parse(self, response):
        pass
```
实现抓取逻辑：
```python
import scrapy


class WikipediaSpider(scrapy.Spider):
    name = "wikipedia"
    allowed_domains = ["en.wikipedia.org"]
    start_urls = ["https://en.wikipedia.org/wiki/Web_scraping"]

    def parse(self, response):
        # 存储标题的列表
        titles = []

        # 标题级别列表（h1, h2, h3, h4, h5）
        title_level_list = [1, 2, 3, 4, 5]

        # 遍历每个标题级别
        for title_level in title_level_list:
            # 查找当前标题级别的所有元素
            title_elements = response.css(f"h{title_level}")

            # 遍历每个标题元素
            for title_element in title_elements:
                # 提取标签与文本
                tag = title_element.root.tag
                text = title_element.css("::text").get().strip()

                # 直接向 feed 产出数据
                yield {
                    "tag": tag,
                    "title": text,
                }
```
现在可运行该 Spider 并导出到 `titles.csv`：
```bash
scrapy crawl wikipedia -o titles.csv
```
你可以在仓库的 `scrapy_scraping` 文件夹中找到该 Scrapy 项目。

## Python 网页抓取的挑战
网页抓取并不总像本仓库演示的那样简单。多数网站深知其数据的价值，即使数据是公开可见的，因此会实施多种[反爬策略](https://www.bright.cn/blog/web-data/anti-scraping-techniques)来阻止抓取访问与数据提取。

一些高效方法包括 CAPTCHA、浏览器指纹、TLS 指纹、限流与 IP 封禁。尽管可以通过各种变通方法绕过，但这是一场“猫鼠游戏”，大多数技巧只是暂时有效且并不稳定。

解决方案？继续往下看！

## 使用 Web Scraper API 简化抓取
[Bright Data 的 Web Scraper API](https://www.bright.cn/products/web-scraper) 为从 100 多个热门域名（包括知名电商与社交媒体平台）提取结构化数据提供了高效且可扩展的解决方案。支持的域名列表涵盖了全球访问量最高的一些站点。

通过专用端点，API 可无缝获取高质量、合规的数据。其关键特性包括：
- 批量请求（每次最多 5,000 个 URL）。
- 支持导出为 JSON、CSV 等格式。
- 可与任意编程语言与 HTTP 客户端工具集成。
- 无限并发抓取任务，加速数据采集。
- 99.9% 在线率。
- 自动 IP 轮换、验证码识别与 JavaScript 渲染，避免封禁。
- 内置集成住宅代理网络，覆盖 195 个国家/地区、超 7,200 万 IP。

Web Scraper API 可将 Python（或其他语言）中的网页抓取简化为一次简单的 API 调用。查看[官方文档](https://docs.brightdata.com/scraping-automation/web-data-apis/web-scraper-api/overview)中的集成指南。
