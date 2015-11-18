# mini_project
from tkinter import *
import tkinter.messagebox
import csv
import os

#function

def fetch():#x):
    if(rb.get()==1):
        x='male'
    else:
        x='female'
    p=os.path.join('Baby Names 1944-2013',x)
    p=p+'_cy'+e1.get()+'_top.csv'
    print(p)
    f=open(p,'r')
    j=0
    a='list of top '
    a=a+e2.get()+' '+x+' names in the year '+e1.get()
    Label(root, text=(a), anchor=CENTER).pack(fill=X)
    for i in f:
        if j==0:
            j=j+1
            continue
        elif j<int(e2.get()):
            j=j+1
            Label(root, text=(j,i)).pack()
            
           # Label(rf, text=(j,i.2,i[3])).pack()
        else:
            break
    f.close()
    
def click():
    cl=0
    #gender='a'
    if e1.get()=='':
        messagebox.showinfo(title='please enter the value in "year" tab')
    elif int(e1.get())<1944:
        messagebox.showinfo(title='record before 1944, not present')
    elif int(e1.get())>2013:
        messagebox.showinfo(title='record after 2013, not present')
    else:
        cl=1

    if e2.get()=='':
        messagebox.showinfo(title='please enter the value in "top" tab')

    if rb.get()==1:
        #gender='male'
        print('you chose male')# {0}',gender)
    elif rb.get()==2:
        #gender='female'
        print('you chose female')# {0}',gender)
    else:
        messagebox.showinfo(title='please chose male/female in "gender" tab')

    if cl==1 and rb.get()!=0 and e2.get()!='':
        fetch()


#prog
root=Tk()
root.resizable(0,0)

'''
#frames
lf=Frame(root)
lf.pack()#side=LEFT, fill=X)
rf=Frame(root, bd=10)
rf.pack(side=LEFT, fill=X)
'''
'''
#YEAR_LABEL-FRAME
labelframe1 = LabelFrame(lf, text="YEAR", padx=100, pady=50)

labelframe1.pack(fill="both", expand="no")

e1=Entry(labelframe1)
e1.pack()
'''

#YEAR
l1=Label(root, text='YEAR')
l1.pack(fill=X)#side=LEFT)
e1=Entry(root)
e1.pack(padx=5)

#GENDER
rb=IntVar()

rb1=Radiobutton(root, text='Male', variable=rb, value=1)
rb1.pack()
rb2=Radiobutton(root, text='Female', variable=rb, value=2)
rb2.pack()

'''
#TOP_LABEL-FRAME
labelframe2 = LabelFrame(lf, text="TOP")
labelframe2.pack(fill="both", expand="no")

e2=Entry(labelframe2)
e2.pack(side=LEFT)
'''

#TOP
l2=Label(root, text='TOP')
l2.pack()
e2=Entry(root)
e2.pack()


#GO
go=Button(root, text='GO', command=click)
go.pack()

#SCROLLBAR
sb=Scrollbar(root, orient=VERTICAL, elementborderwidth=5)

#STATUS BAR
st = Label(root, text='created by: shardul lingwal', bd=2, relief=SUNKEN, anchor=W)
st.pack(side=BOTTOM, fill=X)



