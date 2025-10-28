# COSC 304 - Introduction to Database Systems<br>Lab 6: Using Java/Python with MySQL and Microsoft SQL Server

This lab shows how Java and Python programs can connect to MySQL and Microsoft SQL Server.

**Make sure your computer is setup to run Docker by following these [setup instructions](https://github.com/rlawrenc/cosc_304/blob/main/labs/setup).**

### Setup MySQL and SQL Server Docker Container

 - All of the files that you need are in the directory. 
 - Open a command shell either directly on your machine or using VSCode. Make sure your current directory is the directory containing `docker-compose.yml`.
 - Run the command `docker-compose up -d`
 - If everything is successful, the MySQL database will start on port 3306. If there is a port conflict, change the port to 3307 in the `docker-compose.yml` file by modifying the line below `ports:` to `'3307:3306'`. The connection URL is then `localhost:3307/testuser`.
 - Your database is `mydb` or `testuser`. There are other databases also created such as `workson`. Do NOT use the sample `university` database from lab 2.
 - Microsoft SQL Server will be running on port 1433. Note SQL Server is not supported on the Apple M1 chip. However, there is an alternate version that is. In the docker-compose.yml file, change: `image: mcr.microsoft.com/mssql/server:2019-latest` to this: `image: mcr.microsoft.com/azure-sql-edge`.

## Part #1: MySQL

There is [a sample Java program](code/TestJDBCMySQL.java) or [sample Python program](code/PythonQueryExample.py) that connects to a WorksOn database hosted by MySQL. 
The user id and password information is in the `docker-compose.yml` file.

### Java Setup

1. Make sure the [MySQL JDBC driver](code/mysql-connector-j-9.5.0.jar) is in your classpath. This can be done in VSCode in the `Java Project` tab.

2. Use [the sample file](code/TestJDBCMySQL.java).

3. These are the modifications you must make to get the program working:

```
Change Line 5 to:	String url = "jdbc:mysql://localhost/workson";
Change Line 6 to:	String uid = "put your user id here";
Change Line 7 to:	String pw = "put your password here";
```

### Python Setup

1. Install MySQL connector library in a terminal using the command: `pip install mysql-connector-python`  You may need to restart VSCode for it to see the library after it is installed.

2. Use [the sample Python file](code/PythonQueryExample.py) and setup in your Python environment.

3. These are the modifications you must make to get the program working:

```
Change Line 3 to add the password for testuser account:

 cnx = mysql.connector.connect(user='testuser', <b>password='todo'</b>, host='localhost', database='workson', ssl_disabled='True') 
```

The result of the program is this:

```
Employee Name,Salary
M. Smith,50000.0
A. Lee,40000.0
J. Miller,20000.0
B. Casey,50000.0
L. Chu,30000.0
R. Davis,40000.0
J. Jones,50000.0
```

## Question #1

Create a new program called `MyJDBC.java` or `MySQLQuestion.py`.

Note: With Python to have multiple cursors on a connection, set the cursor to buffering like this: `cursor = cnx.cursor(buffered=True)`.

Your program should be able to do this:

- List each employee that is a supervisor.
- Sort the list of supervisors by name.
- Under each supervisor, list the employees that he/she <b>directly</b> supervises sorted by decreasing salary.

The result of your program should look like this:

```
Supervisor: B. Casey
   M. Smith, 50000.00

Supervisor: J. Jones
   B. Casey, 50000.00
   R. Davis, 40000.00

Supervisor: L. Chu
   J. Miller, 20000.00

Supervisor: M. Smith
   J. Doe, 30000.00
   
Supervisor: R. Davis
   A. Lee, 40000.00
   L. Chu, 30000.00
```
**Answer:**  [Java answer file](code/MyJDBC.java), [Python answer file](code/MySQLQuestion.py)

## Part #2: Microsoft SQL Server

### Java Setup

1. Make sure the [Microsoft SQL Server JDBC driver](code/mssql-jdbc-13.2.1.jre11.jar) is in your classpath. This can be done in VSCode in the `Java Project` tab.

2. If the workson database was not automatically created, you must connect to SQL Server using SQuirreL or command line to create the workson database. 

3. Use [the sample file](code/TestJdbcSqlServer.java).  

4. These are the modifications you must make to get the program working:

```
Change Line 5 to:	String url = "jdbc:sqlserver://localhost;DatabaseName=workson;TrustServerCertificate=True";
Change Line 6 to:	String uid = "sa";
Change Line 7 to:	String pw = "put password here";
```

### Python Setup

1. Install SQL Server pyodbc connector by following [these directions](https://docs.microsoft.com/en-us/sql/connect/python/python-driver-for-sql-server).

2. Use [the sample Python file](code/PythonSQLServer.py) and setup in your Python environment.

3. These are the modifications you must make to get the program working:

```
Change Line 3 and 4 to:	cnx = pyodbc.connect("""DRIVER={ODBC Driver 17 for SQL Server};SERVER=localhost;
							DATABASE=workson;UID=sa;PWD=yourPassword""")
```

### Expected Output

```
Employee Name,Salary
M. Smith,50000.0
A. Lee,40000.0
J. Miller,20000.0
B. Casey,50000.0
L. Chu,30000.0
R. Davis,40000.0
J. Jones,50000.0
```

## Question #2

Modify either the Java or Python sample program to create a program that lists the top project for each department based on the total hours worked by employees on projects.

### Expected Output

```
Id: D1     Name: Management
Proj#	Name	Total Hours
P1   	Instruments	36

Id: D2     Name: Consulting
Proj#	Name	Total Hours
P4   	Maintenance	96

Id: D3     Name: Accounting
Proj#	Name	Total Hours
P3   	Budget	46
```

**Answer:**  [Java answer file](code/SqlServerQuestion.java), [Python answer file](code/SqlServerQuestion.py)

## [Lab 6 Assignment (Java)](assignJava/)

## [Lab 6 Assignment (Python)](assignPython/)
