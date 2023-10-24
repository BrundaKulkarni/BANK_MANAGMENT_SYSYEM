# BANK_MANAGMENT_SYSYEM
import sqlite3 as db
import random

con = db.connect("bank_project.db")

# to create customer table
cursor = con.cursor()
cursor.execute("CREATE TABLE IF NOT EXISTS customer (cust_acc_no INTEGER PRIMARY KEY AUTOINCREMENT,name varchar(30),address varchar(100),contact int(10),password int(6),age int(3),adhar_no int(12),acc_type varchar(30),balance float(10))")

con.commit()
con.close()


def withdrawl(cust_acc_no, amount):
    con = db.connect("bank_project.db")
    cursor = con.cursor()

    cursor.execute("UPDATE customer SET balance = balance - ? WHERE cust_acc_no = ?", (amount, cust_acc_no))
    con.commit()
    con.close()


def deposit(cust_acc_no, amount):
    con = db.connect("bank_project.db")
    cursor = con.cursor()

    cursor.execute("UPDATE customer SET balance = balance + ? WHERE cust_acc_no = ?", (amount, cust_acc_no))
    con.commit()

    con.close()


def balStatus(cust_acc_no):
    con = db.connect("bank_project.db")
    cursor = con.cursor()

    cursor.execute("SELECT balance FROM customer WHERE cust_acc_no = "+(cust_acc_no,))
    result = cursor.fetchone()
    con.close()
    if result:
        return result[0]
    else:
        return None


def newAcc(name, address, contact, password, age, adhar_no, acc_type, balance):
    con = db.connect("bank_project.db")
    cursor = con.cursor()
    cust_acc_no = random.randint(1,1000)
    cursor.execute("INSERT INTO customer (cust_acc_no,name,address,contact,password,age,adhar_no,acc_type,balance) VALUES (?,?,?,?,?,?,?,?,?)", (cust_acc_no,name, address, contact, password, age, adhar_no, acc_type, balance))

    con.commit()
    con.close()
    print(f"hello {name} your account is created successfully with account no {cust_acc_no}!!!!!")


def seeDetails(cust_acc_no):
    con = db.connect("bank_project.db")
    cursor = con.cursor()
    cursor.execute("select * from customer where cust_acc_no = "+cust_acc_no)
    data = cursor.fetchall()
    for row in data:
        print(row)

    con.commit()
    con.close()


title = """ 
  _______   _                      _           ____              _    
 |__   __| (_)                    | |         |  _ \            | |   
    | |_ __ _ _ __ ___  _   _ _ __| |_ _   _  | |_) | __ _ _ __ | | __
    | | '__| | '_ ` _ \| | | | '__| __| | | | |  _ < / _` | '_ \| |/ /
    | | |  | | | | | | | |_| | |  | |_| |_| | | |_) | (_| | | | |   < 
    |_|_|  |_|_| |_| |_|\__,_|_|   \__|\__, | |____/ \__,_|_| |_|_|\_\
                                        __/ |                         
                                       |___/                          
"""

print(title)
print("****____Trimurty Bank____****")


while True:
    print("\nMain Menu")
    print("1. open new account")
    print("2. Deposit")
    print("3. Balance Status")
    print("4. withdraw")
    print("5. See Details")
    print("6. Exit")

    ch = int(input("Enter your choice = "))
    if ch == 1:
        print("****Hello welcome to Trimurty Bank ****")
        name = input("Enter your name : ")
        address = input("Enter your address : ")
        contact = int(input("Enter mobile number : "))
        if contact <= 10:
            print("CONTACT CREATED SUCCESSFULLY")
        else:
            var = ("ENTER YOUR CONTACT NUMBER")

        password = input("Enter pin : ")
        age = int(input("Enter your age : "))
        if age >= 18:
            print("VALID AGE")
        else:
            print("Age should be more than 18")

        adhar_no = int(input("Enter your adhar number : "))
        if len(str(adhar_no)) != 12:
            print("INVALID ADHAR NUMBER...PLZ RECHECK")
        else:
            print("THANKU FOR UR ADHAR NUMBER...!!!")

        acc_type = input("Enter account type : ")
        balance = input("Enter the initial balance : ")
        newAcc(name, address, contact, password, age, adhar_no, acc_type, balance)

    elif ch == 2:
        cust_acc_no = input("Enter your account number : ")
        amount = input("Enter amount to deposite :")
        deposit(cust_acc_no, amount)
        print("Money Deposited successfully!!!!!")

    elif ch == 3:
        cust_acc_no = input("Enter your account number : ")
        balance = balStatus(cust_acc_no)
        if balance is not None:
            print(f"Account balance: {balance}")
        else:
            print("Account not found.")

    elif ch == 4:
        cust_acc_no = input("Enter your account number : ")
        amount = input("Enter amount to withdraw : ")
        withdrawl(cust_acc_no, amount)
        print("Withdrawl successfully!!!!!")

    elif ch == 5:
        cust_acc_no = input("Enter account number to see details : ")
        seeDetails(cust_acc_no)
    elif ch == 6:
        print("Thanks for using our service. Do come again.")
        exit()
    else:
        print("Please check your input...")
