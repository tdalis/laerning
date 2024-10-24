# Write your first C# code - Day 2

## Perform basic operations on numbers in C#

In this module we will cover:
- Performing simple mathematical operations including addition, subtraction, multiplication, and division
- Performing multi-step operations that must be completed in a certain order
    - That's a common scenario in programming
- Determining the remainder after performing division
- Incrementing or decrementing a value and so on

Let's say you want to convert Fahrenheit to Celsius and then display that information in a formatted message. We will learn how to do that in this module.

The key learning objectives by the end of this module are:
- Perform mathematical operations on numeric values
- Observe implicit type conversion between strings and numeric values
- Temporarily convert one data type into another

### Additions

Similar to every other language I've programmed in the past such as Python and R, C# is using the plus symbol ```+``` to do additions. Although the same symbol can be used for concatenation of strings, the C# compiler knows that you want to do addition when the ```+``` operator is surrounded by two numeric values. For example:

```C#
int firstNumber = 12;
int secondNumber = 7;
Console.WriteLine(firstNumber + secondNumber);
```

The output will be ```19```.

The C# compiler is smart enough to know that when you use both string and int with the ```+``` symbol, it means that you want to concatenate the two operands. For example:

```C#
string firstName = "Bob";
int widgetsSold = 7;
Console.WriteLine(firstName + " sold " + widgetsSold + " widgets.");
```

Will return: ```Bob sold 7 widgets.```.

What if we add a number after the number (widgetsSold + 7)?

```C#
string firstName = "Bob";
int widgetsSold = 7;
Console.WriteLine(firstName + " sold " + sidgetsSold + 7 + " widgets.");
```

This will return ```Bob sold 77 widgets.```. This is because the C# compiler treats everything as a string and concatenates it all together.

If our intention is to add the two integers so that it returns ```14``` instead of ```77``` then we can use parentheses:

```C#
string firstName = "Bob";
int widgetsSold = 7;
Console.WriteLine(firstName + " sold " + (sidgetsSold + 7) + " widgets.");
```

The above will return ```Bob sold 14 widgets.``` This is because the C# compiler treats the parentheses the same way we would in mathematics, telling the compiler to first resolve the inner-most parentheses before everything else. In this case, the inner-most parentheses contains two integers, therefore, it will be compiled as an addition and not as a concatenation.

It is not good practice to do concatenation and addition in a single line of code, but the module is trying to show to us what's possible.

### Next - let's write code that performs addition, subtraction, multiplication, and division with integers

A simple example:

```C#
int sum = 7 + 5;
int difference = 7 - 5;
int product = 7 * 5;
int quotient = 7 / 5;

Console.WriteLine("Sum: " + sum);
Console.WriteLine("Difference: " + difference);
Console.WriteLine("Product: " + product);
Console.WriteLine("Quotient: " + quotient);
```

The above will return:

```
Sum: 12
Difference: 2
Product: 35
Quotient: 1
```

Which showcases that the same operators we are used to be using in mathematics are used in C# (same as with other programming languages we might be familiar with).

The quotient returns 1 because the decimal are truncated from the quotient since we are defining it as an int.

To fix this we would define it as decimal:

```C#
decimal decimalQuotient = 7.0m / 5;

Console.WriteLine("Decimal quotient: " + decimalQuotient);
```

Which will return ```Decimal quotient: 1.4```.

At least one number being divided needs to be of decimal type (as with 7.0m). Both can be of decimal ```7.0m``` and ```5.0m``` for example.

### Cast results of integer division

What if we want to divide two variable of type ```int``` but don't want the result truncated? We can perform a data type cast from ```int``` to ```decimal```. To do that, we use the name of the data type (in this case ```decimal```), surrounded by parentheses in front of the value to cast it. Let's see the example below:

```C#
int first = 7;
int second = 5;
decimal quotient = (decimal)first / (decimal)second;

Console.WriteLine(quotient);
```

The above will return ```1.4```

### Now let's write code to determine the remainder after integer division

If we want to know whether one number is divisible by another, we need to return the remainder of the integer division. The module points out that this can be useful during long processing operations when looping through hundreds of thousands of data records and you want to provide feedback to the end user after every 100 data records have been processed. To do that we can use the modulus operator ```%```.

Let's see it in practice:

```C#
Console.WriteLine($"Modulus of 200 / 5 : {200 % 5}");
Console.WriteLine($"Modulus of 7 /5 : {7 % 5}");
```

The output will be:

```
Modulus of 200 / 5 : 0
Modulus of 7 /5 : 2
```

When the modulus is 0, that means that the dividend is divisible by the divisor.

### Order of operations

Earlier on we saw that we can use the ```()``` symbols as the order of operations operators but this isn't the only way the order of operations is determined.

Same as in maths, we can use PEMDAS to help us remember the order of operations:

1. <strong>P</strong>arentheses (whatever is inside the parentheses is performed first)
2. <strong>E</strong>xponents
3. <strong>M</strong>ultiplication and <strong>D</strong>ivision (from left to right)
4. <strong>A</strong>ddition and <strong>S</strong>ubtraction (from left to right)

C# follows the same order as PEDMAS except for exponents.

We can use the below example to see the PEDMAS in practice:

```C#
int value1 = 3 + 4 * 5;
int value2 = (3 + 4) * 5;

Console.WriteLine(value1);
Console.WriteLine(value2);
```

The output:
```
23
35
```

### Increment and decrement values

We can increment and decrement values using special operators that are combinations of symbols.

We will need to increment and decrement values often. Especially when we're writing looping logic or code that interacts with a data structure.

The ```+=``` operator adds and assigns the value on the right of the operator to the value on the left of the operator. To understand this, let's consider lines 2 and 3 below:

```C#
int value = 0; // value is now 0.
value = value + 5; // value is now 5.
value += 5; // value is now 10 (5+5).
```

The ```++``` operator increments the value of the variable by 1. Let's consider lines 2 and 3 below:

```C#
int value = 0; // value is now 0.
value = value + 1; // value is now 1.
value++; // value is now 2.
```

These same techniques can be used for subtraction, multiplication, and more. Operators like ```+=```, ```-=```, ```*=```, and ```--``` are known as compound assignment operators because they compound some operation in addition to assigning the result to the variable.

Let's practice incrementing and decrementing our values:

```C#
int value = 1;

value = value + 1;
Console.WriteLine("First increment: " + value);

value += 1;
Console.WriteLine("Second increment: " + value);

value++;
Console.WriteLine("Third increment: " + value);

value = value - 1;
Console.WriteLine("First decrement: " + value);

value -= 1;
Console.WriteLine("Second decrement: " + value);

value--;
Console.WriteLine("Third decrement: " + value);
```

Output:
```
First increment: 2
Second increment: 3
Third increment: 4
First decrement: 3
Second decrement: 2
Third decrement: 1
```

### Position the increment and decrement operators

Both the increment and decrement operators have an interesting quality - depending on their position, they perform their operation before or after they retrieve their value. If you use the operator before the value ```++value```, then the increment will happen before the value is retrieved. Likewise, ```value++``` will increment the value after the value has been retrieved.

Let's try to understand this with an example

```C#
int value = 1;
value++;

Console.WriteLine("First: " + value);
Console.WriteLine($"Second: {value++}");
Console.WriteLine("Third: " + value);
Console.WriteLine("Fourth: " + (++value));
```

Output:
```
First: 2
Second: 2
Third: 3
Fourth: 4
```

The reason the ```Second``` value returns 2 is because it prints the value before the increment operation. The ```Third``` value confirms that the increment operation was executed after the ```Second``` value was returned. The ```Fourth``` value first increments and then it returns the final value.

### Coding challenge, calculate celsius given the current temperature in Fahrenheit

I'll go along with this challenge, although I am passionate about the abolition of Fahrenheit from the world! Why do we need Fahrenheit? 

The instructions say that to convert temperatures in degrees Fahrenheit to Celsius we need to first subtract 32 and then multiply by five ninths (5 / 9).

The solution I devised is:

```C#
int fahrenheit = 94;
decimal celsius = (fahrenheit - 32) * (5.0m / 9.0m);

Console.WriteLine($"The temerature is {celsius} Celsius.");
```

Output:
```
The temerature is 34.444444444444444444444444447 Celsius.
```

The output is the expected output.

## Calculate and print student grades

Learning objectives:

- Work with variables to store and retrieve data
- Perform basic math operations
- Format strings to present results

The exercise is simple but it puts what we've learned so far in good practice:

```C#
// initialise variables - graded assignments
int currentAssignments = 5;

int sophia1 = 93;
int sophia2 = 87;
int sophia3 = 98;
int sophia4 = 95;
int sophia5 = 100;

int nicolas1 = 80;
int nicolas2 = 83;
int nicolas3 = 82;
int nicolas4 = 88;
int nicolas5 = 85;

int zahirah1 = 84;
int zahirah2 = 96;
int zahirah3 = 73;
int zahirah4 = 85;
int zahirah5 = 79;

int jeong1 = 90;
int jeong2 = 92;
int jeong3 = 98;
int jeong4 = 100;
int jeong5 = 97;

int sophiaSum = sophia1 + sophia2 + sophia3 + sophia4 + sophia5;
int nicolasSum = nicolas1 + nicolas2 + nicolas3 + nicolas4 + nicolas5;
int zahirahSum = zahirah1 + zahirah2 + zahirah3 + zahirah4 + zahirah5;
int jeongSum = jeong1 + jeong2 + jeong3 + jeong4 + jeong5;

decimal sophiaScore = (decimal) sophiaSum / currentAssignments;
decimal nicolasScore = (decimal) nicolasSum / currentAssignments;
decimal zahirahScore = (decimal) zahirahSum / currentAssignments;
decimal jeongScore = (decimal) jeongSum / currentAssignments;

Console.WriteLine("Student\t\tGrade\n");

Console.WriteLine("Sophia:\t\t" + sophiaScore + "\tA");
Console.WriteLine("Nicolas:\t" + nicolasScore + "\tB");
Console.WriteLine("Zahirah\t\t" + zahirahScore + "\tB");
Console.WriteLine("Jeong:\t\t" + jeongScore + "\tA");
```

Output:
```
Student		Grade

Sophia:		94.6	A
Nicolas:	83.6	B
Zahirah		83.4	B
Jeong:		95.4	A
```

