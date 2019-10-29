# Strings

## Padding: center(), rjust(), ljust()
The center() method returns a string which is padded with the specified character.

The rjust()\ljust() methods return a string which is padded on either side.
```python
>>> 'A'.center(1, '*')
'A'
>>> 'A'.center(2, '*')
'A*'
>>> 'A'.center(3, '*')
'*A*'
>>> 'A'.rjust(1, '*')
'A'
>>> 'A'.rjust(2, '*')
'*A'
>>> 'A'.rjust(3, '*')
'**A'
>>> 'A'.ljust(1, '*')
'A'
>>> 'A'.ljust(2, '*')
'A*'
>>> 'A'.ljust(3, '*')
'A**'
```

## str.format()

**Different common uses of the str.format() function**

*format(value[, format_spec])*

First example:

```python
>>> "{}".format('value')
'value'
```

Multiple values:
```python
>>> "{} {}".format('value1','value2')
'value1 value2'
```

Reverse order:
```python
>>> "{1} {0}".format('value1','value2')
'value2 value1'
```

Key Word Arguments

```python
>>> print("{kwarg} is {0} used for {1}"
.format("being", "string formatting", kwarg ="Some Key Word Argument"))

... Some Key Word Argument is being used for string formatting
```

We can also use types to further format values:

**Syntax:** {field_name:conversion}.format(value) 

*Note: field_name can be the index (0) or name of key word argument*

```python
conversion values:
s – strings
d – decimal integers (base-10)
f – floating point display
c – character
b – binary
o – octal
x – hexadecimal with lowercase letters after 9
X – hexadecimal with uppercase letters after 9
e – exponent notation
```

Use like this:

**s - string**

```python
>>> '{kwarg:s}'.format(kwarg='5')
'5'
>>> '{kwarg:s}'.format(kwarg=5)
Traceback (most recent call last):
  File "", line 1, in 
ValueError: Unknown format code 's' for object of type 'int'
```

Notice how we got an error for trying to convert an int to a string

**d - decimal integers**

```python
>>> # This works 
print("Convert {0} to decimal integer: {0:d}.".format(100))
# Notice the Error
print("Convert {0} to decimal integer: {0:d}.".format(100.0))

... Convert 100 to decimal integer: 100.
>>> ... Traceback (most recent call last):
  File "", line 2, in 
ValueError: Unknown format code 'd' for object of type 'float'
```

**f - floats**

```python
>>> # Default decimal precision to 0.000001
print("Convert {0} to float: {0:f}.".format(100.123456789))
... Convert 100.123456789 to float: 100.123457.
>>> # Change decimal precision to 0.01
print("Convert {0} to float: {0:.2f}.".format(100.123456789))
... Convert 100.123456789 to float: 100.12.
>>> # Change decimal precision to 0.1
print("Convert {0} to float: {0:.1f}.".format(100.123456789))
... Convert 100.123456789 to float: 100.1.
>>> # Change decimal precision to 0.000000001
print("Convert {0} to float: {0:.9f}.".format(100.123456789))
... Convert 100.123456789 to float: 100.123456789.
```  

**c - single character** *(accepts integer or single character string). [Use this link for a unicode character lookup table](https://unicode-table.com/en/)*

Brief sample...

| NUL | SOH | STX | ETX | EOT | ENQ | ACK | BEL | BS  | HT | LF  | VT  | FF | CR | SO | SI |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|----|-----|-----|----|----|----|----|
| DLE | DC1 | DC2 | DC3 | DC4 | NAK | SYN | ETB | CAN | EM | SUB | ESC | FS | GS | RS | US |
|     | !   | "   | #   | $   | %   | &   | '   | (   | )  | *   | +   | ,  | -  | .  | /  |
| 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9  | :   | ;   | <  | =  | >  | ?  |
| @   | A   | B   | C   | D   | E   | F   | G   | H   | I  | J   | K   | L  | M  | N  | O  |
| P   | Q   | R   | S   | T   | U   | V   | W   | X   | Y  | Z   | [   | \  | ]  | ^  | _  |
|     |     |     |     |     |     |     |     |     |    |     |     |    |    |    |    |

...

```python
#000
>>> ' '.join(['{0:c}'.format(_) for _ in range(16)])
'\x00 \x01 \x02 \x03 \x04 \x05 \x06 \x07 \x08 \t \n \x0b \x0c \r \x0e \x0f'
#001
>>> ' '.join(['{0:c}'.format(_) for _ in range(16,32)])
'\x10 \x11 \x12 \x13 \x14 \x15 \x16 \x17 \x18 \x19 \x1a \x1b \x1c \x1d \x1e \x1f'
#002
>>> ' '.join(['{0:c}'.format(_) for _ in range(32,48)])
'  ! " # $ % & \' ( ) * + , - . /'
#003
>>> ' '.join(['{0:c}'.format(_) for _ in range(48,64)])
'0 1 2 3 4 5 6 7 8 9 : ; < = > ?'
#004
>>> ' '.join(['{0:c}'.format(_) for _ in range(64,80)])
'@ A B C D E F G H I J K L M N O'
#005
>>> ' '.join(['{0:c}'.format(_) for _ in range(80,96)])
'P Q R S T U V W X Y Z [ \\ ] ^ _'
```

**b - binary**

```python
>>> print("{0:b}".format(0))
0
>>> print("{0:b}".format(1))
1
>>> print("{0:b}".format(2))
10
>>> print("{0:b}".format(3))
11
>>> print("{0:b}".format(100))
1100100
```

**o - octal**

```python
>>> print("{0:o}".format(0))
0
>>> print("{0:o}".format(8))
10
>>> print("{0:o}".format(8*2))
20
>>> print("{0:o}".format(8*3))
30
>>> print("{0:o}".format(8*4))
40
>>> print("{0:o}".format(8 ** 1))
10
>>> print("{0:o}".format(8 ** 2))
100
>>> print("{0:o}".format(8 ** 3))
1000
```

**x\H - hex\HEX**

```python
print("{0:x}".format(0*16)) # 0
print("{0:x}".format(1*16)) # 10
print("{0:x}".format(2*16)) # 20
print("{0:x}".format(16 ** 1)) # 10
print("{0:x}".format(16 ** 2)) # 100
print("{0:x}".format(16 ** 3)) # 1000
print("{0:x}".format(10)) # a
print("{0:X}".format(10)) # A
print("{0:x}".format(15)) # f
print("{0:X}".format(15)) # F
```

**e - exponent**

```python
print("{0:e}".format(10))   # 1.000000e+01 
print("{0:e}".format(100))  # 1.000000e+02
print("{0:e}".format(1000)) # 1.000000e+03 
```

## str.find()

**Use a substring and start-end range to identify where a substring resides in a larger string**

*str.find(sub[, start[, end]])*

```python
s = 'ABCBCBCBCBC'

# str.find() will return the index found or -1.

# with only a start value the rest of the string is searched
print(s.find('BC', 0)) # >>> 1
print(s.find('BC', 1)) # >>> 1
print(s.find('BC', 2)) # >>> 3
print(s.find('BC', 3)) # >>> 3
print(s.find('BC', 4)) # >>> 5
print(s.find('BC', 5)) # >>> 5
print(s.find('BC', 6)) # >>> 7
print(s.find('BC', 7)) # >>> 7
print(s.find('BC', 8)) # >>> 9
print(s.find('BC', 9)) # >>> 9
print(s.find('BC', 10)) # >>> -1

# with start and end, the substring must be within the range
print(s.find('BC', 0,1)) # >>> -1
print(s.find('BC', 0,2)) # >>> -1
print(s.find('BC', 0,3)) # >>> 1
```