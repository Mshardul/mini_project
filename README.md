from tkinter import *
import tkinter.messagebox
import csv
import os
import matplotlib.pyplot as plt

#functions to check which button is pressed

    

#functions to fetch values and show graph
def fetch2():
    if(rb.get()==1):
        x='male'
    else:
        x='female'
    p=os.path.join('Baby Names 1944-2013',x)
    y=int(e1.get())
    #n=2013-e1.get()
    lst_year=[] #empties the list
    lst_count=[]
    lst_count2=[]
    f=(e2.get()).split(';')
    f1=f[0]
    f2=f[1]
    while y<2014:
        lst_year.append(y)
        q=p+'_cy'+str(y)+'_top.csv'
        f=open(q,'r')
        f_r=csv.reader(f)
        count1=0
        count2=0
        for i in f_r:
            if i[0]==f1.upper():
                count1=1
                ip=i[1]
                ip2=ip.split('=')
                ipm=ip2.pop()
                lst_count.append(int(ipm))
            if i[0]==f2.upper():
                count2=1
                ip=i[1]
                ip2=ip.split('=')
                ipm=ip2.pop()
                lst_count2.append(int(ipm))
            if count2==1 and count1==1:
                break
        if count1==0:
            lst_count.append(int(count1))
        if count2==0:
            lst_count2.append(int(count2))
        y=y+1
    print(f1)
    print(lst_count)
    print(f2)
    print(lst_count2)
    plt.title('POPULARITY GRAPH OF "%s" FROM THE YEAR %d TO 2013' %(e2.get().upper(),int(e1.get())))
    plt.xlabel('NO. OF CHILDREN')
    plt.ylabel('YEAR')
    plt.plot(lst_year,lst_count, label=f1)
    plt.plot(lst_year,lst_count2, label=f2)
    plt.legend()
    plt.show()

def fetch():
    if(rb.get()==1):
        x='male'
    else:
        x='female'
    p=os.path.join('Baby Names 1944-2013',x)
    y=int(e1.get())
    #n=2013-e1.get()
    j=0
    lst_year=[] #empties the list
    lst_count=[]
    while y<2014:
        lst_year.append(y)
        q=p+'_cy'+str(y)+'_top.csv'
        f=open(q,'r')
        f_r=csv.reader(f)
        count=0
        for i in f_r:
            if i[0]==e2.get().upper():
                count=1
                ip=i[1]
                ip2=ip.split('=')
                ipm=ip2.pop()
                lst_count.append(int(ipm))
                break
        if count==0:
            lst_count.append(int(count))
        y=y+1
    plt.title('POPULARITY GRAPH OF "%s" FROM THE YEAR %d TO 2013' %(e2.get().upper(),int(e1.get())))
    plt.xlabel('NO. OF CHILDREN')
    plt.ylabel('YEAR')
    plt.plot(lst_year,lst_count)
    plt.show()



#function to check the inserted values            
def click(choice):
    cl=0
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

    if rb.get()==0:
        messagebox.showinfo(title='please chose male/female in "gender" tab')

    if cl==1 and rb.get()!=0 and e2.get()!='':
        if choice==1:
            fetch()
        elif choice==2:
            fetch2()
    

##MAIN PROGRAM        
root=Tk()

#NAME
l2=Label(root, text='NAME')
l2.pack(fill=X)#side=LEFT)
e2=Entry(root)
e2.pack(pady=10)


#YEAR
l1=Label(root, text='YEAR')
l1.pack(fill=X)#side=LEFT)
e1=Entry(root)
e1.pack(pady=10)

#GENDER
rb=IntVar()

rb1=Radiobutton(root, text='Male', variable=rb, value=1)
rb1.pack()
rb2=Radiobutton(root, text='Female', variable=rb, value=2)
rb2.pack()

#LIST OF YEAR AND COUNTS
lst_year=[]
lst_count=[]
lst_count2=[]

#GO
graph=Button(root, text='GRAPH', command=lambda: click(1))
graph.pack(padx=100, pady=10)

#COMPARE
comp=Button(root, text='COMPARE', command=lambda: click(2))
comp.pack(pady=10)






