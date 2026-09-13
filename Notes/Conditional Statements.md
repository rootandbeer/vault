---
categories:
  - "[[Guides]]"
topics:
  - "[[Python]]"
created:
---

## If, Elsif, Else
Indentation creates "blocks" of code. In the following example, if `x` greater than 1 then it will check if `x` less than 100, then print `All Done`. If x is less than 1 it will just print `All Done`
```python
x = input('Enter a Number:')
x = int(x) # convert from str to int
if x > 1 :
	print('More than one')
	if x < 100 : # Since still indented, will only run if X > 1
		print('Less than 100')
print('All Done')
```

Else Statements are ran when the `if` statement is false:
```python
x = input('Enter a Number:')
x = int(x)
if x < 2 :
	print('Bigger')
else :
	print('Smaller')
print('All Done')
```

The code will only check the `elif` statement if the previous `if` statement was `false`. If the `if` statement was `true`, it will skip the `elif` statement. If neither the `if` or `elif` statement is `true`, then it will run the `else` statement.
```python
x = input('Enter a Number:')
x = int(x)
if x < 2 :
	print('Small')
elif x < 10 :
	print('Medium')
else print('Large')
print('All Done')
```

if `x = 1` then `Small` will be printed.
if `x = 5` then `Medium` will be printed.
if `x = 11` then `Large` will be printed.
All inputs will have `All Done` printed when the script has finished running.

## Try, Except
a `try` statement will attempt to do something, if it gives a `traceback` error then it will immediately proceed to the `except` statement.

```python
astr = "hello rob"
try:
	istr = int(astr)
	print('Howdy') #this will be skipped since the previous line gave an error
except:
	istr = -1
	print (istr) #this will print since the try statement gave an error
```
*This will print `-1` since the `try` statement gives an error. The `Howdy` is skipped*

You can use multiple lines inside of a `try` statement, the line that throws an error will be the last line processed in the `try` statement. Once an error is thrown, the script IMMEDIATELY goes to the `except` statement.