# Stateless Widgets

Stateless widgets in Flutter are widgets that don’t maintain any mutable state. They are designed to be immutable and rebuild each time the framework needs to update the UI. They are suitable for static, unchanging views or simple animations. They can be created using the `StatelessWidget` class and have a single build method that returns a widget tree.

# StatelessWidget class

A widget that does not require mutable state.

A stateless widget is a widget that describes part of the user interface by building a constellation of other widgets that describe the user interface more concretely. The building process continues recursively until the description of the user interface is fully concrete (e.g., consists entirely of *RenderObjectWidgets*, which describe concrete *RenderObjects*).

Stateless widget are useful when the part of the user interface you are describing does not depend on anything other than the configuration information in the object itself and the **BuildContext** in which the widget is inflated. For compositions that can change dynamically, e.g. due to having an internal clock-driven state, or depending on some system state, consider using **StatefulWidget**.

## Performance considerations

The **build** method of a stateless widget is typically only called in three situations: the first time the widget is inserted in the tree, when the widget's parent changes its configuration, and when an **InheritedWidget** it depends on changes.

If a widget's parent will regularly change the widget's configuration, or if it depends on inherited widgets that frequently change, then it is important to optimize the performance of the **build** method to maintain a fluid rendering performance.

There are several techniques one can use to minimize the impact of rebuilding a stateless widget:

- Minimize the number of nodes transitively created by the build method and any widgets it creates. For example, instead of an elaborate arrangement of Rows, Columns, Paddings, and SizedBoxes to position a single child in a particularly fancy manner, consider using just an Align or a CustomSingleChildLayout. Instead of an intricate layering of multiple Containers and with Decorations to draw just the right graphical effect, consider a single CustomPaint widget.

- Use const widgets where possible, and provide a const constructor for the widget so that users of the widget can also do so.

- Consider refactoring the stateless widget into a stateful widget so that it can use some of the techniques described at StatefulWidget, such as caching common parts of subtrees and using GlobalKeys when changing the tree structure.

- If the widget is likely to get rebuilt frequently due to the use of InheritedWidgets, consider refactoring the stateless widget into multiple widgets, with the parts of the tree that change being pushed to the leaves. For example instead of building a tree with four widgets, the inner-most widget depending on the Theme, consider factoring out the part of the build function that builds the inner-most widget into its own widget, so that only the inner-most widget needs to be rebuilt when the theme changes.

- When trying to create a reusable piece of UI, prefer using a widget rather than a helper method. For example, if there was a function used to build a widget, a `State.setState` call would require Flutter to entirely rebuild the returned wrapping widget. If a Widget was used instead, Flutter would be able to efficiently re-render only those parts that really need to be updated. Even better, if the created widget is const, Flutter would short-circuit most of the rebuild work.

>The following is a skeleton of a stateless widget subclass called GreenFrog. Normally, widgets have more constructor arguments, each of which corresponds to a final property.

```dart
class GreenFrog extends StatelessWidget {
  const GreenFrog({ super.key });

  @override
  Widget build(BuildContext context) {
    return Container(color: const Color(0xFF2DBD3A));
  }
}
```

>This next example shows the more generic widget Frog which can be given a color and a child:

```dart
class Frog extends StatelessWidget {
  const Frog({
    super.key,
    this.color = const Color(0xFF2DBD3A),
    this.child,
  });

  final Color color;
  final Widget? child;

  @override
  Widget build(BuildContext context) {
    return Container(color: color, child: child);
  }
}
```

By convention, widget constructors only use named arguments. Also by convention, the first argument is key, and the last argument is `child`, `children`, or the equivalent.

# Widget Trees and Element Trees
You might ask yourself, “I see how these build methods work, but when do they get called?” Well, let’s start with just a single DogName widget.

We tend to think of apps built with Flutter as a tree of widgets, and that’s not a bad thing. But as I mentioned at the beginning, widgets are really just configurations for pieces of an app’s UI. They’re blueprints. So what are these configurations for? Elements. An element is a widget that’s been made real and mounted onscreen. The element tree represents what is actually displayed on the device at any given moment.

Each widget class has both a corresponding element class and a method to create an instance.

![Widget and Element trees](https://miro.medium.com/v2/resize%3Afit%3A720/format%3Awebp/1%2AgG5opaiBFJ0A1ka9tZMeSQ.png)

StatelessWidget, for example, creates a StatelessElement.

When a widget is mounted to the tree, Flutter calls the `createElement()` method. Flutter asks the widget for an element, and pops that element onto the element tree with a reference back to the widget that created it.

StatefulElement then asks “I wonder if I have any children?” and calls the Widget’s `build()` method.

![Alt text](https://miro.medium.com/v2/resize%3Afit%3A720/format%3Awebp/1%2A0IZb-Rcyw4yBo3CXQM5zTg.png)

These widgets then create their own corresponding elements.

![Widget Tree](https://miro.medium.com/v2/resize%3Afit%3A720/format%3Awebp/1%2A7qIL1q-ZP005QS_zTt0j5A.png)

And those are mounted to the element tree as well.

![Element tree](https://miro.medium.com/v2/resize%3Afit%3A720/format%3Awebp/1%2AO-D8lfgoAmMqHViU2dWuyg.png)

So the app now has two trees: one that represents what’s actually on the screen (the elements), and one that holds the blueprints they were made from (the widgets).

Now you might be wondering what starts the process of building and creating elements, what kicks off the whole thing, so to speak. If you look at `main()`, which is the entry point for the app, you can see that it calls the `runApp()` method, and that’s the starting point. The `runApp()` method takes a widget and mounts it as the app’s root element with height and width constraints that match the size of the screen.

```dart
void main() {
 runApp(new DogApp());  //The runApp() method takes a widget and mounts it as the app’s root element
}
```

Then, Flutter progresses through all of the build() methods in the widget tree, creating widgets and using them to make elements, until everything is built, mounted onscreen, and ready to be laid out and rendered.

![Final Hierarchy](https://miro.medium.com/v2/resize:fit:1100/0*QLq-DId9PheGXmk9)

Which is how it displays three little boxes containing the names of the yellow labs.

![Alt text](https://miro.medium.com/v2/resize%3Afit%3A640/format%3Awebp/1%2AlRBiFALMxP8-RPAsmlk7pw.png)