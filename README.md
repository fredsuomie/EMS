# EMS
Employee Management System

import mysql.connector

con = mysql.connector.connect(
    host="localhost",
    port=3306,
    user="root",
    password="Anonymous@13",
    database="emp",
    auth_plugin="mysql_native_password"
)


def addEmployee():
    Id = input("Enter Employee Id: ")

    if (check_employee(Id) == True):
        print("Employee already exsit \nTry Again\n")
        menu()
    else:
        Name = input("Enter Employee Name: ")
        Post = input("Enter Employee Post: ")
        Salary = input("Enter Employee Salary: ")
        data = (Id, Name, Post, Salary)

        sql = 'insert into emply values(%s,%s,%s,%s)'
        c = con.cursor()

        c.execute(sql, data)

        con.commit()
        print("Employee Added Successfully")
        menu()

def Promote_Employee():
    Id = input("Enter Employee Id: ")

    if(check_employee(Id) == False):
        print("Employee doesn't exist\nTry Again\n")
        menu()
    else:
        Amount = input("Enter increase in Salary: ")

        sql = 'select salary from emply where Id=%s'
        data = (Id,)
        c = con.cursor()

        c.execute(sql,data)

        r = c.fetchone()
        t = r[0]+Amount

        sql = 'update emply set salary=%s where Id=$s'
        d = (t, Id)

        c.execute(sql, d)

        con.commit()
        print("Employee Promoted")
        menu()

def Remove_Employee():
    Id = input("Enter Employee Id: ")

    if(check_employee(Id) == False):
        print("Employee does not exist\nTry Again\n")
        menu()
    else:

        sql = 'delete from emply where Id=%s'
        data = (Id,)
        c = con.cursor()

        con.commit()
        print("Employee Removed")
        menu()

def check_employee(Id,):

    sql = 'select * from emply where Id=%s'

    c = con.cursor(buffered=True)
    data = (Id,)

    r = c.rowcount
    if r == 1:
        return True
    else:
        return False

def Display_Employees():


    sql = 'select * from emply'
    c = con.cursor()

    c.execute(sql)

    r = c.fetchall()
    for i in r:
        print("Employee ID:",i[0])
        print("Employee Name:",i[1])
        print("Employee Post:",i[2])
        print("Employee Salary:",i[3])
        print("-----------------\
        ------------------------\
        -------------------------\
        -----------------")

    menu()

def menu():
    print("Welcome to Employee Management Record")
    print("Press")
    print("1. to Add Employee")
    print("2. to Remove Employee")
    print("3. to Promote Employee")
    print("4. to Display Employees")
    print("5. to Exit")

    ch = int(input("Enter your choice:"))
    if ch == 1:
        addEmployee()
    elif ch == 2:
        Remove_Employee()
    elif ch == 3:
        Promote_Employee()
    elif ch == 4:
        Display_Employees()
    elif ch == 5:
        exit(0)
    else:
        print("Invalid Choice")
        menu()

menu()
