from tkinter import *
import tkinter.messagebox
from tkinter.messagebox import *
from tkinter.ttk import *
import sqlite3
conn=sqlite3.Connection('malu')
rootp=Tk()
rootp.title("Malvika Thakur RA1711008010210")
Label(rootp,text="Malvika's Airline Booking System",font="Bold 20").grid(row=0,column=0)
b=PhotoImage(file="unnamed.png")
Label(rootp,image=b).grid(row=1,column=0)
cur = conn.cursor()
cur.execute("CREATE TABLE TABLE1(source CHAR(20),dest CHAR(20),day CHAR(20),time CHAR(20)) ")
cur.execute("INSERT INTO TABLE1 VALUES('Delhi','Chennai','Sunday','1:00 AM')")
cur.execute("INSERT INTO TABLE1 VALUES('Delhi','Mumbai','Monday','1:00 AM')")
cur.execute("INSERT INTO TABLE1 VALUES('Delhi','Bangalore','Tuesday','1:00 PM')")
cur.execute("INSERT INTO TABLE1 VALUES('Chennai','Delhi','Wednesday','7:00 AM')")
cur.execute("INSERT INTO TABLE1 VALUES('Chennai','Delhi','Wednesday','4:00 PM')")
cur.execute("CREATE TABLE TABLE2(source char(20),dest char(20),adno number,day char(20),time char(20))")

def fun_search():
    # rootp.destroy()
    root4 = Tk()
    root4.title("Welcome,Search Flights")
    Label(root4, text="From").grid(row=0, column=0)
    w1 = Combobox(root4, height=5, width=15, values=["Delhi", "Mumbai", "Chennai", "Bangalore"])
    w1.grid(row=0, column=1)
    Label(root4, text="To").grid(row=1, column=0)
    w2 = Combobox(root4, height=5, width=15, values=["Delhi", "Mumbai", "Chennai", "Bangalore"])
    w2.grid(row=1, column=1)
    Label(root4, text="Choose day of travel").grid(row=2, column=0)
    w3 = Combobox(root4, text="choose day", height=5, width=15,
                  values=["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"])
    w3.grid(row=2, column=1)
    Label(root4, text="Choose time of flight").grid(row=3, column=0)
    w4 = Combobox(root4, text="choose time", height=5, width=15,
                  values=["1:00 AM","7:00 AM","1:00 PM","4:00 PM","9:00 PM"])
    w4.grid(row=3, column=1)

    def fun_search1():
        a = w1.get()
        b = w2.get()
        c = w3.get()
        d = w4.get()
        cur = conn.cursor()
        if a == '' or b == '' or c == '' or d == '':
            tkinter.messagebox.showerror("Error", "Cant leave any field empty")
        else:
            if a != b:
                cur.execute("SELECT * FROM TABLE1 where source=? and dest=? and day=? and time=?", (a, b, c, d,))
                conn.commit()
                e = cur.fetchall()
                print(e)
                if not e:
                    showerror("SORRY", "Flight is not available")
                else:
                    showinfo("SEARCH SUCCESSFUL", "Flight is available")
            else:
                showerror("Oops", "boarding and destination can't be same")
        root4.destroy()
    Bs = Button(root4, text="search", command=fun_search1).grid(row=4, column=0)
    #root4.mainloop()SO AFTER SEARCHING IT CLOSES


def fun_book():
    # rootp.destroy()
    root = Tk()
    root.title('BOOKING')
    Label(root, text="From").grid(row=1, column=0)
    w = Combobox(root, height=5, width=15, values=["Delhi", "Mumbai", "Chennai", "Bangalore"])
    w.grid(row=1, column=1)
    Label(root, text='To').grid(row=2, column=0)
    w1 = Combobox(root, height=5, width=15, values=["Delhi", "Mumbai", "Chennai", "Bangalore"])
    w1.grid(row=2, column=1)
    Label(root, text='Enter last 4 digit of your Aadhar Number').grid(row=3, column=0)
    w4 = Entry(root, width=18)
    w4.grid(row=3, column=1)
    Label(root, text="Choose day of travel").grid(row=5, column=0)
    w2 = Combobox(root, text="choose day", height=5, width=15,
                  values=["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"])
    w2.grid(row=5, column=1)
    Label(root, text="Choose time of your flight").grid(row=6, column=0)
    w3 = Combobox(root, height=5, width=15, values=["1:00 AM", "7:00 AM", "1:00 PM", "4:00 PM", "9:00 PM"])
    w3.grid(row=6, column=1)

    def fun_book1():
        a = w.get()
        b = w1.get()
        c = w4.get()
        f = w2.get()
        g = w3.get()
        x = (a, b, c, f, g)
        cur = conn.cursor()
        if a == '' or b == '' or c == '' or f == '' or g == '':
            showerror("OOPS", "you can't leave any field empty")
        else:
            cur.execute("SELECT * FROM TABLE1 where source=? and dest=? and day=? and time=?", (a, b, f, g,))
            e=cur.fetchall()
            if not e:
                showerror("SORRY", "Flight is not available")
            else:
                if a != b:
                    cur.execute("insert into TABLE2 values(?,?,?,?,?)", x)
                    tkinter.messagebox.showinfo("CONGRATS", "Your Seat Has Been Reserved")
                    conn.commit()
                    cur.execute("select source, dest, day, time from TABLE2 where adno=(?)", (c,))
                    tkinter.messagebox.showinfo("Records", cur.fetchall())
                else:
                    tkinter.messagebox.showerror("Error", "You Can't Choose Same City")
        root.destroy()
    Bi = Button(root, text="Book", command=fun_book1).grid(row=7, column=1)
    #root.mainloop()


def fun_cancel():
    # rootp.destroy()
    root2 = Tk()
    root2.title("CANCEL")
    Label(root2, text="Enter last 4 digit of your Aadhar Number").grid(row=0, column=0)
    e1 = Entry(root2)
    e1.grid(row=0, column=1)
    Label(root2, text="Select Boarding").grid(row=1, column=0)
    w2 = Combobox(root2, height=5, width=17, values=["Delhi", "Mumbai", "Chennai", "Bangalore"])
    w2.grid(row=1, column=1)
    Label(root2, text="Select Day").grid(row=2, column=0)
    w3 = Combobox(root2, height=5, width=17, values=["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"])
    w3.grid(row=2, column=1)
    Label(root2, text="Select Time").grid(row=3, column=0)
    w4 = Combobox(root2, height=5, width=17, values=["1:00 AM", "7:00 AM", "1:00 PM", "4:00 PM", "9:00 PM"])
    w4.grid(row=3, column=1)

    def fun_cancel1():
        a = e1.get()
        b = w2.get()
        c = w3.get()
        d = w4.get()
        cur = conn.cursor()
        x = str(a)
        y = str(c)
        conn.commit()
        if d == '' or b == '' or c == '' or d == '':
            tkinter.messagebox.showerror("Oops", "You Can't Leave Any Field Empty")
        else :
            cur.execute("select source, dest, day, time from TABLE2 where adno=(?)", (a,))
            e = cur.fetchall()
            print(e)
            if not e:
                showerror("SORRY", "There Is No Such Flight!")
            else:
                cur.execute("delete from TABLE2 where adno=(?) and source=(?) and day=(?) and time=(?)", (a, b, c, d,))
                cur.execute("select * from TABLE2")
                tkinter.messagebox.showinfo("CANCELLATION SUCCESSFUL","Your reservation is cancelled.")
                print(cur.fetchall())
        root2.destroy()
    Bc = Button(root2, text="Cancel Reservation", command=fun_cancel1).grid(row=4, column=0)
   # root2.mainloop()


B1=Button(rootp,text="Search Flight",command=fun_search).grid(row=2,column=0)
B2=Button(rootp,text="Book Flight",command=fun_book).grid(row=3,column=0)
B3=Button(rootp,text="Cancel Booking",command=fun_cancel).grid(row=4,column=0)
rootp.mainloop()