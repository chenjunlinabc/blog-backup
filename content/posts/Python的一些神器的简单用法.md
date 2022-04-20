---
title: "Python的一些神器的简单用法"
categories: [ "默认" ]
tags: [ "Python" ]
draft: false
slug: "27"
date: "2021-06-16 14:39:00"
---

Virtualenv是一个能创建隔绝的独立的Python虚拟环境工具。它能够建立多个相互独立，互不影响的Python工作环境

用来创建一套独立于系统Python环境的虚拟环境，在虚拟环境下，使用pip安装的依赖都会安装到当前的虚拟环境中，对系统的python环境没有影响

当开发多个Python程序时当，程序1要使用3.6环境，但是程序2要使用3.8环境时，Virtualenv可以完美解决这个问题

Windows
pip install virtualenvwrapper-win

使用pip安装Virtualenv

pip3 install virtualenv

然后创建一个Virtualenv虚拟环境

virtualenv webpy #webpy为虚拟环境目录名，目录名自定义

virtualenv -p python环境路径 虚拟环境名 #创建指定Python环境路径的虚拟环境

virtualenv --no-site-packages 虚拟环境名 #创建一个干净的Python虚拟环境，系统Python环境的所有第三方包不会复制过来

virtualenv --no-site-packages --python=版本名 虚拟环境名 #创建一个指定python版本的虚拟环境

workon # 输出所有虚拟环境名 Windows

workon 虚拟环境名 # 进入虚拟环境 Windows

source 文件夹路径 # 激活当前virtualenv并进入虚拟环境

或者进入虚拟环境目录的bin目录，输入source activate

Windows是在虚拟环境目录下的Scripts目录，输入activate

deactivate # 退出当前环境

在虚拟环境下，使用pip安装的所有第三方包都会安装到当前的虚拟环境中，不会对系统的Python环境进行"污染"

想要删除某一个虚拟环境时，只需要将虚拟环境的目录删除


---

pip使用国内源

清华：https://pypi.tuna.tsinghua.edu.cn/simple
豆瓣：http://pypi.douban.com/simple/
中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/

临时使用
pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple pandas

linux下

配置永久使用

cd ~ # 进入用户目录
mkdir .pip # 新建一个隐藏文件夹
touch pip.conf # 新建一个文件
nano pip.conf # 编辑文件，这里使用nano，可以使用其他编辑器，例如vim

在文件中添加
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple

windows下

配置永久使用

在user目录下创建一个pip目录

在该目录新建一个pip.ini文件

在文件中添加
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
[install]
trusted-host = pypi.tuna.tsinghua.edu.cn






---



jupyter

最简单的安装方法就是使用python的科学计算包----Anaconda

该发行版中包含了大量的科学包，其中就附带了jupyter


可以通过 pip 来安装 pip install jupyter notebook

启动notebook服务器也很简单，在终端使用jupyter notebook命令就可以了‘’

服务器的默认地址是http://localhost:8888


通过点击“New”，来新建文件夹或者文档


通过ctrl+c来关闭服务器



修改jupyter默认工作路径

在终端使用jupyter notebook --generate-config

输出一个路径的文件，修改这个文件


找到#c.NotebookApp.notebook_dir = ''

修改成c.NotebookApp.notebook_dir = '你想要修改到的那个工作目录的路径'


这个工作目录必须提前创建好，否则可能出现闪退



修改默认浏览器

同样在终端使用jupyter notebook --generate-config

如果你已经知道这个文件在哪里可以不用，这个命令只是让我们找到jupyter的配置文件，jupyter_notebook_config.py

找到#c.NotebookApp.notebook_dir = ''

在它的后面加入


    import webbrowser
    webbrowser.register(
        "Microsoft Edge", 
        None, 
        webbrowser.GenericBrowser(u""C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe"")
        )
    c.NotebookApp.browser = "Microsoft Edge"

路径要修改为浏览器的路径

---

conda

conda有两个，一个是anaconda，另一个是miniconda

anaconda是集成（包含）了一些科学计算包，而miniconda是你要用什么就装什么

Miniconda 是一个 Anaconda 的轻量级版本，默认只包含了 python 和 conda，但是可以通过 pip 和 conda 来安装所需要的包。

这里安装的minicoda

wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh

bash Miniconda3-latest-Linux-x86_64.sh

如果是新机器，一路yes就可以了


创建环境

conda create --name webpy python=3.7 // 建立一个名叫py37的环境，指定Python版本为Python3.7的最新版本

激活环境

activate webpy // 用于Windows系统
source activate webpy // 用于mac或者linux系统

python --version // 检查Python版本

退出当前环境
deactivate webpy // 用于Windows系统
source deactivate webpy // 用于mac或者linux系统


conda remove --name webpy --all // 删除指定环境



conda info -e // 查看系统的所有环境



conda install numpy // 安装numpy库

conda list // 查看已经安装好的库

conda list -n webpy // 查看指定环境已经安装好的库


conda install -n webpy numpy // 安装库到指定环境内

conda update -n webpy numpy // 指定环境更新指定库

conda remove -n webpy numpy // 删除指定环境中的指定库

conda update anaconda // 更新anaconda

conda update python // 更新Python



添加国内镜像来提升下载速度（使用清华镜像站）

conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/ 
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/ 
conda config --set show_channel_urls yes



---


docx模块（操作Word）

导入模块
from docx import Document
from docx.shared import Inches

\#初始化Document对象
document = Document()

\#Document("1.docx")

\#写入标题，段落样式由level控制，范围0到9，默认为1
document.add_heading(text="hallo word",level=9)

\#写入段落，段落样式由style控制
data = document.add_paragraph(text="https://cjlio.com/",style=None)

\#插入段落到现有段落中，段落样式由style控制
data.insert_paragraph_before(text="abcxyz",style=None)

\#插入图片,并且缩小图片英寸为1.6
document.add_picture("1.jpg",width=Inches(1.6))

\#插入表格，rows是指定行数，cols是指定行数，style为表格样式
table = document.add_table(rows=1,cols=3,style="Table Grid")

\#插入列表中第一行的单元格
maincells = table.rows[0].cells
maincells[0].text = "id"
maincells[1].text = "user"
maincells[2].text = "pass"

\#初始化
    data_cells =(
        [1,"root","a"],
        [2,"admin","b"],
        [3,"user","c"]
    )

\#写入
    for item in data_cells:
        maincells_rows = table.add_row().cells
        maincells_rows[0].text = str(item[0])
        maincells_rows[1].text = item[1]
        maincells_rows[2].text = item[2]

\#保存
document.save("hallo.docx")


---

\#docx样式

\# 导入模块
from docx import Document
from docx.shared import Pt
from docx.shared import RGBColor
from docx.oxml.ns import qn

\# 初始化Document对象
document = Document("1.docx")

\# 插入页眉
header = document.sections[0].header

header.add_paragraph("hallo python")

\# 插入页脚
footer = document.sections[0].footer

footer.add_paragraph("hallo word")

\# 样式
p1 = document.add_paragraph()
text1 = p1.add_run("hallo WORD，hallo python-docx，你好，世界")

\# 字体大小
text1.font.size = Pt(16)

\# 字体是否加粗
text1.bold = True

\# 字体
text1.font.name = "黑体"

\# 字体（只能设置中文）
text1.element.rPr.rFonts.set(qn("w:eastAsia"), "黑体")

\# 字体颜色
text1.font.color.rgb = RGBColor(0, 0, 255)

\# 斜体
text1.italic = True

\# 下划线
text1.underline = True

\# 保存
document.save("hallo.docx")



---

xlrd模块+xlwt模块(操作xlsx)

\# 导入xlrd模块
import xlrd

\#加载1.xlsx
data = xlrd.open_workbook("1.xlsx")

\#判断是否加载成功，可以指定第几张工作表，0为第一个工作表
print(data.sheet_loaded(0)) \#True

\#卸载指定工作表
\#data.unload_sheet(0)

\#输出全部工作表名字（Sheet）
print(data.sheet_names())

\#输出工作表的数量
print(data.nsheets)

\#输出全部所有sheet对象
print(data.sheets())

\#将指定工作表索引到变量中
main = data.sheet_by_index(0)

\#名字索引到变量中
\#main = data.sheet_by_name("Sheet1")

\#输出工作表名字
print(main.name)

\#输出工作表有效总行数
print(main.nrows)

\#输出工作表有效总列数
print(main.ncols)

\#输出工作表有效总列数
print(main.row_len(1))

\#输出单元格值类型和内容
print(main.row(0))

\# 数据类型：
\# 空：0
\# 字符串：1
\# 数字：2
\# 日期：3
\# 布尔：4
\# error：5

\#输出第1行第二列的单元格内容
print(main.row(0)[1].value)

\#输出第二列表的全部内容
print(main.col(1))

\#输出第一行的内容
print(main.row_values(0))

\#输出单元格数据类型
print(main.row_types(0))

\#输出第一列的内容
print(main.col_values(0))

\#输出第二列的数据类型
print(main.col_types(1))

\#输出第6行第一列内容和数据类型
print(main.cell(5,0))

\#输出第6行第一列内容的数据类型
print(main.cell(5,0).ctype)

\#输出第6行第一列内容
print(main.cell(5,0).value)





利用xlwt模块写

\#导入xlwt模块
import xlwt

\#xlwt模块不支持xlsx，只支持xls

\#新建workbook，字符集为utf-8
hallo = xlwt.Workbook(encoding="utf8")

\#新建sheet
abc = hallo.add_sheet("Sheet1")

\#第1行到第3行和第1列到第9列合并并且写入hallo word
\#abc.write_merge(0,2,0,8,"hallo word")

\#在第4行第1列写入hallo word
\#abc.write(3,0,"hallo word")

\#批量写入
    data = ((1,"hallo","abc","xyz",123),(1,"hallo","abc","xyz",123))

    for i,a in enumerate(data):
        for q,s in enumerate(a):
            abc.write(i,q,s)

\#在第1行第1列写入图片，只支持bmp格式
abc.insert_bitmap("1.bmp", 0, 0)

\#保存
hallo.save("hallo.xls")



\#样式

\#初始化
style = xlwt.XFStyle()

\#初始化样式
stylefont = xlwt.Font()

\#名称
stylefont.name = "宋体"

\#加粗
stylefont.bold = True

\#字号
stylefont.height = 11*20

\#字体颜色，注意不是RGB颜色，而是在xlwt内部定义的
stylefont.colour_index = 0x0A

style.font = stylefont 


\#初始化Alignment
align = xlwt.Alignment()

\#水平对齐方式
align.vert = 0x01

\#垂直对齐方式
align.horz = 0x00

\#应用对齐方式
style.alignment = align

\#水平对齐方式
\#
\#0x01(左端对齐)
\#0x02(水平居中对齐)
\#0x03(右端对齐)
\#
\#
\#垂直对齐方式
\#
\#0x00(上端对齐)
\#0x01(垂直居中对齐)
\#0x02(底部对齐)


\#初始化Borders
borders = xlwt.Borders()

\#指定边框样式
borders.right = xlwt.Borders.DASHED


\#应用边框
style.borders = borders

\# top 上边框
\# bottom 下边框
\# left 左边框
\# right 右边框
\#
\#DOTTED 点线
\#DASHED 虚线


\#初始化Pattern
color = xlwt.Pattern()

\#填充模式为完全填充
color.pattern = xlwt.Pattern.SOLID_PATTERN

\#填充黑色
color.pattern_fore_colour = 0

\# 应用
style.pattern = color


\#应用style
abc.write_merge(0,2,0,8,"hallo word",style)



xlrd和xlwt使用起来不是很好，而且xlwt模块不支持xlsx，因此不详细写，openpyxl才是王道


---





Celery是一个基于Python开发的分布式任务调度模块

安装

pip install celery[redis]

如果需要redis消息中间件的话，也可以选择RabbitMQ



---






easy_install是一个基于setuptools的工具，可以用来下载，编译，管理Python的包

官网：http://peak.telecommunity.com/DevCenter/EasyInstall

安装setuptools就会安装easy_install

pip install setuptools


或者通过ez_setup.py来安装

https://pypi.org/project/ez_setup/

下载ez_setup.py的源程序

安装easy_install：
python ez_setup.py

更新easy_install:
python ez_setup.py –U setuptools




安装包

easy_install Virtualenv

也可以通过url来安装

将已经安装的到pypl上最新版本

easy_install --upgrade PyProtocols

更新包

easy_install "Virtualenv==x.x"

或者要求版本大于某个版本：

easy_install "Virtualenv>x.x"

查找PyPI上的最新可用版本

easy_install --upgrade Virtualenv

删除包

只需要删除包名版本的.egg文件或者目录，如果不想继续使用删除的包，然后easy_install -mxN 包名



---




Python内置多线程库threading

进程是资源分配的最小单位，一个程序必须至少有一个进程，一个进程必须至少有一个线程

线程是程序执行的最小单位，多线程是可以有效加速程序计算

进程的资源独立（独立内存，地址空间等等），而线程是可以共享进程中的数据的（IPC），同一个进程下的线程，共享使用相同的地址空间（全局），但是也存在数据同步和互相排斥的问题


导入threading模块

import threading

查看全部正在运行的线程信息（不包括没启动或者已经终止的线程）

threading.enumerate()

查看正在运行的线程的数量

threading.active_count()

查看正在运行的线程信息

threading.current_thread()

添加线程（需要接受target参数，该参数指向这个线程要完成的任务）

threading.Thread(target=xxx)

启动线程

thread.start()

终止线程（会等待线程终止）

thread.join()

简单的例子：

    import threading
    def funs(name,data):
        print("hallo",name,data)
    a1 = threading.Thread(target=funs, args = ("root",666))
    a1.start()
    a1.join()

执行后如果看到hallo root 666就说明已经执行线程成功了


线程锁Lock

同一进程下的线程是可以共享内存的，而Lock是保证多线程之间不会出现内存被乱改或者没有同步，多线程在执行修改内存后，进行lock.acquire()共享内存上锁，执行完毕在解锁lock.release()



---





Python多进程库multiprocessing

multiprocessing库用法和threading类似，例如：


import multiprocessing
def funs(name,data):
    print("hallo",name,data)
if __name__=='__main__':
    a1 = multiprocessing.Process(target=funs, args = ("root",666))
    a1.start()
    a1.join()


进程池 Pool（进程池就是将需要完成的任务，放到该进程池中，由Python自己解决多进程问题）

例如：

    import multiprocessing
    def demo(x):
        return x+x
    def funs():
        pool = multiprocessing.Pool()
        data = pool.map(demo, range(3))
        print("hallo",data)
    if __name__=='__main__':
        funs()

Pool自定义需调用核数量（默认为CPU的核数）

    pool = multiprocessing.Pool(processes=4)


进程锁和线程锁用法一样，就不写了





---
