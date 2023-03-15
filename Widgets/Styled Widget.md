# Styled Widget

Styled Widgets are Flutter widgets that are decorated with custom styles, such as colors, fonts, and shapes. They can be created by wrapping existing widgets with other widgets, such as Container, Theme, or BoxDecoration. For example:

- Container widget can be used to set a fixed width, height, padding, and margin.
- Theme widget can be used to specify a color scheme and typography for an entire app or a section of it.
- BoxDecoration can be used to add a border, background color, and a border radius to a widget.
- Styled Widgets allow developers to easily customize the look and feel of their Flutter app and create a consistent visual style.

Manage the theme of your app, makes your app responsive to screen sizes, or add padding.

# MediaQuery Class

Establishes a subtree in which media queries resolve to the given data.

For example, to learn the size of the current media (e.g., the window containing your app), you can read the `MediaQueryData.size` property from the `MediaQueryData` returned by `MediaQuery.of`: `MediaQuery.of(context).size`.

Querying the current media using `MediaQuery.of` will cause your widget to rebuild automatically whenever the `MediaQueryData` changes (e.g., if the user rotates their device).

If no `MediaQuery` is in scope then the `MediaQuery.of` method will throw an exception. Alternatively, `MediaQuery.maybeOf` may be used, which returns null instead of throwing if no `MediaQuery` is in scope.

# Padding

A widget that insets its child by the given padding.

When passing layout constraints to its child, padding shrinks the constraints by the given padding, causing the child to layout at a smaller size. Padding then sizes itself to its child's size, inflated by the padding, effectively creating empty space around the child.

> This snippet creates "Hello World!" Text inside a Card that is indented by sixteen pixels in each direction.

```dart
const Card(
  child: Padding(
    padding: EdgeInsets.all(16.0),
    child: Text('Hello World!'),
  ),
)
```

![OutPut](https://flutter.github.io/assets-for-api-docs/assets/widgets/padding.png)

## Design Discussion

Why use a `Padding` widget rather than a `Container` with a `Container.padding` property?
There isn't really any difference between the two. If you supply a `Container.padding` argument, `Container` simply builds a Padding widget for you.

`Container` doesn't implement its properties directly. Instead, `Container` combines a number of simpler widgets together into a convenient package. For example, the `Container.padding` property causes the container to build a Padding widget and the `Container.decoration` property causes the container to build a `DecoratedBox` widget. If you find `Container` convenient, feel free to use it. If not, feel free to build these simpler widgets in whatever combination meets your needs.

> In fact, the majority of widgets in Flutter are simply combinations of other simpler widgets. Composition, rather than inheritance, is the primary mechanism for building up widgets.

## Theme

Applies a theme to descendant widgets. A theme describes the colors and typographic choices of an application.

Applies a theme to descendant widgets.

A theme describes the colors and typographic choices of an application.

Descendant widgets obtain the current theme's `ThemeData` object using `Theme.of`. When a widget uses `Theme.of`, it is automatically rebuilt if the theme later changes, so that the changes can be applied.

The `Theme` widget implies an `IconTheme` widget, set to the value of the `ThemeData.iconTheme` of the *data* for the `Theme`.

