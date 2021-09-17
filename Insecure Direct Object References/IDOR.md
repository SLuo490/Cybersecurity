# [Insecure Direct Object References](https://guides.codepath.org/websecurity/Insecure-Direct-Object-Reference)

# What is IDOR?

Insecure Direct Object Reference is when code accesses a restricted resource based on user input, but fails to verify user's authorization to access that resource
- "there exists a "direct reference" to an "object" which is "insecure"

# Example:
Imagine a user clicks on a link to view an invoice
```
http://foo.com/receipt?invoice=TPX-10457
```
The code that accesses the invoice might look something like: 
```php
<?php
  $sql = "SELECT * FROM orders WHERE invoice_id='{$invoice}'";
  $result = myqli_query($connection, $sql);
?>
```
This is "direct object reference" since a user has direct access to the invoice. 
- By entering `TPX-10456` the user can access other invoices. These invoices might review someone else's name, address, phone, email, credit card, and more. 

# Example of Invoices:
- Database Data
- Files
- Directories
- Scripts
- Functions

# Summary
If a user can trigger what should be a privileged result without having to validate their privileges, then it is probably a case of Insecure Direct Object Reference. 

# Preventions

The primary prevention method is to validate a user's authorization before allowing acces to a privileged resource
- Require the user to log in
- Confimr that logged in user have sufficient access privileges

The other way is to change direct object reference to a indirect object reference

Example:
- logged in user sees list of 4 previous orders
- Previous orders are numbered 1 - 4
- User chooses order #2
- The request sends ID = '2', NOT the order ID
- ID is indirect, only meaninful in user's scope

The only valid choices for the user are 1-4, all return resources for which the user is authorized. 
If another user attempts to access the same URL (with ID='2') it would only return one of their authorized resources.





