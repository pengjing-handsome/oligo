# oligo
# coding=utf-8
from tkinter import *
import tkinter as tk
from tkinter import ttk
from tkinter import Menu
from tkinter import scrolledtext
from tkinter import filedialog
from tkinter import messagebox as mBox
import pandas as pd

global  filename

def clean():
    name.delete(1.0, END)
    name0.delete(1.0, END)
    name1.delete(1.0, END)
    name2.delete(1.0, END)
    name3.delete(1.0, END)


def save2():
    msg = name.get('1.0',END).strip()
    msg = msg.upper()
    list0 = msg.splitlines()
    list1 = name0.get('1.0',END).strip().splitlines()
    if(len(list1) == 0):
        mBox.showerror('Error', '嘿，你输入的序列没有通过检测或还没有输入序列，请你重新输入序列!')
        return
    if(not check(list0)):
        return
    else:
        f=filedialog.asksaveasfilename(initialfile="未命名",defaultextension=".csv")
        if(len(f) == 0):
            return
        else:
            style = ["引物名称", "靶序列", " +G", "oligo1", "oligo2"]
            list0 = msg.splitlines()
            list4 = name3.get('1.0', END).strip().splitlines()
            list1 = name0.get('1.0', END).strip().splitlines()
            list2 = name1.get('1.0', END).strip().splitlines()
            list3 = name2.get('1.0', END).strip().splitlines()
            result = []
            index = []
            row = 1
            for i in range(0, len(list0)):
                index.append(row)
                row += 1
                temp = []
                temp.append(list4[i])
                temp.append(list0[i])
                temp.append(list1[i])
                temp.append(list2[i])
                temp.append(list3[i])
                result.append(temp)
            test = pd.DataFrame(columns=style,data=result,index=index)
            test.to_csv(f,encoding='GBK')

def check(list):
    line = 1
    for i in list:
        if len(i) % 20 != 0:
            mBox.showerror('Error','长度不符合，请输入20个碱基!')
            return False
        else:
            for j in range(0, len(i)):
                if i[j] == "G":
                    continue
                elif i[j] == "C":
                    continue
                elif i[j] == "A":
                    continue
                elif i[j] == "T":
                    continue
                else:
                    mBox.showerror('Error', '你序列中的第'+repr(line)+'行第'+repr(j+1)+'位不是A、T、G、C四个字母:\n请重新输入，进行修改!')
                    return False
            line+=1
    return True

def start(): #acgtacgtacgtacgtacgt
    name0.delete(1.0, END)
    name1.delete(1.0, END)
    name2.delete(1.0, END)
    target = name.get('1.0', END).strip()
    target = target.upper()
    list = target.splitlines()
    # mBox.showerror('Error', '序列中只允许存在A、T、G、C四个字母:\n请重新输入!')
    if (not check(list)):
        return
    else:
        row = 1
        for i in list:
            if i[0] !='G':
                y = "G"
                i = y + i
            name0.insert(END,i.upper()+'\n')
            name1.insert(END,'5-CACC' + i.upper() + '-3' + '\n')
            name2.insert(END,'5-AAAC' + DNA_reverse(DNA_complement(i)) + '-3\n')
            row +=1


def DNA_complement(target):
            target = target.upper()
            target = target.replace('A', 't')
            target = target.replace('T', 'a')
            target = target.replace('C', 'g')
            target = target.replace('G', 'c')
            return target.upper()

def DNA_reverse(target):
    target = target.upper()
    return target[::-1]

if __name__ == "__main__":
    root=Tk()
    root.title("引物设计")
    root.geometry("550x350")
    root.resizable(0, 0)
    menuBar = Menu(root)
    root.config(menu=menuBar)
    fileMenu = Menu(menuBar, tearoff=0)#file菜单
    fileMenu.add_command(label="New")
    fileMenu.add_command(label="Open")
    menuBar.add_cascade(label="File", menu=fileMenu)
    l=Label(root,text=("靶序列*:"),font=("宋体,12"),width=("10"),height=("3"),fg='red')#,bg='blue'
    l.grid(column=0, row=1)
    name = tk.Text(root,height=4,width = 40)
    name.grid(column=1,row=1)
    Button(root,text="开始",command=start).grid(column=2, row=0)
    Button(root,text="清除",command=clean).grid(column=4, row=0)
    Button(root,text="保存",command=save2).grid(column=5, row=0)

    l1=Label(root,text=("+G:"),font=("宋体,12"),width=("10"),height=("3"))
    l1.grid(column=0, row=2)
    name0 = Text(root,height=4,width = 40)
    name0.grid(column=1,row=2)

    l2=Label(root,text=("oligo1:"),font=("宋体,12"),width=("10"),height=("3"))
    l2.grid(column=0, row=3)
    name1 = Text(root,height=4,width = 40)
    name1.grid(column=1,row=3)

    l3=Label(root,text=("oligo2:"),font=("宋体,12"),width=("10"),height=("3"))
    l3.grid(column=0, row=4)
    name2 = Text(root,height=4,width = 40)
    name2.grid(column=1,row=4)

    l4=Label(root,text=("引物名称*:"),font=("宋体,12"),width=("10"),height=("3"),fg='red')#,bg='blue'
    l4.grid(column=0, row=0)
    name3 = Text(root,height=4,width = 40)
    name3.grid(column=1,row=0)

    root.mainloop()

