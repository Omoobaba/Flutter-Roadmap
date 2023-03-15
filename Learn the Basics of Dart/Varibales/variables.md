# Variables

In Flutter, variables are used to store values. There are two types of variables in Flutter:

- local variables: These are declared within a function and are only accessible within that function
- Instance variables: They are declared within a class and are accessible throughout the entire class.

Variables in Flutter can store values of different data types, such as numbers, strings, booleans, and more.

# Variable declaration using the ‘var’ keyword

There are different ways to initialize a variable in Dart. The straight forward and recommended way is by using the keyword **‘var‘**.

example:

```dart
var personName = 'John Doe';
var personAge = 25;
```

In the above example, we created a variable with name ‘personName’ and stored the value ‘John Doe’ in that variable. Similarly we created a variable ‘personAge’ and stored 25 value in it.

We use the keyword ‘var’ when we create a variable and we omit the keyword when we use it.

example:

```dart
print(personName); // we get John Doe
print(personAge); // we get 25
```

Based on the value assigned into a variable, Dart will automatically infer data type to that variable. In the above example, Dart will automatically give the type of ‘String’ to personName and ‘int’ to personAge.

> After initializing a variable with certain type of data using var keyword, it is not possible to assign a different type of data in the same variable later.
>Suppose if we try to assign personName = 123  we get the following error message
> “A value of type ‘int’ can’t be assigned to a variable of type ‘String'”

# Variable Declaration using Data Type in Dart

Instead of using the ‘var’ keyword, we can use the data type to create a variable. The variables in the previous example can also be initialized like this

```dart
String personName = 'John Doe';
int personAge = 25;
```

# Variable Declaration using ‘dynamic’ keyword

There is another way to create a variable in Dart by using the keyword ‘dynamic‘. As the name suggests, the variable which is created by using the keyword dynamic, can contain any type of data.

example:

```dart
dynamic myVar = "Hello World";
myVar = 100; // we can assign a different type data 
myVar = true; // assigning boolean value
```

>We should avoid using the type ‘dynamic’ in Dart unless we do not have any other option.