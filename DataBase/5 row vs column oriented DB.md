# Row vs Column Oriented DB

Row oriented DB

- tables are stored as rows in disk
- A single block io read to table, fetches multiple rows with all their columns
- more IO is required to find row but once you find it we get all the columns as it is stored in ram

if the where is a seq column db can find the page/block easily

select * is worse in column oriented

a row can be in more than 1 page/block?

TóAST in pg?

if a query like 
select sum(salary) from emp

here we get all columns in memory

Column Oriented database
- Tables are stores as columns
- a single block io read retches multiple columns with all matching rows
- less IO for many values
- but gettig multiple columns is slow
- OLAP

block 1
1:1001 2:2002 
value : row id
for delete we need to delete value in multiple pages

we find the row number using where and then DB will find the value needed 

Select * from will need so many more IOs of pages
