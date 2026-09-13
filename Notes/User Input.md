---
categories:
  - "[[Guides]]"
topics:
  - "[[Python]]"
created:
---

```python
name = input('Who are you? ') # Prompt for user input
print('Welcome', name)
```

We can instruct Python to pause and read data from the user using `input()` function. The `input()` function returns a string.

Running the above script will return:

``` bash
Who are you? Input_Your_Name
Welcome Input_Your_Name
```

Input will ***always*** return a string.
## Converting User Input

This script will change US elevator naming conventions to European standard. Also need to change the user input from a str to a int.

```python
inp = input('Europe floor?') # Asking for user input
usf = int(inp) + 1 # Converting inp from str to int & adding +1
print('US floor', usf) # Output the results
```
### Example:

This script will provide gross pay for hours worked.
```python
hrs = input("Enter Hours:")
rate = input("Enter Pay Rate:")
pay = float(hrs) * float(rate) # Decimals need to be converted to a float()

print("Pay:", pay)
```