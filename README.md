# -
基于pygame和tkinter  本地音乐播放器软件

<img alt="画江湖之不良人 第三季" src=".//播放软件截图.png">


```

import pygame
import os
import time
import random
from PIL import ImageTk, Image
import PIL#中的Image与tk中的重名，报错没有open
import sys
from tkinter import ttk
from tkinter import *
import tkinter.filedialog
import threading
import tkinter
from tkinter import filedialog
import configparser# 导入模块



config = configparser.ConfigParser()   # 创建对象
# config.write(open('config.ini', "w", encoding='utf8'))
config.read(".\\config.ini", encoding='utf8')  # 读取配置文件，如果配置文件不存在则创建
if len(config.sections()) == 0:
    print("---")
    # config.add_section('Name')
    config.read_dict({'section1': {}})#创建节点
    config.write(open('config.ini', 'w'))  # 一定要写入才生效
    print(config.sections())
       # 添加一个节点，节点名为section1, 此时添加的节点section1尚未写入文件
print("---")
print(config.sections())
top = tkinter.Tk()#创建窗口
pygame.mixer.init()#初始化
a = []
b = []
print(len(config.items("section1")))

# def wenjian():
#     pass


# def playmusic():
#     #用于音乐播放
#
#
#     pygame.mixer.music.load(fpath)#打开音乐文件
#     pygame.mixer.music.play(-1)#开始播放，-1代表循环，如果是5代表当前播放1次再加5次播放
#     #pygame.mixer.music.queue()#队列一个音乐文件以跟随当前
#     # pygame.mixer.music.set_pos()# 设定播放位置
#     # pygame.mixer.music.get_pos() # 获得音乐播放时间
#     # pygame.mixer.music.get_volume()  获取音量
#
#     a = pygame.mixer.music.get_busy()#查看是否正在播放
#     print(a)
#     b = pygame.mixer.music.get_pos()#查看从何处开始播放
#     print(b)


#窗口

# top.geometry('168x360')#初始化窗口大小
top.geometry('200x370+1711+640')  # 初始化窗口大小显示位置
top.title("播放器0.1V")#窗口命名
# top.overrideredirect(True)
top.wm_iconbitmap(".\\01.ico")#窗口图标
top.resizable(width=FALSE, height=FALSE)#设置窗口缩放
print("---1----")

# 背景
canvas = tkinter.Canvas(top, width=200, height=470, bd=0, highlightthickness=0)
imgpath = '.\\beijing.gif'
print("QAQ.QAQ.QAQ.QAQ")
img = PIL.Image.open(imgpath)
photo = PIL.ImageTk.PhotoImage(img)
print(img)

canvas.create_image(100, 200, image=photo)
print("^-^")
canvas.pack()

# 导入文件夹函数
def daoru():
    # list_name = L
    path1 = tkinter.filedialog.askdirectory()  # 打开文件夹
    print(path1)

    # 遍历文件夹写入文件函数
    def listdir(path):
        print("----------------------")
        for file in os.listdir(path):  # listdir：用于返回指定的文件夹包含的文件或文件夹的名字的列表
            file_path = os.path.join(path, file)  # 拼接路径和文件名
            if os.path.isdir(file_path):  # 判断是文件夹继续调用函数
                listdir(file_path)  # 自己调自己，继续循环判断
            elif os.path.splitext(file_path)[1] == '.mp3':  # 判断是MP3的音乐文件
                # file_path
                # list_name.append(file_path)#添加到列表
                # file_path = "E:/tt/abc.py"
                filepath, fullflname = os.path.split(file_path)  # 分离路径和文件名
                fname, ext = os.path.splitext(fullflname)  # 分离文件名和路径
                config.set('section1', fname, file_path)  # 注意键值是用set()方法
                config.write(open('config.ini', "w", encoding='utf8'))  # 将添加的节点node写入配置文件
                listbox.insert(tkinter.END, fname)  # 写入tk 的listbox组件最后一行
            elif os.path.splitext(file_path)[1] == '.flac':  # 判断是flac的音乐文件
                # file_path
                # list_name.append(file_path)#添加到列表
                filepath, fullflname = os.path.split(file_path)
                fname, ext = os.path.splitext(fullflname)
                config.set('section1', fname, file_path)  # 注意键值是用set()方法
                config.write(open('config.ini', "w", encoding='utf8'))  # 将添加的节点node写入配置文件
                listbox.insert(tkinter.END, fname)  # 写入tk 的listbox组件最后一行

    listdir(path1)  # 调用函数

# 删除
def Delete():
    print("^_^")
    # items = listbox.curselection()
    items = listbox.curselection()
    print(items)
    for k in items:
        print(k)
        # print(listbox.get(k))
        print(listbox.get(k))
        # lb.delete(k)
        listbox.delete(k)  # 删除listbox中的元素
        config.remove_option('section1', listbox.get(k))  # 删除配置文件中对应的路径
        config.write(open('config.ini', "w", encoding='utf8'))  # 写入配置文件

# 音量设置
def yingliang(yl):
    print(int(yl))
    y2 = int(yl)/100
    pygame.mixer.music.set_volume(float(y2))  # 设置音量
#音量滚动条
scale = tkinter.Scale(top, label='音量', from_=0, to=100,
                      orient=HORIZONTAL,#水平放置
                      activebackground='White',#指定当鼠标在上方飘过的时候滑块的背景颜色
                      length=165,  #滚动条长度
                      bg="Cornsilk",
                      sliderlength=13,#设置滑块的长度
                      relief=GROOVE,#边框样式
                      sliderrelief=FLAT,#设置滑块的样式
                      width=5,# 指定 Scale 组件的宽度
                      # tickinterval=0.2,#间隔值
                      font=("微软雅黑",8),  # 字体，大小
                      borderwidth='0',  # 按钮边框
                      resolution=1,  #移动值
                      command=yingliang)#返回函数
scale.set(50)  # 设置初始值
# scale.pack(fill=X, expand=1, side=LEFT)
# scale.grid(row=3, column=2, columnspan=5)


#播放进度
# def jingdu(yl):
#     print(yl)
#     # pygame.mixer.music.set_pos()# 设定播放位置
#     # pygame.mixer.music.get_pos() # 获得音乐播放时间
#     pygame.mixer.music.set_volume(float(yl))
# #滚动条
# scale2 = tkinter.Scale(top, from_=0, to=1.0, orient=HORIZONTAL,
#                       length=150,  #滚动条长度
#                       # tickinterval=0.2,#间隔值
#                       font=(0),  # 字体，大小
#                       borderwidth='1',  # 按钮边框
#                       resolution=0.01,  #移动值
#                       command=yingliang)#返回函数
# scale2.set(0)  # 设置初始值


fm1 = Frame(top)#组件
# photo = tkinter.PhotoImage(file="C:\\Users\\Administrator\\Desktop\\飞机大战\\111.gif")
# imgLabel = Label(top, text='pack1', image=photo)
# imgLabel.grid(row=1, column=2)
# Label(top, text='pack1').grid(row=1, column=2)
# Label(top, text='pack1').grid(row=2, column=1)
# Label(top, text='pack1').grid(row=2, column=2)
print("---2----")
#播放上一首
def Last_one():
    def xianc():
        global Xunhuan
        Xunhuan = False  # 结束旧线程的循环并结束线程
        time.sleep(0.2)
        Xunhuan = True
        # pygame.mixer.music.load()  # 打开音乐文件
        # pygame.mixer.music.play(-1)  # 开始播放，-1代表循环，如果是5代表当前播放1次再加5次播放
        if pygame.mixer.music.get_busy():  # 判断
            pygame.mixer.music.stop()  # 停止播放当前歌曲
        items = listbox.curselection()  # 获取listbox中的（x，）
        print(items)
        for k in items:  # 获取x值
            print(k)
            listbox.selection_clear(k)  # 取消当前选中
            listbox.selection_set(k - 1)  # 蓝色底框选中
            print(listbox.get(k - 1))  # 通过x值获取listbox中显示的具体元素
            pygame.mixer.music.load(config.get('section1', listbox.get(k - 1)))  # 打开歌曲文件，通过元素获取配置文件中歌曲的路径
            pygame.mixer.music.play(-1)  # 播放歌曲
        while Xunhuan:
            print("---1---")
            if not pygame.mixer.music.get_busy():  # 判断是否有歌曲正在播放
                # print(panduan)
                if panduan == ["随机"]:
                    print("随机")
                    NUM = listbox.curselection()
                    print(NUM)
                    for Number in NUM:
                        print(Number)  # 当前位置
                        listbox.selection_clear(Number)  # 取消选中
                    randem_num = len(config.items("section1"))
                    randem_name = random.randint(0, randem_num)
                    listbox.selection_set(randem_name)  # 蓝色底框选中
                    Key = listbox.get(randem_name)
                    print(Key)
                    pygame.mixer.music.load(config.get('section1', Key))  # 打开音乐文件
                    pygame.mixer.music.play(0)  # 开始播放，-1代表循环，如果是5代表当前播放1次再加5次播放

                elif panduan == ["循环"]:
                    print("循环")
                    NUM = listbox.curselection()
                    print(NUM)
                    for Number in NUM:
                        print(Number)
                        Key = listbox.get(Number)
                    print(Key)
                    listbox.selection_clear(Number)  # 取消选中
                    # NUM = NUM+1
                    listbox.selection_set(Number)  # 蓝色底框选中
                    pygame.mixer.music.load(config.get('section1', Key))  # 打开音乐文件
                    pygame.mixer.music.play(-1)  # 开始播放，-1代表循环，如果是5代表当前播放1次再加5次播放

                else:
                    NUM = listbox.curselection()
                    print(NUM)
                    for Number in NUM:
                        print(Number)
                        Key = listbox.get(Number + 1)
                    print(Key)
                    listbox.selection_clear(Number)  # 取消选中
                    # NUM = NUM+1
                    listbox.selection_set(Number + 1)  # 蓝色底框选中
                    pygame.mixer.music.load(config.get('section1', Key))  # 打开音乐文件
                    pygame.mixer.music.play(0)  # 开始播放，-1代表循环，如果是5代表当前播放1次再加5次播放
                # name_yy = name_yy+1
            # NUM1 = listbox.curselection()
            # # print(NUM1)
            # for Number1 in NUM1:
            #     # print(Number1)
            #     Key1 = listbox.get(Number1)
            # # print(Key1)
            # label.configure(text="正在播放:"+Key1)

            # print("T_T")

    t = threading.Thread(target=xianc)  # 开启线程执行播放音乐函数
    t.setDaemon(bool)  # 主进程结束子线程也结束
    t.start()

    # 播放音乐和暂停
def Bofangandzhanting():
    # action.configure(text='Hello ')
    A = (len(a))
    if A == 0:
        tk1.configure(text=' ▶ ')
        print("--1--")
        a.append("ds")
        pygame.mixer.music.pause()  # 暂停音乐播放
    else:
        tk1.configure(text=' ║ ')
        print("--2--")
        a.pop()
        print("________________________")
        pygame.mixer.music.unpause()  # 恢复暂停的音乐
def Next_one():
    def xianc():
        global Xunhuan
        Xunhuan = False  # 结束旧线程的循环并结束线程
        time.sleep(0.2)
        Xunhuan = True
        # pygame.mixer.music.load()  # 打开音乐文件
        # pygame.mixer.music.play(-1)  # 开始播放，-1代表循环，如果是5代表当前播放1次再加5次播放
        if pygame.mixer.music.get_busy():  # 判断
            pygame.mixer.music.stop()  # 停止播放当前歌曲
        items = listbox.curselection()  # 获取listbox中的（x，）
        print(items)
        for k in items:  # 获取x值
            print(k)
            listbox.selection_clear(k)  # 取消当前选中
            listbox.selection_set(k + 1)  # 蓝色底框选中
            print(listbox.get(k + 1))  # 通过x值获取listbox中显示的具体元素
            pygame.mixer.music.load(config.get('section1', listbox.get(k + 1)))  # 打开歌曲文件，通过元素获取配置文件中歌曲的路径
            pygame.mixer.music.play(-1)  # 播放歌曲
        while Xunhuan:
            print("---1---")
            if not pygame.mixer.music.get_busy():  # 判断是否有歌曲正在播放
                # print(panduan)
                if panduan == ["随机"]:
                    print("随机")
                    NUM = listbox.curselection()
                    print(NUM)
                    for Number in NUM:
                        print(Number)  # 当前位置
                        listbox.selection_clear(Number)  # 取消选中
                    randem_num = len(config.items("section1"))
                    randem_name = random.randint(0, randem_num)
                    listbox.selection_set(randem_name)  # 蓝色底框选中
                    Key = listbox.get(randem_name)
                    print(Key)
                    pygame.mixer.music.load(config.get('section1', Key))  # 打开音乐文件
                    pygame.mixer.music.play(0)  # 开始播放，-1代表循环，如果是5代表当前播放1次再加5次播放

                elif panduan == ["循环"]:
                    print("循环")
                    NUM = listbox.curselection()
                    print(NUM)
                    for Number in NUM:
                        print(Number)
                        Key = listbox.get(Number)
                    print(Key)
                    listbox.selection_clear(Number)  # 取消选中
                    # NUM = NUM+1
                    listbox.selection_set(Number)  # 蓝色底框选中
                    pygame.mixer.music.load(config.get('section1', Key))  # 打开音乐文件
                    pygame.mixer.music.play(-1)  # 开始播放，-1代表循环，如果是5代表当前播放1次再加5次播放

                else:
                    NUM = listbox.curselection()
                    print(NUM)
                    for Number in NUM:
                        print(Number)
                        Key = listbox.get(Number + 1)
                    print(Key)
                    listbox.selection_clear(Number)  # 取消选中
                    # NUM = NUM+1
                    listbox.selection_set(Number + 1)  # 蓝色底框选中
                    pygame.mixer.music.load(config.get('section1', Key))  # 打开音乐文件
                    pygame.mixer.music.play(0)  # 开始播放，-1代表循环，如果是5代表当前播放1次再加5次播放
                # name_yy = name_yy+1
            # NUM1 = listbox.curselection()
            # # print(NUM1)
            # for Number1 in NUM1:
            #     # print(Number1)
            #     Key1 = listbox.get(Number1)
            # # print(Key1)
            # label.configure(text="正在播放:"+Key1)

            # print("T_T")

    t = threading.Thread(target=xianc)  # 开启线程执行播放音乐函数
    t.setDaemon(bool)  # 主进程结束子线程也结束
    t.start()

print("---3----")
#上一首按钮
tkinter.Button(fm1,
               text='上一首',#按钮上的显示
               command=Last_one,#点击事件
               width=7,#对于Button,label或者Text控件,这个属性可以定义以”字符数目” 为单位的高度,其他控件则是以”像素”为单位
               height=2,
               bg='Cornsilk',  # 按钮背景色
               activebackground='Chocolate2',
               borderwidth='0',#按钮边框
               font=('微软雅黑',9)).pack(side=LEFT)
# b2.place(x=116, y=50, anchor=NW)
# b2.grid(row=2, column=3)
print("---4----")

# 播放按钮
tk1 = tkinter.Button(fm1,  # 不要加窗口名top，会报错
               text='▶',  # 按钮上的显示
               command=Bofangandzhanting,  # 点击事件
               activeforeground='Gainsboro',  # 点击后字体颜色
               activebackground='Chocolate3',  # 点击后按钮背景色
               bg='Cornsilk',  # 按钮背景色
               fg='Chocolate2',  # 按钮上字体颜色
               font=('微软雅黑',9),  # 字体，大小
               borderwidth='0',  # 按钮边框
               width=12,  # 对于Button,label或者Text控件,这个属性可以定义以”字符数目” 为单位的高度,其他控件则是以”像素”为单位
               height=2)
tk1.pack(side=LEFT)
# b1.place(x=171, y=50, anchor=NW)#定义按钮的位置
# b1.grid(row=2, column=4)
print("---5----")

#下一首按钮
tkinter.Button(fm1,
               text='下一首',#按钮上的显示
               command=Next_one,#点击事件
               width=7,#对于Button,label或者Text控件,这个属性可以定义以”字符数目” 为单位的高度,其他控件则是以”像素”为单位
               height=2,
               bg='Cornsilk',  # 按钮背景色
               activebackground='Chocolate2',
               borderwidth='0',#按钮边框
               font=('微软雅黑',9)).pack(side=LEFT)
# b3.place(x=226, y=50, anchor=NW)
# b3.grid(row=2, column=5)
print("---6----")
# fm1.grid(row=2, column=2)

print("---7----")
# global fpath
print("---8----")
# fpath = filedialog.askopenfilename()#获取文件路径
# return fpath#将路径赋值
# print(fpath)#打印路径：展示

def  Randomplay():

    def shijian1():
        global Xunhuan, panduan
        panduan = ["列表"]
        Xunhuan = False  # 结束旧线程的循环并结束线程
        time.sleep(0.2)  # 休眠0.2秒等待上个线程结束
        Xunhuan = True  # 开启新线程的循环
        shumxu.configure(text=' 列表循环中 ')  # 按钮显示
        def xianc3():
            # 判断播放状态并列表随机播放
            while Xunhuan:
                print("---2---")
                if not pygame.mixer.music.get_busy():  # 判断是否有歌曲正在播放
                    NUM = listbox.curselection()
                    print(NUM)
                    for Number in NUM:
                        print(Number)
                        Key = listbox.get(Number + 1)
                    print(Key)
                    listbox.selection_clear(Number)  # 取消选中
                    # NUM = NUM+1
                    listbox.selection_set(Number + 1)  # 蓝色底框选中
                    pygame.mixer.music.load(config.get('section1', Key))  # 打开音乐文件

                    pygame.mixer.music.play(0)  # 开始播放，-1代表循环，如果是5代表当前播放1次再加5次播放
        t = threading.Thread(target=xianc3)  # 开启线程执行播放音乐函数
        t.setDaemon(bool)  # 主进程结束子线程也结束
        t.start()

    def shijian2():
        global Xunhuan, panduan
        panduan = ["随机"]
        Xunhuan = False  # 结束旧线程的循环并结束线程
        time.sleep(0.2)  # 休眠0.2秒等待上个线程结束
        Xunhuan = True  # 开启新线程的循环
        shumxu.configure(text=' 随机播放中 ')#按钮显示
        def xianc2():

            # 判断播放状态并列表随机播放
            while Xunhuan:
                print("---3---")
                if not pygame.mixer.music.get_busy():  # 判断是否有歌曲正在播放
                    NUM = listbox.curselection()
                    print(NUM)
                    for Number in NUM:
                        print(Number)#当前位置
                        listbox.selection_clear(Number)  # 取消选中
                    randem_num = len(config.items("section1"))#获取一共有多少首歌
                    randem_name = random.randint(0, randem_num)#产生随机数
                    listbox.selection_set(randem_name)  # 蓝色底框选中
                    Key = listbox.get(randem_name)#随机歌曲名
                    print(Key)
                    pygame.mixer.music.load(config.get('section1', Key))  # 打开音乐文件

                    pygame.mixer.music.play(0)  # 开始播放，-1代表循环，如果是5代表当前播放1次再加5次播放
                # print("T_T")
        t = threading.Thread(target=xianc2)  # 开启线程执行播放音乐函数
        t.setDaemon(bool)  # 主进程结束子线程也结束
        t.start()

    def shijian3():
        global Xunhuan, panduan
        panduan = ["循环"]
        Xunhuan = False  # 结束旧线程的循环并结束线程
        time.sleep(0.2)  # 休眠0.2秒等待上个线程结束
        Xunhuan = True  # 开启新线程的循环
        shumxu.configure(text=' 循环播放中 ')  # 按钮显示
        def xianc4():
            # 判断播放状态并列表随机播放
            while Xunhuan:
                print("---4---")
                if not pygame.mixer.music.get_busy():  # 判断是否有歌曲正在播放
                    NUM = listbox.curselection()
                    print(NUM)
                    for Number in NUM:
                        print(Number)
                        Key = listbox.get(Number)
                    print(Key)
                    pygame.mixer.music.load(config.get('section1', Key))  # 打开音乐文件

                    pygame.mixer.music.play(-1)  # 开始播放，-1代表循环，如果是5代表当前播放1次再加5次播放
        t = threading.Thread(target=xianc4)  # 开启线程执行播放音乐函数
        t.setDaemon(bool)  # 主进程结束子线程也结束
        t.start()

    B = (len(b))
    if B == 0:
        b.append("1")
        shijian2()
    elif B == 1:
        b.append("2")
        shijian3()
    elif B == 2:
        b.remove("1")#指定删除
        b.remove("2")
        shijian1()


shumxu = tkinter.Button(top, text=' 列表循环中',
                        relief=RAISED,  #边框样式
                        activebackground='LightSteelBlue1',  # 点击后按钮背景色
                        # bg='Gainsboro',  # 按钮背景色
                        bg="LightSteelBlue1",#背景色
                        # activebackground='LightYellow1',#当鼠标放上去时，按钮的背景色
                        # activeforeground='grey21',#当鼠标放上去时，按钮字的前景色
                        # highlightcolor='DodgerBlue4',#要高亮的颜色
                        state=ACTIVE,  #设置按钮组件状态
                        borderwidth='0',  #边框
                        fg="black",  # 字体颜色
                        font=('微软雅黑'),
                        command=Randomplay)
# shumxu.grid(row=6, column=2)


#添加指定文件函数
def tianjia():
    fpath1 = filedialog.askopenfilename()  # 获取文件路径
    # L.append(fpath)
    # file_path = "E:/tt/abc.py"
    filepath, fullflname = os.path.split(fpath1)
    fname, ext = os.path.splitext(fullflname)
    config.set('section1', fname, fpath1)  # 注意键值是用set()方法
    config.write(open('config.ini', "w", encoding='utf8'))  # 将添加的节点node写入配置文件
    listbox.insert(tkinter.END, fname)
    config.write(open('config.ini', "w", encoding='utf8'))

# yingliang = float(input(':'))
# wenjianname = window()#调用窗口

#列表滚动条模块
fm2 = Frame(top, pady=0)#组件
scrollbar1 = Scrollbar(fm2,
                       activerelief=GROOVE,#指定当鼠标在上方飘过的时候滑块的样式
                       bd="10",#指定边框宽度
                       width='12',#指定滚动条的宽度
                       relief=GROOVE,#指定边框样式
                       bg="LightSteelBlue1",#指定背景颜色
                       borderwidth='0')
listbox = Listbox(fm2,
                  width=25,
                  bg="LightSteelBlue1",
                  yscrollcommand=scrollbar1.set,
                  borderwidth='0',#边框
                  # bg='Black',#指定背景颜色
                  # underline=False,
                  fg="grey21",#字体颜色
                  selectbackground="DeepSkyBlue")#选中背景色
print(">.<")
print(config.options('section1'))
for i in config.options('section1'):
    print("-.-/-.-")
    listbox.insert(END, i)#遍历列表
print(">.<")
listbox.pack(side=LEFT)#组件左边left
scrollbar1.config(command=listbox.yview)
scrollbar1.pack(side=RIGHT, fill=Y)#组件右边，y轴拉伸

menu = Menu(top, tearoff=0)#右键菜单
menu.add_command(label="导入本地文件夹", command=daoru)
menu.add_separator()
menu.add_command(label="添加本地文件", command=tianjia)
menu.add_separator()
menu.add_command(label="删除", command=Delete)#只能删除listbox中的，非列表
def popupmenu(event):
    menu.post(event.x_root, event.y_root)
listbox.bind("<Button-3>", popupmenu)#右键绑定

# scrollbar2 = Scrollbar(fm2)#无法实现的x轴拉伸？
# scrollbar2.config(command=listbox.xview)？
# scrollbar2.pack(side=BOTTOM, fill=X)#组件下边，x轴拉伸？
#双击命令
def name(event):
    global Xunhuan
    Xunhuan = False#结束旧线程的循环并结束线程
    time.sleep(0.2)#休眠0.2秒等待上个线程结束
    Xunhuan = True#开启新线程的循环


    def xianc():
        # global num
        items = listbox.curselection()
        print(items)
        for name__yy in items:
            print("QAQ.QAQ.QAQ.QAQ")
            print(name__yy)
            y = listbox.get(name__yy)
        listbox.selection_set(name__yy)
             # print(listbox.get(num+1))
        # name_yy = listbox.get(listbox.curselection())
        print("_+_+_+_+")
        # print(listbox.curselection())
        pygame.mixer.music.load(config.get('section1', y))# 打开音乐文件
        pygame.mixer.music.play(0)# 开始播放，-1代表循环，如果是5代表当前播放1次再加5次播放
        # print(name_yy)#打印鼠标双击的元素
        #判断播放状态并列表循环播放
        while Xunhuan:
            print("---1---")
            if not pygame.mixer.music.get_busy():#判断是否有歌曲正在播放
                # print(panduan)
                try:
                    if panduan == ["随机"]:
                        print("随机")
                        NUM = listbox.curselection()
                        print(NUM)
                        for Number in NUM:
                            print(Number)  # 当前位置
                            listbox.selection_clear(Number)  # 取消选中
                        randem_num = len(config.items("section1"))
                        randem_name = random.randint(0, randem_num)
                        listbox.selection_set(randem_name)  # 蓝色底框选中
                        Key = listbox.get(randem_name)
                        print(Key)
                        pygame.mixer.music.load(config.get('section1', Key))  # 打开音乐文件
                        pygame.mixer.music.play(0)  # 开始播放，-1代表循环，如果是5代表当前播放1次再加5次播放

                    elif panduan == ["循环"]:
                        print("循环")
                        NUM = listbox.curselection()
                        print(NUM)
                        for Number in NUM:
                            print(Number)
                            Key = listbox.get(Number)
                        print(Key)
                        listbox.selection_clear(Number)  # 取消选中
                        # NUM = NUM+1
                        listbox.selection_set(Number)  # 蓝色底框选中
                        pygame.mixer.music.load(config.get('section1', Key))  # 打开音乐文件
                        pygame.mixer.music.play(-1)  # 开始播放，-1代表循环，如果是5代表当前播放1次再加5次播放

                    else:
                        NUM = listbox.curselection()
                        print(NUM)
                        for Number in NUM:
                            print(Number)
                            Key = listbox.get(Number+1)
                        print(Key)
                        listbox.selection_clear(Number)  # 取消选中
                        # NUM = NUM+1
                        listbox.selection_set(Number+1)  # 蓝色底框选中
                        pygame.mixer.music.load(config.get('section1', Key))  # 打开音乐文件
                        pygame.mixer.music.play(0)  # 开始播放，-1代表循环，如果是5代表当前播放1次再加5次播放
                except Exception:
                    NUM = listbox.curselection()
                    print(NUM)
                    for Number in NUM:
                        print(Number)
                        Key = listbox.get(Number + 1)
                    print(Key)
                    listbox.selection_clear(Number)  # 取消选中
                    # NUM = NUM+1
                    listbox.selection_set(Number + 1)  # 蓝色底框选中
                    pygame.mixer.music.load(config.get('section1', Key))  # 打开音乐文件
                    pygame.mixer.music.play(0)  # 开始播放，-1代表循环，如果是5代表当前播放1次再加5次播放
    t = threading.Thread(target=xianc)#开启线程执行播放音乐函数
    t.setDaemon(bool)#主进程结束子线程也结束
    t.start()



# name_yy = name_yy+1
# NUM1 = listbox.curselection()
# # print(NUM1)
# for Number1 in NUM1:
#     # print(Number1)
#     Key1 = listbox.get(Number1)
# # print(Key1)
# label.configure(text="正在播放:"+Key1)

# print("T_T")
listbox.bind('<Double-Button-1>', name)#返回listbox中的鼠标双击值
# fm2.grid(row=4, column=2, padx=0)
imgpath1 = '.\\beijing-1.gif'
print("QAQ.QAQ.QAQ.QAQ")
img1 = PIL.Image.open(imgpath1)
photo1 = PIL.ImageTk.PhotoImage(img1)
print(img1)
label = Label(top, width=20, image=photo1, compound=tkinter.CENTER)#关键:设置为背景图片)

def Playing_now():
    while True:
        print("-v-")
        if pygame.mixer.music.get_busy():  # 判断是否有歌曲正在播放
            try:
                NUM = listbox.curselection()
                print(NUM)
                for Number in NUM:
                    print(Number)
                    Key = listbox.get(Number)
                z = re.findall(r"-(.+)", Key)
                for i in z:
                    print(i)
                    label.configure(text="正在播放:" + i)
            except Exception:
                NUM = listbox.curselection()
                print(NUM)
                for Number in NUM:
                    print(Number)
                    Key = listbox.get(Number)
                    label.configure(text="正在播放:" + Key)
t = threading.Thread(target=Playing_now)#开启线程执行播放音乐函数
t.setDaemon(bool)#主进程结束子线程也结束
t.start()


label.pack(fill=X)
scale.pack(fill=X)
shumxu.pack(fill=X)
fm1.pack(fill=X)
fm2.pack(fill=X)

canvas.create_window(100, 10, width=200, height=32,
                     window=fm1)
canvas.create_window(100, 42, width=200, height=45,
                     window=scale)
canvas.create_window(100, 160, width=200, height=190,
                     window=fm2)
canvas.create_window(100, 360, width=200, height=20,
                     window=label)
canvas.create_window(100, 265, width=200, height=20,
                     window=shumxu)

# window()#调用窗口

# playmusic()#调用播放函数
# 进入消息循环1
top.mainloop()


```
