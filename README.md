





import mysql.connector as a
# GLOBAL VARIABLES DECLARATION
myConnnection =""
cursor=""
userName=""
password =""
#MODULE TO CHECK MYSQL CONNECTIVITY
def MYSQLconnectionCheck ():


          global myConnection
          global userName
          global password
          userName = input("\n ENTER MYSQL SERVER'S USERNAME : ")
          password = input("\n ENTER MYSQL SERVER'S PASSWORD : ")

          myConnection=a.connect(host="localhost",user="root",passwd="12345")
          if myConnection:
                    print("\n CONGRATULATIONS ! YOUR MYSQL CONNECTION HAS BEEN ESTABLISHED !")
                    cursor=myConnection.cursor()
                    cursor.execute("CREATE DATABASE IF NOT EXISTS SMS")
                    cursor.execute("COMMIT")
                    cursor.close()
                    return myConnection
          else:
                    print("\nERROR ESTABLISHING MYSQL CONNECTION CHECK USERNAME AND PASSWORD !")

#MODULE TO ESTABLISHED MYSQL CONNECTION
def MYSQLconnection ():


          global userName
          global password
          global myConnection

          myConnection=a.connect(host="localhost",user="root",passwd
          ="12345", database="SMS" )
          if myConnection:
                    return myConnection
          else:
                    print("\nERROR ESTABLISHING MYSQL CONNECTION !")
                    myConnection.close()
#MODULE FOR NEW ADMISSION
def newStudent():

          if myConnection:

                    cursor=myConnection.cursor()
                    createTable ='''CREATE TABLE IF NOT EXISTS STUDENT(SNAME
                    VARCHAR(30),FNAME VARCHAR(30),MNAME VARCHAR(30)
                    ,PHONE VARCHAR(12), ADDRESS VARCHAR(100),SCLASS
                    VARCHAR(5),SSECTION VARCHAR(5),
                    SROLL_NO VARCHAR(5),SADMISSION_NO VARCHAR(10)
                    PRIMARY KEY)
                    '''
                    cursor.execute(createTable)

                    sname=input("\n ENTER STUDENT'S NAME : ")
                    fname=input(" ENTER FATHER'S NAME : ")
                    mname=input(" ENTER MOTHER'S NAME : ")
                    phone=input(" ENTER CONTACT NO. : ")
                    address=input(" ENTER ADDRESS : ")
                    sclass=input(" ENTER CLASS : ")
                    ssection=input(" ENTER SECTION : ")
                    sroll_no=input(" ENTER ROLL_NO : ")
                    sadmission_no=input(" ENTER ADMISSION_NO : ")

                    sql='''INSERT INTO
                    student(SNAME,FNAME,MNAME,PHONE,ADDRESS,SCLASS,SSECTION,SROLL_NO,SADMISSION_NO)
                    VALUES('{}','{}','{}','{}','{}','{}','{}','{}','{}')
                    '''.format(sname,fname,mname,phone,address,sclass,ssection,sroll_no,sadmission_no)
                    cursor.execute(sql)
                    cursor.execute("COMMIT")
                    cursor.close()
                    print("\nNew Student Enrolled Successfully !")


          else:
                    print("\nERROR ESTABLISHING MYSQL CONNECTION !")

#MODULE TO DISPLAY STUDENT'S DATA
def displayStudent():
          cursor=myConnection.cursor()
          if myConnection:
                    cursor.execute("SELECT * FROM STUDENT")
                    data=cursor.fetchall()
                    print(data)
                    cursor.close()
          else:
                    print("\nSomthing Went Wrong ,Please Try Again !")
#MODULE TO UPDATE STUDENT'S RECORD
def updateStudent():
          cursor=myConnection.cursor()
          if myConnection:
                    admission_no=input("ENTER ADMISSION NO")
                    sql="SELECT * FROM STUDENT WHERE SADMISSION_NO= %s"
                    cursor.execute(sql,(admission_no,))
                    data=cursor.fetchall()
          if data:
                    print("PRESS 1 FOR NAME")
                    print("PRESS 2 FOR CLASS")
                    print("PRESS 3 FOR ROLL NO")
                    choice=int(input("Enter Your Choice"))
                    if choice==1:
                              name=input("ENTER NAME OF THE STUDENT :")
                              sql='''UPDATE STUDENT SET SNAME= '{}' WHERE SADMISSION_NO = '{}'
                              '''.format(name,admission_no)
                              cursor.execute(sql)
                              cursor.execute("COMMIT")
                              print("NAME UPDATED")

                    elif choice == 2:
                              std=input("ENTER CLASS OF THE STUDENT :")
                              sql='''UPDATE STUDENT SET SCLASS= '{}' WHERE SADMISSION_NO='{}'
                              '''.format(std,admission_no)
                              cursor.execute(sql)
                              cursor.execute("COMMIT")
                              print("CLASS UPDATED")

                    elif choice==3:
                              roll_no=int(input("ENTER ROLL NO OF THE STUDENT :"))

                              sql='''UPDATE STUDENT SET SROLL_NO= {} WHERE
                              SADMISSION_NO = '{}' '''.format(roll_no,sadmission_no)
                              cursor.execute(sql,(roll_no,admission_no))
                              cursor.execute("COMMIT")
                              print("ROLL NO UPDATED")

                    else:
                              print("Record Not Found Try Again !")
                              cursor.close()
                    
                            


#MODULE TO ENTER MARKS OF THE STUDENT
def marksStudent () :
          if myConnection:

                    cursor=myConnection.cursor()
                    createTable ="""CREATE TABLE IF NOT EXISTS MARKS(SADMISSION_NO
                    VARCHAR(10) PRIMARY KEY,CS INT,ENGLISH INT ,MATH INT ,
                    PHYSICS INT,CHEMISTRY INT,TOTAL INT ,AVERAGE
                    DECIMAL)
                    """
                    cursor.execute(createTable)
                    admission_no=input("ENTER ADMISSION NO OF THE STUDENT :")
                    cs=int(input("\n ENTER MARKS OF HINDI : "))
                    english=int(input("\n ENTER MARKS OF ENGLISH : "))
                    math=int(input("\n ENTER MARKS OF MATH : "))
                    physics=int(input("\n ENTER MARKS OF SCIENCE : "))
                    chemistry=int(input("\n ENTER MARKS OF SOCIAL : "))
                    total = cs + english + math + physics + chemistry
                    average = total/5
                    sql='''INSERT INTO
                    MARKS(SADMISSION_NO,CS,ENGLISH,MATH,PHYSICS,CHEMISTRY,
                    TOTAL,AVERAGE) VALUES('{}',{},{},{},{},{},{},{}
                    '''.format(admission_no,cs,english,math,physics,chemistry)
                    cursor.execute(sql,values)
                    cursor.execute("COMMIT")
                    cursor.close()
                    print("\nMarks of the Student Entered Successfully !")

          else:
                    print("\nERROR ESTABLISHING MYSQL CONNECTION !")

#MODULE TO GENERATE REPORT CARD OF ALL STUDENTS

def reportCardAllStudent () :
          cursor=myConnection.cursor()
          if myConnection:
                    cursor.execute("SELECT * FROM MARKS")
                    data=cursor.fetchall()
                    print(data)
                    cursor.close()
          else:
                    print("\nSomthing Went Wrong ,Please Try Again !")
#MODULE TO GENERATE REPORT CARD OF ONE STUDENTS
def reportCardOneStudent():
          cursor=myConnection.cursor()
          if myConnection:
                    admission_no=input("ENTER ADMISSION NO OF THE STUDENT :")
                    cursor=myConnection.cursor()
                    sql="SELECT * FROM MARKS WHERE SADMISSION_NO= '{}'".format(admission_no)
                    cursor.execute(sql)
                    data=cursor.fetchall()
          if data:
                    print(data)
          else:
                    print("Record Not Found , Please Try Again !")
                    cursor.close()

         

#MODULE TO ENTER FEES OF THE STUDENTS
def feeStudent () :
          if myConnection:

                    cursor=myConnection.cursor()
                    createTable ="""CREATE TABLE IF NOT EXISTS FEES(SADMISSION_NO
                    VARCHAR(10) PRIMARY KEY,MONTH INT ,TUTION_FEES INT,
                    COMPUTER_FEES INT ,
                    MUSIC_FEES INT, TOTAL INT)
                    """
                    cursor.execute(createTable)
                    admission_no=input("ENTER ADMISSION NO OF THE STUDENT :")
                    month=int(input("\n ENTER MONTH IN NUMERIC FORM (1-12) : "))
                    tutionfee=int(input("\n ENTER TUTION FEES : "))
                    computerfee=int(input("\n ENTER COMPUTER FEES : "))
                    musicfee=int(input("\n ENTER MUSIC FEES : "))
                    total = tutionfee  + computerfee + musicfee
                    sql='''INSERT INTO
                    FEES(SADMISSION_NO,MONTH,TUTION_FEES,COMPUTER_FEES,MUSIC_
                    FEES,TOTAL) VALUES('{}',{},{},{},{},{})
                    '''.format(admission_no,month,tutionfee,computerfee,musicfee,total)
                    
                    cursor.execute(sql)
                    cursor.execute("COMMIT")
                    cursor.close()
                    print("\nFees of Student Accepted Successfully !")

          else:
                    print("\nERROR ESTABLISHING MYSQL CONNECTION !")

#MODULE TO GENERATE FEES RECEIPT OF ALL STUDENTS
def feeReceiptAllStudent():
          cursor=myConnection.cursor()
          if myConnection:
                    cursor.execute("SELECT * FROM FEES")
                    data=cursor.fetchall()
                    print(data)
                    cursor.close()
          else:
                    print("\nSomthing Went Wrong ,Please Try Again !")

#MODULE TO GENERATE FEES RECEIPT OF ONE STUDENT
def feeReceiptOneStudent():
          cursor=myConnection.cursor()
          if myConnection:
                    admission_no=input("ENTER ADMISSION NO OF THE STUDENT :")
                    cursor=myConnection.cursor()
                    sql="SELECT * FROM FEES WHERE SADMISSION_NO= '{}'".format(admission_no)
                    cursor.execute(sql)
                    data=cursor.fetchall()
          if data:
                    print(data)
          else:
                    print("Record Not Found , Please Try Again !")
                    cursor.close()

          
#MODULE TO ISSUE TRANSFER CERTIFICATE
def transferStudent():
          cursor=myConnection.cursor()

          if myConnection:
                    admission_no=input("ENTER ADMISSION NO OF THE STUDENT :")
                    cursor=myConnection.cursor()
                    sql="SELECT * FROM STUDENT WHERE SADMISSION_NO= '{}'".format(admission_no)
                    cursor.execute(sql)
                    data=cursor.fetchall()
                    print("This Student have been given the Transfer Certificate!!!")
          if data:
                    sql="DELETE FROM STUDENT WHERE SADMISSION_NO='{}'".format(admission_no)
                    cursor.execute(sql)
                    cursor.execute("COMMIT")
                    
          else:
                    print("Record Not Found , Please Try Again !")
                    cursor.close()

         
                   

#MODULE TO PROVIDE HELP FOR USER
def helpMe():
          print("Please, Visit The Offcial Website Of Vidyalaya To Download The Mannual !!!")


# MAIN SCREEN OF THE SYSTEM
print("**************************************************************************")
print("\n+-----------------  JNV MANAGEMENT SOFTWARE  ----------------------------+")

print("\n+------------------------ SESSION 2022-23 -------------------------------+")
print("\n+-------------------------- WELCOME!!------------------------------------+")
print("\n+------------ JAWAHAR NAVODAYA VIDYALAYA , BASDEI , SURAJPUR(C.G.)-------+")
print("\n+------------------ TOPIC -  SCHOOLMANAGEMENT ---------------------------+")
print("\n+---------------    DESIGNED AND MAINTAINED BY:   -----------------------+")
print("\n+------------------------------------------------------------------------+")
print("\n+----------- SUDHANSHU TIWARI 12th A ROLL NO-1243 -----------------------+")
print("\n+----------   SACHIN GUPTA 12th A ROLL NO- 1236--------------------------+")
print("\n+-----------  ANISH TOPPO 12th A ROLL NO- 1207 --------------------------+")
print("\n+------------------------------------------------------------------------+")
print("\n**************************************************************************")
#STARTING POINT OF THE SYSTEM
myConnection = MYSQLconnectionCheck ()
if myConnection:
          MYSQLconnection ()
while True:

    print("|-------------------------------------------------------------|")
    print("| Enter 1 - New Admission. |")
    print("| Enter 2 - Display Student's Data. |")
    print("| Enter 3 - Update Students's Data . |")
    print("| Enter 4 - Issue Transfer Certififcate . |")
    print("| Enter 5 - Add Student's Marks Detail. |")
    print("| Enter 6 - Generate All Student's Report Card. |")
    print("| Enter 7 - Generate Student Wise Report Card. |")
    print("| Enter 8 - Pay Student's Fee. |")
    print("| Enter 9 - Generate Student Wise Fees Receipt. |")
    print("| Enter 10 - Generate Student's Fee Receipt. |")
    print("| Enter 11- Exit. |")
    print("|-------------------------------------------------------------|")
    print("| Enter 0(ZERO) - Help. |")
    print("|-------------------------------------------------------------|")

    choice=int(input("PLEASE ENTER YOUR CHOICE : "))
    if choice==1:
        newStudent()
    elif choice==2:
        displayStudent()
    elif choice==3:

        updateStudent()
    elif choice==4:
        
        transferStudent()
    elif choice==5:
        marksStudent()
    elif choice==6:
        reportCardAllStudent()
    elif choice==7:
        reportCardOneStudent()
    elif choice==8:
        feeStudent()
    elif choice==9:
        feeReceiptAllStudent()
    elif choice==10:
        feeReceiptOneStudent()
    elif choice==11:
        myConnection.close()

    elif choice==0:
        helpMe()
    
          

 
    else:
          print("Sorry ,May Be You Are Giving Me Wrong Input, Please Try Again !!! ")
    

         

# END OF THE PROJECT



