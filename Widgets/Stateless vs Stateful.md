The state of an app can very simply be defined as anything that exists in the memory of the app while the app is running. This includes all the widgets that maintain the UI of the app including the buttons, text fonts, icons, animations, etc. So now as we know what are these states let’s dive directly into our main topic i.e what are these stateful and stateless widgets and how do they differ from one another.

**State:** The State is the information that can be read synchronously when the widget is built and might change during the lifetime of the widget.  

In other words, the state of the widget is the data of the objects that its properties (parameters) are sustaining at the time of its creation (when the widget is painted on the screen). The state can also change when it is used for example when a *CheckBox* widget is clicked a check appears on the box.

**Stateless Widgets:** The widgets whose state can not be altered once they are built are called stateless widgets. These widgets are immutable once they are built i.e any amount of change in the variables, icons, buttons, or retrieving data can not change the state of the app. Below is the basic structure of a *stateless* widget. *Stateless* widget overrides the `build()` method and returns a widget. For example, we use *Text* or the *Icon* in our flutter application where the state of the widget does not change in the *runtime*. It is used when the UI depends on the information within the object itself. Other examples can be *Text*, *RaisedButton*, *IconButtons*.

### Example
```dart
import 'package:flutter/material.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
    const MyApp({Key? key}) : super(key: key);

    @override
    Widget build(BuildContext context) {
    	return Container();
    }
}
```

So let us see what this small code snippet tells us. The name of the stateless widget is MyApp which is being called from the `runApp()` and extends a stateless widget. Inside this MyApp a build function is overridden and takes *BuildContext* as a parameter. This *BuildContext* is unique to each and every widget as it is used to locate the widget inside the widget tree.

>**Note:** The widgets of a Flutter application are displayed in the form of a Widget Tree where we connect the parent and child widgets to show a relationship between them which then combines to form the state of your app.

The build function contains a container which is again a widget of Flutter inside which we will design the UI of the app.  In the stateless widget, the build function is called only once which makes the UI of the screen.

### **Example:** Stateless Widget

```dart
import 'package:flutter/material.dart';
 
//This function triggers the build process
void main() => runApp(const MyApp());
 
class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);
  
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      home: Scaffold(
        backgroundColor: const Color.fromARGB(255, 230, 255, 201),
        appBar: AppBar(
          leading: const Icon(Icons.menu),
          backgroundColor: Colors.green,
          title: const Text(
            "GeeksforGeeks",
            textAlign: TextAlign.start,
          ),
        ), // AppBar
        body: const Center(
          child: Text(
            "Stateless Widget",
            style: TextStyle(color: Colors.black, fontSize: 30),
          ),
        ), // Container
      ), // Scaffold
    ); // MaterialApp
  }
```

### **Output:**

![Stateless app](https://media.geeksforgeeks.org/wp-content/uploads/20220526004026/Screenshot1653505423-371x660.png)

**Stateful Widgets:** The widgets whose state can be altered once they are built are called stateful Widgets. These states are mutable and can be changed multiple times in their lifetime. This simply means the state of an app can change multiple times with different sets of variables, inputs, data. Below is the basic structure of a stateful widget. *Stateful* widget overrides the `createState()` method and returns a S*tate. It is used when the UI can change dynamically. Some examples can be *CheckBox*, *RadioButton*, *Form*, *TextField*.

Classes that inherit “Stateful Widget” are immutable. But the State is mutable which changes in the runtime when the user interacts with it.

```dart
import 'package:flutter/material.dart';
 
void main() => runApp(const MyApp());
 
class MyApp extends StatefulWidget {
  const MyApp({Key? key}) : super(key: key);
 
  @override
  // ignore: library_private_types_in_public_api
  _MyAppState createState() => _MyAppState();
}
 
class _MyAppState extends State<MyApp> {
  @override
  Widget build(BuildContext context) {
    return Container();
  }
}
```

So let us see what we have in this code snippet. The name of the Stateful Widget is MyApp which is called from the `runApp()` and extends a stateful widget. In the MyApp class, we override the create state function. This `createState()` function is used to create a mutable state for this widget at a given location in the tree. This method returns an instance for the respected state subclass. The other class which is `_MyAppState` extends the state, which manages all the changes in the widget. Inside this class, the build function is overridden which takes the `BuildContext` as the parameter. This build function returns a widget where we design the UI of the app. Since it is a stateful widget the build function is called many times which creates the entire UI once again with all the changes.

### **Example:** Stateful Widget

```dart
import 'package:flutter/material.dart';
 
//This function triggers the build process
void main() => runApp(const MyApp());
 
// StatefulWidget
class MyApp extends StatefulWidget {
  const MyApp({Key? key}) : super(key: key);
 
  @override
  // ignore: library_private_types_in_public_api
  _MyAppState createState() => _MyAppState();
}
 
class _MyAppState extends State<MyApp> {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      home: Scaffold(
        backgroundColor: Color.fromARGB(255, 230, 255, 201),
        appBar: AppBar(
          leading: const Icon(Icons.menu),
          backgroundColor: Colors.green,
          title: const Text(
            "GeeksforGeeks",
            textAlign: TextAlign.start,
          ),
        ), // AppBar
        body: const Center(
          child: Text(
            "StateFul Widget",
            style: TextStyle(color: Colors.black, fontSize: 30),
          ),
        ), // Container
      ), // Scaffold
    ); // MaterialApp
  }
}
```

### **Output**:

![Output](https://media.geeksforgeeks.org/wp-content/uploads/20220526003242/Screenshot1653505309-371x660.png)

*Stateless* widget is useful when the part of the user interface you are describing does not depend on anything other than the configuration information and the *BuildContext* whereas a *Stateful* widget is useful when the part of the user interface you are describing can change *dynamically*.