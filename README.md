# py_crd
py
from tkinter import *
from tkinter import ttk
import mysql.connector as sql
#...Database class..
class Con:
    def __init__(self):
        self.db = sql.connect(host="localhost",user="root",password="",database="python_crude")
        self.cur=self.db.cursor()
        self.db.commit()
    def insert(self):
        try:
            self.cur.execute("INSERT INTO user2(id,name,city) VALUES(%s,%s,%s)",(uid.get(), unm.get(), city.get()))
            self.db.commit()
            self.show()
        except sql.IntegrityError:
            print("ID already exists!")
    def delete(self):
            self.cur.execute("DELETE FROM user2 where id=%s",(uid.get(),))
            self.db.commit()
            self.show()
            self.clear()
    def update(self):
        self.cur.execute("UPDATE user2 SET name=%s, city=%s WHERE id=%s",(unm.get(),city.get(),uid.get()))
        self.db.commit()
        self.show()
        self.clear()
    def show(self):
        self.cur.execute("SELECT * from user2")
        data=self.cur.fetchall()
        for child in lv.get_children():
            lv.delete(child)
        for row in data:
            lv.insert("", "end", values=row)
    def clear(self):
        uid.delete(0,END)
        unm.delete(0, END)
        city.delete(0,END)
# -- GUI Code ---            
ob = Con()
top = Tk()
top.geometry("500x400")
top.title("Student management")

Label(top,text="ID").place(x=20, y=20)
uid=Entry(top,width=25)
uid.place(x=100,y=20)

Label(top,text="name").place(x=20,y=60)
unm=Entry(top,width=25)
unm.place(x=100,y=60)

Label(top,text="city").place(x=20,y=100)
city=Entry(top,width=25)
city.place(x=100,y=100)

btn_insert = Button(top,text="Insert", width=10, bg="red",command=ob.insert)
btn_insert.place(x=100,y=140)

btn_update = Button(top,text="Update", width=10, bg="green",command=ob.update)
btn_update.place(x=200,y=140)

btn_delete = Button(top,text="Delete", width=10, bg="blue",command=ob.delete)
btn_delete.place(x=300,y=140)

columns = ("ID","Name", "City")
lv=ttk.Treeview(top, columns=columns, show="headings")
for col in columns:
    lv.heading(col,text=col)
    lv.column(col, width=120)
lv.place(x=20, y=200, width=460, height=180)

def select_row(event):
    selected = lv.focus()
    data = lv.item(selected, "values")

    if data:
        uid.delete(0, END)
        unm.delete(0, END)
        city.delete(0, END)

        uid.insert(0, data[0])
        unm.insert(0, data[1])
        city.insert(0, data[2])

lv.bind("<<TreeviewSelect>>", select_row)
ob.show()
top.mainloop()


