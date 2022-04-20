---
title: "python爬虫学习笔记"
categories: [ "学习" ]
tags: [ "Python" ]
draft: false
slug: "125"
date: "2021-12-31 01:05:00"
---

urllib，xpath，jsonpath，beautiful，requests，selenium，Scrapy




python库内置的HTTP请求库
urllib.request 请求模块
urllib.error 异常处理模块
urllib.parse url解析模块
urllib.robotparsef robots.txt解析模块

urllib.request提供了最基本的http请求方法，主要带有处理授权验证，重定向，浏览器Cookies功能

模拟浏览器发送get请求，就需要使用request对象，在该对象添加http头
import urllib.requst
response = urllib.request.urlopen('https://cjlio.com/')
print(response.read().decode('utf-8'))

使用type()方法
import urllib.requst
response = urllib.request.urlopen('https://cjlio.com/')
print(type(response))

HTTPResposne类型对象

通过status属性获取返回的状态码
import urllib.requst
response = urllib.request.urlopen('https://cjlio.com/')
print(response.status)
print(response.getheaders())

post发送一个请求，只需要把参数data以bytes类型传入
import urllib.parse
import urllib.request
data = bytes(urllib.parse.urlencode({'hallo':'python'}),encoding='utf-8')
response = urllib.request.urlopen('http://httpbin.org/post'.data = data)
print(response.read())

timeout参数用于设置超时时间，单位为秒
import urllib.request
response = urllib.request.urlopen('https://cjlio.com/',timeout=1)

这里设置超时时间为1秒，如果超了1秒，服务器依然没有响应就抛出URLError异常，可以结合try和except









    import urllib.parse
    import urllib.request
    url = "https://cjlio.com/"
    headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.110 Safari/537.36 Edg/96.0.1054.62'}
    data = urllib.request.Request(url=url, headers=headers)
    hi = hallo = urllib.request.urlopen(data)
    #hallo = urllib.request.urlopen(data).read().decode('utf-8')
    #print(hallo)

    # 6个方法
    #abc = hi.read(6) #按字节返回
    #xyz = hi.readline()  # 读取一行
    #a = hi.readlines()  # 一行一行的读取
    #b = hi.getcode()  # 状态码
    #c = hi.geturl()  # 获取url地址
    #d = hi.getheaders()  # 获取状态信息

    # 下载到本地

    #urllib.request.urlretrieve(url,data.html)

    #url_img = "https://cjlio.com/1.jpg"
    #urllib.request.urlretrieve(url_img,abc.jpg)

    #url_mp4 = "https://cjlio.com/1.mp4"
    #urllib.request.urlretrieve(url_img,xyz.mp4)


    # auote() 转Unicode编码
    #name = urllib.parse.quote("哈哈哈")

    #urlencode(),get拼接
    gg = {
        "name": "小陈",
        "data": "hallo word"
    }
    haha = urllib.parse.urlencode(gg)
    #print(haha)

    # post请求
    ssr = {
        data: "abc"
    }
    abcxyz = urllib.parse.urlencode(ssr).encode('utf‐8')
    nogo = urllib.request.Request(url=url, headers=headers, data=abcxyz)
    yesgo = urllib.request.urlopen(nogo)
    #print(yesgo.read().decode('utf‐8'))

    #ajax请求（get和post）


    #cookie（扩展headers）

    headers = {
        'Cookie': 'xxxx'
    }




    #HTTPHandler()

    #request = urllib.request.Request(url=url, headers=headers) 
    #handler = urllib.request.HTTPHandler()
    #opener = urllib.request.build_opener(handler) 
    #response = opener.open(request) 
    #print(response.read().decode('utf‐8'))



    #代理服务器
    urls = "http://baidu.com"

    request = urllib.request.Request(url=urls, headers=headers)
    proxies = {'https': '14.115.106.223:808'}
    handler = urllib.request.ProxyHandler(proxies=proxies) 
    opener = urllib.request.build_opener(handler) 
    response = opener.open(request) 
    content = response.read().decode('utf‐8')
    #print(content)




---

    from lxml import etree
    import urllib.request
    
    # 解析本地文件
    #html_data = etree.parse('./hallo.html', etree.HTMLParser())
    # 解析服务器响应文件
    # html = etree.HTML(response.read().decode('utf‐8')
    #result = etree.tostring(html_data)
    # print(result.decode('utf-8'))
    # /为查找子节点，//为查找全部子孙节点（不考虑层级）
    # list = html_data.xpath("//ul/li")
    # 查找全部带id
    # list = html_data.xpath("//ul/li[@id]/text()")
    # 查找指定id
    # list = html_data.xpath('//ul/li[@id="a"]/text()')
    # 查找指定class的
    # list = html_data.xpath('//ul/li[@class="a"]/text()')
    # 模糊查询（包含）
    # list = html_data.xpath('//ul/li[contains(@id, "ab")]/text()')
    # 查找以什么开头的
    # list = html_data.xpath('//li[starts-with(@id, "a")]/text()')
    # 查找以什么结尾的
    # list = html_data.xpath('//li[ends-with(@id, "a")]/text()')
    # 查找内容
    # list = html_data.xpath('//a[text()="小陈的辣鸡屋"]/text()')
    # 多属性匹配(和)
    # list = html_data.xpath('//li[contains(@id, "a") and @href="https://cjlio.com"]/a/text()')
    # 或者
    # list = html_data.xpath('//li[contains(@id, "a") | @href="https://cjlio.com"]/a/text()')
    # 查找指定顺序的
    # list = html_data.xpath('//a[1]/text()') #获取第一个，可以指定第几个
    # list = html_data.xpath('//a[last()]/text()') #获取最后一个
    # list = html_data.xpath('//a[position()<6]/text()') # 获取前5个
    # list = html_data.xpath('//a[last()-3]/text()') #获取倒数第4个
    # print(list)
    
    #实例：获取logo和logo的url
    #url = "https://cjlio.com"
    #headers = {
    #    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.110 Safari/537.36 Edg/96.0.1054.62'
    #}
    #request = urllib.request.Request(url=url, headers=headers)
    #response = urllib.request.urlopen(request)
    #content = response.read().decode("utf-8")
    #hallo = etree.HTML(content)
    #data = hallo.xpath('//h3[@id="logo"]/a/@href')[0]
    #data1 = hallo.xpath('//h3[@id="logo"]/a/text()')[0]
    #print(data,data1)
    
    
    #获取文章的标题
    
    
    
    
    #headers = {
    #    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.110 Safari/537.36 Edg/96.0.1054.62'
    #}
    #
    #if __name__ == "__main__":
    #    page1 = int(input("开始"))
    #    page2 = int(input("结束"))
    #
    #    for page in range(page1, page2 + 1):
    #        urldata = "https://cjlio.com/archives/" + str(page) + ".html"
    #        #print(urldata)
    #        request = urllib.request.Request(url=urldata, headers=headers)
    #        response = urllib.request.urlopen(request)
    #        if (response.getcode() == 200):
    #            content = response.read().decode("utf-8")
    #            hallo = etree.HTML(content)
    #            data = hallo.xpath('//h3[@class="post-title"]/a/text()')[0]
    #            print(data)
    #        elif(response.getcode() == 404):
    #            continue
    
    
    #爬取网站的全部a链接，并且返回到数组中
    
    #if __name__ == "__main__":
    #    url = input("请输入要爬取的url")
    #    abc = int(input("是否启动只搜索当前域名的url,请回复0或者1"))
    #    headers = {
    #       'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.110 Safari/537.36 Edg/96.0.1054.62'
    #    }
    #    request = urllib.request.Request(url=url, headers=headers)
    #    response = urllib.request.urlopen(request)
    #    content = response.read().decode("utf-8")
    #    hallo = etree.HTML(content)
    #    data = hallo.xpath('//a/@href')
    #    a = 1
    #    b = 1
    #    c = "https"
    #    d = "http"
    #    if (abc == 1):
    #        while a < len(data):
    #            if (c or d in data[a]):
    #                if (url in data[a]):
    #                    urldata = data[a]
    #                    print(urldata)
    #                a += 1
    #    else:
    #        while b < len(data):
    #            if (c or d  in data[a]):
    #                urldata = data[b]
    #                print(urldata)
    #                b += 1
    

---


练习（json解析bing官方提供的api，获取精密壁纸）


    import jsonpath
    import json
    import urllib.request
    # pip install jsonpath
    headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.110 Safari/537.36 Edg/96.0.1054.62'
    }
    abc = 0
    while abc == 0 :
        a = int(input("请输入要下载的张数（bing最多只能下载8张）"))
        aa = str(a)
        if(a<=8):
            urlapi = "https://cn.bing.com/HPImageArchive.aspx?format=js&idx=0&n="+aa+"&mkt=zh-CN"
            request = urllib.request.Request(url=urlapi, headers=headers)
            resp = urllib.request.urlopen(request)
            content = resp.read().decode("utf-8")
            with open("data.json", "w", encoding="utf-8") as fp:
                fp.write(content)
            obj = json.load(open("data.json", "r", encoding="utf-8"))
            hallo = jsonpath.jsonpath(obj, "$..url")
            img = jsonpath.jsonpath(obj, "$..enddate")
            c = 0
            b = 0
            while c < len(hallo):
                hi = "https://cn.bing.com"
                data = hi + hallo[c]
                print(data)
                c+=1
                while b < len(img):
                    haha = img[b] + ".jpg"
                    print(haha)
                    urllib.request.urlretrieve(data, filename=haha)
                    b+=1
                    break
            print("下载完成！！！")
            abc = 1
        else:
            print("您输入的张数超过8张，bing官方api只能下载8张，请重新输入")



---



Beautiful Soup解析库，和lxml一样，可以从html或者xml中解析数据和提取数据，Beautiful Soup会自动将输入数据转换为unicode编码，输出数据转换为utf-8编码。不需要考虑编码问题。除非文档没有说明编码


目前使用的是Beautiful Soup 4，简称bs4，Beautiful Soup3已停止维护

安装

pip install beautifulsoup4

注意：Beautiful Soup支持第三方解析器，如果不使用第三方解析器的话，将使用Python标准库中的html解析器

    BeautifulSoup(html, html.parser) # 内置标准库
    BeautifulSoup(html, lxml) # lxml库，需要下载lxml库
    BeautifulSoup(html, xml) # lxml的xml解析，需要下载lxml库
    BeautifulSoup(html, html5lib) # html5lib，需要下载html5lib库 pip install html5lib

第一个例子

    from bs4 import BeautifulSoup
    # data =  BeautifulSoup(open("haha.html"),encoding="utf-8", "lxml")
    data =  BeautifulSoup(open("haha.html"))
    print(data.prettify())


bs会将html文档解析为树状结构，该树状结构的节点是Python对象，而这些对象可以分为4种：

Tag：标签，通过tag获取指定标签内容，print(data.div)，可以通过data.标签名的方式获取标签的内容（注意：输出第一个符合条件的标签）

检查对象的类型：print (type(data.div))，可以看到输出结果为<class 'bs4.element.Tag'>，说明该对象为tag

tag有两个属性，分别为name和attrs

    print (data.name)
    print (data.div.name)

可以看到输出结果为[document]和div，data的name为[document]，而标签输出为标签本身的名字

    print (data.div.attrs)

可以看到输出结果是{'class': ['xxx']}的键值对，可以通过data.div["xxx"]方式获取属性值，也可以修改属性值（data.div["class"]= "nav"），删除属性（del data.div["class"]）


 
NavigableString：标签内部的文本（.string），print (data.p.string)，判断类型，print (type(data.p.string))，输出结果为<class 'bs4.element.NavigableString'>

BeautifulSoup：文档的全部内容，也就是data本身，print (type(data))，<class 'bs4.BeautifulSoup'>

Comment：特殊一点的NavigableString类型，可以输出注释的内容（不带注释符号），通过类型判断print (type(data.span.string))，可以看到输出<class 'bs4.element.Comment'>


查找
    
    print(data.div.content) # 查找div的全部子节点，并且以列表的方式输出，可以使用索引来指定获取第几个节点

    print(data.div.children)  #一样是查找div的全部子节点，不过是list生成器，需要手动遍历获取

    print(data.select("span")) #查找标签，也可以通过类名或者id名查找，支持组合查找（和css方式一样）

    print(data.find("div"))

    print(data.find("div",class_="nav"))




---





selenium是一个web应用自动化测试工具，selenium可以直接运行在目前主流浏览器驱动（本身没有浏览器功能，需要搭配第三方浏览器，支持无界面浏览器），模拟用户操作

因为其自动化和可以直接运行在浏览器的原因，可以用于爬虫

chromedriver：https://npm.taobao.org/mirrors/chromedriver（淘宝镜像地址，下载的驱动版本要和浏览器内核的版本一样）
edge: https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/
（webdriver.Edge()）
火狐：https://github.com/mozilla/geckodriver/releases （webdriver.Firefox()）


安装

pip install selenium


导入驱动

    from selenium import webdriver
    path = "浏览器驱动路径"
    driver = webdriver.Chrome(path)
    url = "https://cjlio.com"
    driver.get(url)
    data = driver.page_source
    print(data)


元素定位（模拟鼠标和键盘操作元素的点击或者输入等等，在操作元素之前需要获取到元素的位置）

注意：element是返回单个对象，elements可以返回多个对象

根据id查找元素

data = driver.find_element_by_id("app")

根据name属性值查找
data = driver.find_element_by_name("")

根据xpath查找
data = driver.find_elements_by_xpath()

根据标签名查找
data = driver.find_elements_by_tag_name()

使用bs4的方式查找
data = driver.find_element_by_css_selector("#app")

根据连接的文本查找
data = driver.find_element_by_link_text()

根据连接的文本查找（模糊）
data = driver.find_element_by_partial_link_text()

根据class名查找
data = driver.find_element_by_class_name()


交互和数据获取

数据获取

获取属性值

print(data.get_attribute("href"))

获取标签名

print(data.tag_name)

获取元素文本

print(data.text)

交互

指定浏览器大小

driver.set_window_size(900, 1000)

浏览器的前进和后退，以及刷新

driver.forward()
driver.back()
driver.refresh()


input输入框操作

data.click() #单击元素
data.send_keys("hallo") #输入
data.clear() #清除内容

执行指定js脚本

driver.execute_script(js)

例如：


    import time
    from selenium import webdriver
    path = "msedgedriver.exe"
    driver = webdriver.Edge(path)
    url = "https://www.baidu.com/"
    driver.get(url)
    data = driver.page_source
    datas = driver.find_element_by_id("kw")
    datas.send_keys("小陈的辣鸡屋")
    a = driver.find_element_by_id("su")
    a.click()
    time.sleep(2)
    #bottom = 'document.documentElement.scrollTop=10000'
    #driver.execute_script(bottom)
    b = driver.find_element_by_link_text("小陈的辣鸡屋")
    b.click()


Phantomjs是一个无界面浏览器，因为其不需要进行gui渲染，效率比较高，但是已经停止更新，这里只了解一下，推荐使用Headless Chrome

https://phantomjs.org/

phantomjs操作方式一样

    driver = webdriver.PhantomJS(path)


因为没有界面，访问网页是没有界面的，需要通过快照获取，driver.save_screenshot("cjlio.com.jpg")



Headless Chrome是基于Chrome 59版本以及以上版本的无界面模式（mac和Linux最低要求59版本，而win需要60版本）

    from selenium import webdriver
    from selenium.webdriver.chrome.options import Options
    chrome_options = Options()
    chrome_options.add_argument('--headless') # 无界面模式启动
    chrome_options.add_argument('--disable-gpu')
    path = "Chrome.exe" # Chrome浏览器的路径
    chrome_options.binary_location = path
    driver = webdriver.Chrome(chrome_options=chrome_options) # 实例化
    url = "https://cjlio.com"
    driver.get("url")
    driver.save_screenshot("cjlio.com.jpg")


具体操作方法和selenium一样



---







requests库是基于Python开发并且封装的http库，HTTP for Humans

安装

pip install requests

使用方式很简单

    import requests
    url = "https://cjlio.com"
    response = requests.get(url)
    response.status_code # 200 返回状态码
    print(type(response)) # 返回类型
    response.encoding = "utf-8" # 响应的编码
    print(response.text) # 返回源代码
    print(response.url) # 返回请求的url
    print(response.content) # 返回二进制数据
    print(response.headers) # 返回响应头

requests在请求json的时候还可以直接获取

get请求


get请求的参数通过params属性传递，不需要考虑url的编码问题，不需要考虑请求对象，例如：

    data = {
        'wd': 'python'
    }
    headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.110 Safari/537.36 Edg/96.0.1054.62'
    }
    response = requests.get('https://baidu.com/s?', params=data, headers=headers)
    print (response.text)


post请求

post请求不需要考虑编解码，不需要考虑请求对象

    data = {
        'url': 'cjlio.com',
        'name': 'root'
    }
    url = "https://httpbin.org/post"
    response = requests.post(url=url, data=data, headers=headers)
    import json
    obj = json.loads(response.text,encoding="utf-8")
    print (obj)

注意：默认使用application/x-www-form-urlencoded进行编码，如果想传递json，需要使用requests.post()的json参数，上传文件用files参数

另外还支持put()和delete()

传入cookie需要cookies参数，支持设置请求超时（timeout参数，单位为秒）

代理用proxies参数，指向一个字典（代理池）




---





scrapy是基于Python开发的爬取数据，提取数据的框架，可应用在数据挖掘，数据存储

安装

pip install scrapy

scrapy架构组件分为Scrapy Engine（引擎），Scheduler（调度器），Downloader（下载器），Spider（爬虫），Item Pipeline（数据管道），Downloader middlewares（下载中间件），Spider middlewares（Spider中间件）



Scrapy Engine：负责控制数据流（Data Flow）在组件中的流动，例如通信，数据传递，在某些情况下还触发相对应的事件

Scheduler：负责接收引擎发送的request请求并且将其存储在调度器中，当引擎需要时提供给引擎，调度器会自动去重url（也可以不去重）

Downloader：负责下载引擎发送的全部request请求的响应数据，将获取到数据提供给Spider

Spider：负责处理全部响应数据，并且进行分析提取数据，获取需要的数据，并且将需要进行额外跟进的url传递给引擎，以便重新进入调度器，一般情况一个Spider只负责一个或者多个特定的url

Item Pipeline：负责处理Spider提取出来的数据，经过分析，过滤，存储等步骤，最后存储在本地或者数据库中

Downloader middlewares：其是引擎和Downloader之间的钩子，自定义扩展下载器的功能，例如更换ua，ip等等

Spider middlewares：Spider和引擎之间的钩子，用来处理Spider的输入响应和输出数据，自定义扩展Spider的功能



具体数据流线路：引擎请求一个url，并且获取到处理这个url的Spider，并且向该Spider请求要第一个爬取的url，引擎获取到要爬取的url，将其传递给调度器，引擎再向调度器请求要下一个爬取的URL，调度器返回url给引擎，引擎通过下载中间件传递给下载器，由下载器下载数据，当数据下载完毕（不管是否成功），下载器将响应数据通过下载中间件返回给引擎，引擎得到数据并且通过Spider中间件传递给Spider处理数据，Spider处理数据并且将处理完毕的数据以及需要跟进的url提供给引擎，引擎再将处理完毕的数据传递给Item Pipeline进行下一步处理，并且将需要跟进的url传递给调度器，一直重复循环直到调度器中没有request



新建项目：

scrapy startproject scrapyDemo

这个项目中包含了一些文件，分别是scrapy.cfg（配置文件），scrapyDemo/（项目的Python模块，将在这里编写程序），items.py（item文件，目标文件），pipelines.py（管道文件，用来处理数据，默认为300优先级，值越小优先级越高），settings.py（项目的设置文件，例如robots协议是否遵守，定义ua等等），spiders/（spider爬虫程序存放的目录）


spider爬虫程序（/spiders目录下）


创建一个爬虫必须继承scrapy.Spider类，并且定义三个参数（必须唯一，用来区别其他爬虫），start_urls（爬虫程序执行时要爬取的url），parse()在被调用时，下载完成后得到的响应将作为唯一参数传递给该方法，这个方法负责解析返回回来的数据，以及提取数据和后续要进行下一步处理的响应数据

快速创建一个基础scrapy爬虫程序

scrapy genspider cjlio cjlio.com



    import scrapy
    
    class CjlioSpider(scrapy.Spider):
        name = 'cjlio'
        allowed_domains = ['cjlio.com'] # 过滤爬取的URL，不在此范围内的域名会被过滤掉
        start_urls = ['http://cjlio.com/']
    
        def parse(self, response):
            content = response.text
            print(content)

response属性：.text（返回响应的字符串），.body（返回二进制数据），.xpath()（使用xpath方法来解析，返回的数据为列表），.extract()获取seletor对象的data属性值，.extract_first()获取seletor列表的第一个的数据

还有.url（url地址），.status（状态码），.encoding（响应正文的编码），.request（生成request对象），.css()（用css选择器方式解析数据），.urljoin()（生成绝对url）


执行爬虫程序（cjlio为爬虫的name）

scrapy crawl cjlio

将获取的数据存储为json文件

scrapy crawl cjlio -o cjlio.json

注意：因为scrapy默认使用ascii存储json，需要改为utf-8

在settings.py中添加FEED_EXPORT_ENCODING = 'utf-8'解决

关闭robots协议（不遵守robots协议，注释掉或者改为False，默认遵守robots协议）

    ROBOTSTXT_OBEY = False


scrapy shell是一个交互终端，在没有启动spider的情况下尝试以及测试程序的xpath和css表达式

scrapy shell可以借助ipython终端

pip install ipython

注意：如果安装ipython，scrapy将使用ipython代替Python标准终端，ipython提供了自动补全，高亮等等功能

调试例子（不需要进入Python环境，可以直接在终端中调试）

scrapy shell cjlio.com

response.text

response.xpath()




items.py定义数据结构

    import scrapy
    class ScrapydemoItem(scrapy.Item):
        name = scrapy.Field()
        img = scrapy.Field()


cjlio.py（爬虫程序）

    import scrapy
    class CjlioSpider(scrapy.Spider):
        name = 'cjlio'
        allowed_domains = ['cjlio.com']
        start_urls = ['http://cjlio.com/']
        def parse(self, response):
            content = response.text
            name = content.xpath("xpath匹配").extract_first()
            img = content.xpath("xpath匹配").extract_first()
            print(name, img)


管道封装

cjlio.py（爬虫程序）

    from scrapyDemo.items import ScrapydemoItem
    ...
    data = scrapyDemoItems(name=name,img=img)
    yield data # 每获取一次data数据，就是返回一次数据给管道
    # yield的作用类似于生成器（generator）


注意：如果想使用管道，需要在settings.py中开启

找到

    ITEM_PIPELINES = {
        'scrapyDemo.pipelines.ScrapydemoPipeline': 300,
    }

将注释去掉，300指优先级（优先级1到1000），值越小优先级越高

pipelines.py（管道文件）中的item就是爬虫程序返回的data数据

启动爬虫之前（open_spider(self, spider)）


启动爬虫之后（close_spider(self, spider)）

开启多管道

在 ITEM_PIPELINES 字段中添加新管道（settings.py）

然后在pipelines.py定义新管道的类

查询当前项目中的爬虫任务：scrapy list


自定义UA

settings.py文件中USER_AGENT字段或者DEFAULT_REQUEST_HEADERS都可以


项目中配置headers

spiders爬虫程序

    headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.110 Safari/537.36 Edg/96.0.1054.62'
    }
    url = "https://cjlio.com"
    yield scrapy.Request(
        url=url,
        headers=headers
    )





数据入库

安装pymysql

pip install pymysql

settings.py

（pymysql）
DB_HOST = "127.0.0.1"
DB_RORT = 3306
DB_USER = "root"
DB_PASSWORD = "123abc"
DB_NAME = "dataMax"
DB_CHARSET = "utf8"

开一条新管道，专门处理数据入数据库



pipeline.py

    from scrapy.utils.project import get_project_settings
    import pymysql
    #读取settings文件
    class MysqlMax:
        def open_spider(self, spider):
            settings = get_project_settings()
            self.host = settings['DB_HOST']
            self.port = settings['DB_PORT']
            self.user = settings['DB_USER']
            self.password = settings['DB_PASSWORD']
            self.name = settings['DB_NAME']
            self.charset = settings['DB_CHARSET']
            self.connect()
        def connect(self):
            self.conn = pymysql.connect(
                host = self.host,
                port = self.port,
                user = self.user,
                password = self.password,
                name = self.name,
                charset = self.charset
            )
            self.cursor = self.conn.cursor()

        def process_item(self,item,spider):
            sql = 'insert into book(name,src) values("{}","{}").format(item['name'],item['img'])
            self.cursor.execute(sql)
            self.conn.commit()
            # 执行sql语句，并且提交
            return item

         def close_spider(self, spider):
             self.cursor.close()
             self.conn.close()


链接跟进和链接提取器

爬虫程序文件

    rules = (
        Rule(LinkExtractor(allow = ""),
            callback = "parse_item", 
            follow = True),
    )

allow为正则表达式，当链接满足正则表达式提取，为空则全部匹配（链接提取器）

callback为指定解析数据的规则

follow一般情况默认为False，指定是否从response提取的链接进行跟进，当callback为none时，follow为True

还有dent =()，用来过滤符合正则表达式的链接，当符合时不提取

allow_domains：允许的域名，deny_domains：不允许的域名

restrict_xpaths：提取符合xpath的链接，restrict_css：提取符合选择器的链接


注意：follow当为True会一直提取符合规则的链接，直到全部链接提取完毕




日志以及日志等级

日志等级

CRITICAL：严重错误
ERROR：一般错误
WARNING：警告
INFO：一般信息
DEBUG：调试信息（默认，只有出现DEBUG以及以上等级的日志才会打印输出）

settings.py文件可以设置哪些日志显示，哪些不显示

LOG_FILE：将信息存储到文件中，不显示在输出界面，后缀为.log

LOG_LEVEL：指定日志显示的等级

例如：
LOG_LEVEL = "WARNING"
LOG_FILE= "demo.log"













