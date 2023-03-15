# Control Flow Statements

In Dart, control flow statements are used to control the flow of execution of a program. The following are the main types of control flow statements in Dart:

- if-else: used to conditionally execute code based on a boolean expression.
- for loop: used to repeat a block of code a specific number of times.
- while loop: used to repeat a block of code as long as a given condition is true.
- do-while loop: similar to the while loop, but the block of code is executed at least once before the condition is evaluated.
- switch-case: used to select one of several code blocks to execute based on a value.
- break: used to exit a loop early.
- continue: used to skip the current iteration of a loop and continue with the next one.

These control flow statements can be used to create complex logic and control the flow of execution in Dart programs.

## If and else

Dart supports `if` statements with optional `else` statements, as the next sample shows.

```dart
if (isRaining()) {
  you.bringRainCoat();
} else if (isSnowing()) {
  you.wearJacket();
} else {
  car.putTopDown();
}
```

>The statement conditions must be expressions that evaluate to boolean values, nothing else.

## For loops

You can iterate with the standard fo`r loop. For example:

```dart
var message = StringBuffer('Dart is fun');
for (var i = 0; i < 5; i++) {
  message.write('!');
}
```

Closures inside of Dart’s `for` loops capture the *value* of the index, avoiding a common pitfall found in JavaScript. For example, consider:

```dart
var callbacks = [];
for (var i = 0; i < 2; i++) {
  callbacks.add(() => print(i));
}

for (final c in callbacks) {
  c();
}
```

The output is `0` and then `1`, as expected. In contrast, the example would print `2` and then `2` in JavaScript.

If the object that you are iterating over is an Iterable (such as List or Set) and if you don’t need to know the current iteration counter, you can use the `for-in` form of iteration:

```dart
for (final candidate in candidates) {
  candidate.interview();
}
```

Iterable classes also have a `forEach()` method as another option:

```dart
var collection = [1, 2, 3];
collection.forEach(print); // 1 2 3
```

## While and do-while

A `while` loop evaluates the condition *before* the loop:

```dart
while (!isDone()) {
  doSomething();
}
```

A `do-while` loop evaluates the condition *after* the loop:

```dart
do {
  printLine();
} while (!atEndOfPage());
```

## Break and continue

Use `break` to stop looping:

```dart
while (true) {
  if (shutDownRequested()) break;
  processIncomingRequests();
}
```

Use `continue` to skip to the next loop iteration:

```dart
for (int i = 0; i < candidates.length; i++) {
  var candidate = candidates[i];
  if (candidate.yearsExperience < 5) {
    continue;
  }
  candidate.interview();
}
```

You might write that example differently if you’re using an Iterable such as a list or set:

```dart
candidates
    .where((c) => c.yearsExperience >= 5)
    .forEach((c) => c.interview());
```