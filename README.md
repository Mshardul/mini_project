from tkinter import *
from tkinter import ttk
import tkinter.messagebox
import csv
import os
import tkinter.scrolledtext as tkst
import matplotlib.pyplot as plt

#------------------------------------------------------------------functions_list

def fetch_names():
    if(rb.get()==1):
        x='male'
    else:
        x='female'
    p=os.path.join('Baby Names 1944-2013',x)
    p=p+'_cy'+e1.get()+'_top.csv'
    f=open(p,'r')
    j=0
    a='\nLIST OF TOP '
    a=a+(e2.get()).upper()+' '+x.upper()+' NAMES IN THE YEAR '+e1.get()+'\n\n'
    for i in f:
        if j==0:
            j=j+1
            continue
        elif j<=int(e2.get()):
            j=j+1
            x=i.split(',')
            x1=x[0]
            x2=x[1]
            x3=x[2]
            x11=x1.split('"')[1]
            x22=x2.split('"')[1]
            x33=x3.split('"')[1]
            x333=x33.split('=')
            y=len(x333)
            x3333=x333[y-1]
            x34=x3.split('"')[2]
            z='Position = '+str(j-1)+';  Name = '+x11+';  Amount = '+x22+';'+x34
            a=a+z
        else:
            break
    a=a+'---------------------------------------------'
    save_history(a)
    editArea.insert(INSERT,a)
    f.close()

def extraction():
    cl=0
    cl2=0
    try:
        if e1.get()=='':
            messagebox.showinfo(title='YEAR', message='please enter a year betwwn 1944 and 2013')
        elif int(e1.get())<1944:
            messagebox.showinfo(title='YEAR', message='record before 1944, not present')
        elif int(e1.get())>2013:
            messagebox.showinfo(title='YEAR',message='record after 2013, not present')
        else:
            cl=1
    except:
        messagebox.showinfo(title='YEAR',message='year must be an integer')

    try:
        if e2.get()=='':
            messagebox.showinfo(title="TOP", message='please enter the number of names')
        elif int(e1.get())<0:
            messagebox.showinfo(title='TOP',message='top must be greater than 0')
        else:
            cl2=1
    except ValueError:
        messagebox.showinfo(title='TOP',message='top must be an integer')####isnt working

    if rb.get()==0:
        messagebox.showinfo(title="GENDER",message='please chose male/female in "gender" tab')

    if cl==1 and rb.get()!=0 and e2.get()!='' and cl2==1:
        fetch_names()


#----------------------------------------------------------------functions_graph
def fetch_graph2():
    if(rb.get()==1):
        x='male'
    else:
        x='female'
    p=os.path.join('Baby Names 1944-2013',x)
    y=int(e1.get())
    #n=2013-e1.get()
    lst_year=[] #empties the list
    lst_count=[]
    m=(e3.get()).split(';')
    lngth=0
    for i in m:
        lngth=lngth+1
    if e12.get()=='':
        year_to=2013
    else:
        year_to=int(e12.get())
    while y<=year_to:
        lst_year.append(y)
        for j in m:
            
            q=p+'_cy'+str(y)+'_top.csv'
            f=open(q,'r')
            f_r=csv.reader(f)
            count1=0
            for i in f_r:
                if i[0]==j.upper():
                    
                    count1=1
                    ip=i[1]
                    ip2=ip.split('=')
                    ipm=ip2.pop()
                    lst_count.append(int(ipm))
                if count1==1: #and count2==1:
                    break
            if count1==0:
                lst_count.append(int(count1))
        y=y+1
    l=0
    #print(lst_count)
    maximum=year_to-int(e1.get())
    print(maximum)
    #lst_final=[]
    nm=''
    while l<lngth:
        lst_final=[]
        nm=m[l]
        z=0
        while z<=maximum:
            ele=lst_count[z*lngth+l]
            lst_final.append(ele)
            z=z+1
        print('name = ',nm)
        print('amount = ',lst_final)
        plt.plot(lst_year,lst_final, label=nm)
        l=l+1
        
    plt.title('POPULARITY GRAPH OF "%s" FROM THE YEAR %d TO %d' %(e3.get().upper(),int(e1.get()),year_to))
    plt.xlabel('NO. OF CHILDREN')
    plt.ylabel('YEAR')
    #plt.plot(lst_year,lst_count, label=f1)
    #plt.plot(lst_year,lst_count2, label=f2)
    plt.legend()
    plt.show()
    #else:
     #   messagebox.showinfo(title="NAME",message='you must input exactly two names')

def plot(choice):
    if rb.get()==1:
        x='male'
    else:
        x='female'
    p=os.path.join('Baby Names 1944-2013',x)
    y=int(e1.get())
    j=0
    lst_year=[] #empties the list
    lst_count=[]
    if e12.get()=='':
        year_to=2013
    else:
        year_to=int(e12.get())
    while y<=year_to:
        lst_year.append(y)
        q=p+'_cy'+str(y)+'_top.csv'
        f=open(q,'r')
        f_r=csv.reader(f)
        count=0
        for i in f_r:
            if i[0]==e3.get().upper():
                count=1
                ip=i[1]
                ip2=ip.split('=')
                ipm=ip2.pop()
                lst_count.append(int(ipm))
                break
        if count==0:
            lst_count.append(int(count))
        y=y+1
    if choice==1:
        plt.title('POPULARITY GRAPH OF "%s" FROM THE YEAR %d TO %d' %(e3.get().upper(),int(e1.get()),year_to))
        plt.xlabel('NO. OF CHILDREN')
        plt.ylabel('YEAR')
        plt.plot(lst_year,lst_count)
        plt.show()
    elif choice==2:
        plt.title('POPULARITY BAR-CHART OF "%s" FROM THE YEAR %d TO %d' %(e3.get().upper(),int(e1.get()),year_to))
        plt.xlabel('NO. OF CHILDREN')
        plt.ylabel('YEAR')
        plt.bar(lst_year,lst_count)
        plt.show()
    elif choice==3:
        plt.title('POPULARITY PIE-CHART OF "%s" FROM THE YEAR %d TO %d' %(e3.get().upper(),int(e1.get()),year_to))
        plt.xlabel('NO. OF CHILDREN')
        plt.ylabel('YEAR')
        plt.pie(lst_count,labels=lst_year)
        plt.show()
    

#function to check the inserted values            
def graph():
    cl=0
    cl2=0
    if e1.get()=='':
        messagebox.showinfo(title='YEAR', message='please enter a year betwwn 1944 and 2013')
    elif int(e1.get())<1944:
        messagebox.showinfo(title='YEAR', message='record before 1944, not present')
    elif int(e1.get())>2013:
        messagebox.showinfo(title='YEAR',message='record after 2013, not present')
    else:
        cl=1

    if e12.get()=='':
        cl2=1
    elif int(e1.get())>int(e12.get()):
        messagebox.showinfo(title='YEAR',message="'year from' is greater than 'year to'")
    elif int(e12.get())<1944:
        messagebox.showinfo(title='YEAR', message='record before 1944, not present')
    elif int(e12.get())>2013:
        messagebox.showinfo(title='YEAR',message='record after 2013, not present')
    else:
        cl2=1

    if e3.get()=='':
        messagebox.showinfo(title='NAME',message='please enter the value in "name" tab')

    if rb.get()==0:
        messagebox.showinfo(title='GENDER',message='please chose male/female in "gender" tab')

    if cl==1 and rb.get()!=0 and e3.get()!='' and cl2==1:
        if var1.get()=='GRAPH':
            plot(1)
        elif var1.get()=='BAR':
            plot(2)
        elif var1.get()=='PIE':
            plot(3)




#-------------------------------------------------------------OTHER FUNCTIONS
def close():
    root.destroy()

def res():
    editArea.delete("1.0",END)
    empty=0
    
def save_history(a):
    p='history.txt'
    f=open(p,'a')
    f.write(a)
    f.close()

def hist():
    p='history.txt'
    f=open(p,'r')
    editArea.insert(INSERT,'\n-----HISTORY-----\n')
    for i in f:
        editArea.insert(INSERT,i)
    f.close()


#---------------------------------------------------------------PROG
        
root=Tk()
root.resizable(0,0)
root.title("MINI-PROJECT")


#YEAR
l1=Label(root, text='YEAR', font='bold', fg='red')
l1.grid(row=0, column=0, pady=10, sticky=E)
e1=Entry(root)
e1.grid(row=0, column=1, pady=10, padx=20)

#GENDER
rb=IntVar()
rb1=Radiobutton(root, text='Male', variable=rb, value=1, font='bold')
rb1.grid(row=2, column=0, pady=10)
rb2=Radiobutton(root, text='Female', variable=rb, value=2, font='bold')
rb2.grid(row=2, column=1, pady=10)

#TOP
l2=Label(root, text='TOP', font='bold', fg='red')
l2.grid(row=3, column=0, pady=10, sticky=E)
e2=Entry(root)
e2.grid(row=3, column=1, pady=10, padx=20)

#EXTRACT LIST
go=Button(root, text='EXTRACT LIST', command=extraction, font='bold', fg='blue')
go.grid(row=4, column=0, pady=10, padx=10, sticky=E)

#RESET
rst=Button(root, text='RESET', command=res, font='bold', fg='blue')
rst.grid(row=4, column=1)

#text-box
editArea = tkst.ScrolledText(master=root, wrap=WORD, width=50, height=10, font='bold')
editArea.grid(row=0, column=2, columnspan=6, rowspan=5)

#NAME
l3=Label(root, text='NAME', font='bold', fg='red')
l3.grid(row=6, column=0, pady=20, sticky=E)
e3=Entry(root)
e3.grid(row=6, column=1)

#YEAR TO
l12=Label(root, text='YEAR TO', font='bold', fg='red')
l12.grid(row=6, column=2, sticky=E)
e12=Entry(root)
e12.grid(row=6, column=3, padx=20)

#LIST OF YEAR AND COUNTS
a=''
lst_year=[]
lst_count=[]
lst_count2=[]

#GRAPHICAL
var1=StringVar()
drop=OptionMenu(root,var1,'GRAPH','BAR','PIE')
drop.grid(row=7,column=0, sticky=E)
grph=Button(root, text='OK', command=graph, font='bold', fg='blue')
grph.grid(row=7, column=1)

compr=Button(root, text='COMPARISION', command=fetch_graph2, font='bold', fg='blue')
compr.grid(row=7, column=2)

#VIEW HISTORY
his=Button(root, text='HISTORY', command=hist, font='bold', fg='blue')
his.grid(row=8, column=3, sticky=E)

#EXIT
ex=Button(root, text='EXIT', command=close, font='bold', fg='blue')
ex.grid(row=8, column=4, sticky=E)

#STATUS BAR
st = Label(root, text='created by: shardul lingwal', bd=2, relief=SUNKEN, anchor=W, font='bold')
st.grid(sticky=W, columnspan=6)

