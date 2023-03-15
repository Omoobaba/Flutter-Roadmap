# Functions

Dart is a true object-oriented language, so even functions are objects and have a type, Function. This means that functions can be assigned to variables or passed as arguments to other functions. You can also call an instance of a Dart class as if it were a function. For details, see Callable classes.

Here’s an example of implementing a function:

```dart
bool isNoble(int atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}
```

Although Effective Dart recommends type annotations for public APIs, the function still works if you omit the types:

```dart
isNoble(atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}
```

For functions that contain just on expression, you can use the shorthand syntax

```dart
bool isNoble(int atomicNumber) => _nobleGases[atomicNumber] != null;
```

The `=> *expr*` syntax is a shorthand for `{ return *expr*; }`. The `=>` notation is sometimes referred to as arrow syntax.

**Note:** Only an expression—not a statement—can appear between the arrow (=>) and the semicolon (;). For example, you can’t put an if statement there, but you can use a conditional expression.


# Defining a Function

A function can be defined by providing the name of the function with the appropriate parameter and return type. A function contains a set of statements which are called function body. The syntax is given below.

**Syntax:**
```dart
return_type func_name (parameter_list):  
{  
    //statement(s)  
   return value;  
}
```

Let's understand the general syntax of the defining function.

- **return_type** - It can be any data type such as void, integer, float, etc. The return type must be matched with the returned value of the function.
- **func_name** - It should be an appropriate and valid identifier.
- **parameter_list** - It denotes the list of the parameters, which is necessary when we called a function.
- **return value** - A function returns a value after complete its execution.

Let's understand the following example.

**Example - 1**

```dart
int mul(int a, int b){  
    int c;  
    c = a+b;  
    print("The sum is:${c}");  
}
```

# Parameters

A function can have any number of *required* positional parameters. These can be followed either by *named* parameters or by *optional positional* parameters (but not both).

>**Note:** Some APIs—notably Flutter widget constructors—use only named parameters, even for parameters that are mandatory. See the next section for details.

## Named parameters

Named parameters are optional unless they’re explicitly marked as `required`.

When defining a function, use `{param1, param2, …}` to specify named parameters. If you don’t provide a default value or mark a named parameter as `required`, their types must be nullable as their default value will be `null`:

```dart
/// Sets the [bold] and [hidden] flags ...
void enableFlags({bool? bold, bool? hidden}) {...}
```

When calling a function, you can specify named arguments using `paramName: value`. For example:

```dart
enableFlags(bold: true, hidden: false);
```

To define a default value for a named parameter besides `null`, use `=` to specify a default value. The specified value must be a compile-time constant. For example:

```dart
/// Sets the [bold] and [hidden] flags ...
void enableFlags({bool bold = false, bool hidden = false}) {...}

// bold will be true; hidden will be false.
enableFlags(bold: true);
```

If you instead want a named parameter to be mandatory, requiring callers to provide a value for the parameter, annotate them with `required`:

```dart
const Scrollbar({super.key, required Widget child});
```

If someone tries to create a `Scrollbar` without specifying the `child` argument, then the analyzer reports an issue.

>**Note:** A parameter marked as `required` can still be nullable:
>```dart
>const Scrollbar({super.key, required Widget? child});
>```

## Optional positional parameters

Wrapping a set of function parameters in `[]` marks them as optional positional parameters. If you don’t provide a default value, their types must be nullable as their default value will be `null`:

```dart
String say(String from, String msg, [String? device]) {
  var result = '$from says $msg';
  if (device != null) {
    result = '$result with a $device';
  }
  return result;
}
```

Here’s an example of calling this function without the optional parameter:

```dart
assert(say('Bob', 'Howdy') == 'Bob says Howdy');
```

And here’s an example of calling this function with the third parameter:

```dart
assert(say('Bob', 'Howdy', 'smoke signal') == 'Bob says Howdy with a smoke signal');
```

To define a default value for an optional positional parameter besides `null`, use `=` to specify a default value. The specified value must be a compile-time constant. For example:

```dart
String say(String from, String msg, [String device = 'carrier pigeon']) {
  var result = '$from says $msg with a $device';
  return result;
}

assert(say('Bob', 'Howdy') == 'Bob says Howdy with a carrier pigeon');
```

## The main() function

Every app must have a top-level `main()` function, which serves as the entrypoint to the app. The `main()` function returns `void` and has an optional `List<String>` parameter for arguments.

Here’s a simple main() function:

```dart
void main() {
  print('Hello, World!');
}

```
Here’s an example of the `main()` function for a command-line app that takes arguments:

```dart
// Run the app like this: dart args.dart 1 test
void main(List<String> arguments) {
  print(arguments);

  assert(arguments.length == 2);
  assert(int.parse(arguments[0]) == 1);
  assert(arguments[1] == 'test');
}
```

> You can use the args library to define and parse command-line arguments.