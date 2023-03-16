
# Querying a Large Relational Database

In this project, first, we will work on downloading a database and restoring it on the server. 
You will then query the database to get customer details like name, phone number, email ID, 
sales made in a particular month, increase in month-on-month sales, and even the total sales 
made to a particular customer

# Highlights: 

 Table basics and data types 
 Various SQL operators
 Various SQL functions



# Tasks to be performed

 ## 1 - Download the Adventure Works database from the following location and restore it in your server 

- https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworkss
 - File Name : AdventureWorks2012.bak

AdventureWorks is a sample database shipped with SQL Server, and it can be downloaded 
from the GitHub site. AdventureWorks has replaced Northwind and Pubs sample databases 
that were available in SQL Server 2005. Microsoft keeps updating the sample database as it 
releases new versions.

## 2 - Restore Backup

Follow the below steps to restore a backup of your database using SQL Server Management 
Studio: 

 Open SQL Server Management Studio and connect to the target SQL Server instance 
 Right-click on the Databases node and select Restore Database 
 Select Device and click on the ellipsis (...) 
 In the dialog, select Backup devices, click on Add, navigate to the database backup in 
the file system of the server, select the backup, and click on OK. 
 If needed, change the target location for the data and log files in the Files pane 

 It is a best practice to place the data and log files on different drives. 

 Now, click on OK 
This will initiate the database restore. After it completes, you will have the 
AdventureWorks database installed on your SQL Server instance

## 3 - Perform the following with help of the above database 

 Get all the details from the person table including email ID, phone number, and phone 
number type 
 Get the details of the sales header order made in May 2011 
 Get the details of the sales details order made in the month of May 2011 
 Get the total sales made in May 2011 
 Get the total sales made in the year 2011 by month order by increasing sales 
 Get the total sales made to the customer with FirstName='Gustavo' and LastName 
='Achong'



..............................



select datename(weekday,eomonth(getdate()))

--Get all the details from the person table including email ID, phone number, and phone

--number type
--person.person
--person.personphone
--person.emailaddress
--person.personphonenumbertype

select person.person.*,person.emailaddress.emailaddress,person.personphone.phonenumber,
person.PhoneNumberType.PhoneNumberTypeID from Person.person
join person.EmailAddress on
person.person.BusinessEntityID=person.emailaddress.BusinessEntityID
join person.personphone on
person.person.BusinessEntityID=person.personphone.BusinessEntityID
join person.PhoneNumberType on
person.personphone.PhoneNumberTypeID=person.phonenumbertype.PhoneNumberTypeID



-- Get the details of the sales header order made in May 2011

--select datepart(month,getdate())

select * from sales.SalesOrderHeader
where datepart(year,orderdate)=2011 and datepart(month,orderdate)= 5

select * from sales.SalesOrderHeader
where year(orderdate)=2011 and month(orderdate)= 5

select * from sales.SalesOrderHeader
where OrderDate between '2011-05-01' and '2011-05-31'


-- Get the details of the sales details order made in the month of May 2011

select * from sales.SalesOrderDetail join
sales.SalesOrderHeader on sales.SalesOrderDetail.salesorderid=sales.SalesOrderHeader.SalesOrderID
where month(orderdate)=5 and year(orderdate) = 2011


-- Get the total sales made in May 2011

select sum(totaldue) as total_sales from sales.salesorderheader
where month(orderdate)=5 and year(orderdate)=2011

-- Get the total sales made in the year 2011 by month order by increasing sales 

select month(orderdate) as month,sum(totaldue) as total_sales
from sales.SalesOrderHeader where year(orderdate)=2011
group by month(orderdate)
order by total_sales


select (datename(month,orderdate)) as month,sum(totaldue) as total_sales
from sales.SalesOrderHeader where year(orderdate)=2011
group by (datename(month,orderdate))
order by total_sales

--Get the total sales made to the customer with FirstName='Gustavo' and LastName
--='Achong'
