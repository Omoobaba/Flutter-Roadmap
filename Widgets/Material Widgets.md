# Material Widgets

Material Widgets are a set of Flutter widgets that implement Material Design, Google’s visual language for design. They are designed to provide a consistent look and feel on both Android and iOS devices. Some common Material Widgets include:

- RaisedButton
- Scaffold
- AppBar
- TextField
- Drawer
- SnackBar
- BottomNavigationBar
- IconButton

These widgets are commonly used in Flutter apps to provide a familiar look and feel that follows Material Design guidelines.

# Material Components widgets
Visual, behavioral, and motion-rich widgets implementing the Material Design guidelines.

- App structure and navigation
- Buttons
- Input and selections
- Dialogs, alerts, and panels
- Information displays
- Layout

## AppBar

A Material Design app bar. An app bar consists of a toolbar and potentially other widgets, such as a TabBar and a FlexibleSpaceBar.

A Material Design app bar.

An app bar consists of a `toolbar` and potentially other widgets, such as a `TabBar` and a `FlexibleSpaceBar`. App bars typically expose one or more common *actions* with `IconButtons` which are optionally followed by a `PopupMenuButton` for less common operations (sometimes called the "overflow menu").

App bars are typically used in the `Scaffold.appBar` property, which places the app bar as a fixed-height widget at the top of the screen. For a scrollable app bar, see `SliverAppBar`, which embeds an `AppBar` in a `sliver` for use in a `CustomScrollView`.

The AppBar displays the toolbar widgets, `leading`, `title`, and `actions`, above the `bottom` (if any). The `bottom` is usually used for a `TabBar`. If a `flexibleSpace` widget is specified then it is stacked behind the toolbar and the bottom widget. The following diagram shows where each of these slots appears in the toolbar when the writing language is left-to-right (e.g. English):

The `AppBar` insets its content based on the ambient `MediaQuery`'s padding, to avoid system UI intrusions. It's taken care of by `Scaffold` when used in the `Scaffold.appBar` property. When animating an `AppBar`, unexpected `MediaQuery` changes (as is common in `Hero` animations) may cause the content to suddenly jump. Wrap the `AppBar` in a `MediaQuery` widget, and adjust its padding such that the animation is smooth.

![AppBar](https://flutter.github.io/assets-for-api-docs/assets/material/app_bar.png)

If the `leading` widget is omitted, but the `AppBar` is in a `Scaffold` with a `Drawer`, then a button will be inserted to open the drawer. Otherwise, if the nearest `Navigator` has any previous routes, a `BackButton` is inserted instead. This behavior can be turned off by setting the `automaticallyImplyLeading` to `false`. In that case a null leading widget will result in the middle/title widget stretching to start.

> This sample shows an AppBar with two simple actions. The first action opens a SnackBar, while the second action navigates to a new page.

```dart
import 'package:flutter/material.dart';

void main() => runApp(const AppBarApp());

class AppBarApp extends StatelessWidget {
  const AppBarApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      home: AppBarExample(),
    );
  }
}

class AppBarExample extends StatelessWidget {
  const AppBarExample({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('AppBar Demo'),
        actions: <Widget>[
          IconButton(
            icon: const Icon(Icons.add_alert),
            tooltip: 'Show Snackbar',
            onPressed: () {
              ScaffoldMessenger.of(context).showSnackBar(
                  const SnackBar(content: Text('This is a snackbar')));
            },
          ),
          IconButton(
            icon: const Icon(Icons.navigate_next),
            tooltip: 'Go to the next page',
            onPressed: () {
              Navigator.push(context, MaterialPageRoute<void>(
                builder: (BuildContext context) {
                  return Scaffold(
                    appBar: AppBar(
                      title: const Text('Next page'),
                    ),
                    body: const Center(
                      child: Text(
                        'This is the next page',
                        style: TextStyle(fontSize: 24),
                      ),
                    ),
                  );
                },
              ));
            },
          ),
        ],
      ),
      body: const Center(
        child: Text(
          'This is the home page',
          style: TextStyle(fontSize: 24),
        ),
      ),
    );
  }
}

```

> This sample shows the creation of an AppBar widget with the shadowColor and scrolledUnderElevation properties set

```dart
import 'package:flutter/material.dart';

final List<int> _items = List<int>.generate(51, (int index) => index);

void main() => runApp(const AppBarApp());

class AppBarApp extends StatelessWidget {
  const AppBarApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      theme: ThemeData(
        colorSchemeSeed: const Color(0xff6750a4),
        useMaterial3: true,
      ),
      home: const AppBarExample(),
    );
  }
}

class AppBarExample extends StatefulWidget {
  const AppBarExample({super.key});

  @override
  State<AppBarExample> createState() => _AppBarExampleState();
}

class _AppBarExampleState extends State<AppBarExample> {
  bool shadowColor = false;
  double? scrolledUnderElevation;

  @override
  Widget build(BuildContext context) {
    final ColorScheme colorScheme = Theme.of(context).colorScheme;
    final Color oddItemColor = colorScheme.primary.withOpacity(0.05);
    final Color evenItemColor = colorScheme.primary.withOpacity(0.15);

    return Scaffold(
      appBar: AppBar(
        title: const Text('AppBar Demo'),
        scrolledUnderElevation: scrolledUnderElevation,
        shadowColor: shadowColor ? Theme.of(context).colorScheme.shadow : null,
      ),
      body: GridView.builder(
        itemCount: _items.length,
        padding: const EdgeInsets.all(8.0),
        gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
          crossAxisCount: 3,
          childAspectRatio: 2.0,
          mainAxisSpacing: 10.0,
          crossAxisSpacing: 10.0,
        ),
        itemBuilder: (BuildContext context, int index) {
          if (index == 0) {
            return Center(
              child: Text(
                'Scroll to see the Appbar in effect.',
                style: Theme.of(context).textTheme.labelLarge,
                textAlign: TextAlign.center,
              ),
            );
          }
          return Container(
            alignment: Alignment.center,
            // tileColor: _items[index].isOdd ? oddItemColor : evenItemColor,
            decoration: BoxDecoration(
              borderRadius: BorderRadius.circular(20.0),
              color: _items[index].isOdd ? oddItemColor : evenItemColor,
            ),
            child: Text('Item $index'),
          );
        },
      ),
      bottomNavigationBar: BottomAppBar(
        child: Padding(
          padding: const EdgeInsets.all(8),
          child: OverflowBar(
            overflowAlignment: OverflowBarAlignment.center,
            alignment: MainAxisAlignment.center,
            overflowSpacing: 5.0,
            children: <Widget>[
              ElevatedButton.icon(
                onPressed: () {
                  setState(() {
                    shadowColor = !shadowColor;
                  });
                },
                icon: Icon(
                  shadowColor ? Icons.visibility_off : Icons.visibility,
                ),
                label: const Text('shadow color'),
              ),
              const SizedBox(width: 5),
              ElevatedButton.icon(
                onPressed: () {
                  if (scrolledUnderElevation == null) {
                    setState(() {
                      // Default elevation is 3.0, increment by 1.0.
                      scrolledUnderElevation = 4.0;
                    });
                  } else {
                    setState(() {
                      scrolledUnderElevation = scrolledUnderElevation! + 1.0;
                    });
                  }
                },
                icon: const Icon(Icons.add),
                label: Text(
                  'scrolledUnderElevation: ${scrolledUnderElevation ?? 'default'}',
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

```

## BottomNavigationBar Class

A material widget that's displayed at the bottom of an app for selecting among a small number of views, typically between three and five.

The bottom navigation bar consists of multiple items in the form of text labels, icons, or both, laid out on top of a piece of material. It provides quick navigation between the top-level views of an app. For larger screens, side navigation may be a better fit.

A bottom navigation bar is usually used in conjunction with a `Scaffold`, where it is provided as the `Scaffold.bottomNavigationBar` argument.

The bottom navigation bar's *type* changes how its *items* are displayed. If not specified, then it's automatically set to `BottomNavigationBarType.fixed` when there are less than four items, and `BottomNavigationBarType.shifting` otherwise.

The length of `items` must be at least two and each item's icon and title/label must not be null.

- `BottomNavigationBarType.fixed`, the default when there are less than four items. The selected item is rendered with the `selectedItemColor` if it's non-null, otherwise the theme's `ColorScheme.primary` color is used for `Brightness.light` themes and `ColorScheme.secondary` for `Brightness.dark` themes. If `backgroundColor` is null, The navigation bar's background color defaults to the Material background color, `ThemeData.canvasColor` (essentially opaque white).
- `BottomNavigationBarType.shifting`, the default when there are four or more items. If `selectedItemColor` is null, all items are rendered in white. The navigation bar's background color is the same as the `BottomNavigationBarItem.backgroundColor` of the selected item. In this case it's assumed that each item will have a different background color and that background color will contrast well with white.

> This example shows a `BottomNavigationBar` as it is used within a `Scaffold` widget. The `BottomNavigationBar` has three `BottomNavigationBarItem` widgets, which means it defaults to `BottomNavigationBarType.fixed`, and the `currentIndex` is set to index 0. The selected item is amber. The _onItemTapped function changes the selected item's index and displays a corresponding message in the center of the `Scaffold`.

```dart
import 'package:flutter/material.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  static const String _title = 'Flutter Code Sample';

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      title: _title,
      home: MyStatefulWidget(),
    );
  }
}

class MyStatefulWidget extends StatefulWidget {
  const MyStatefulWidget({super.key});

  @override
  State<MyStatefulWidget> createState() => _MyStatefulWidgetState();
}

class _MyStatefulWidgetState extends State<MyStatefulWidget> {
  int _selectedIndex = 0;
  static const TextStyle optionStyle =
      TextStyle(fontSize: 30, fontWeight: FontWeight.bold);
  static const List<Widget> _widgetOptions = <Widget>[
    Text(
      'Index 0: Home',
      style: optionStyle,
    ),
    Text(
      'Index 1: Business',
      style: optionStyle,
    ),
    Text(
      'Index 2: School',
      style: optionStyle,
    ),
  ];

  void _onItemTapped(int index) {
    setState(() {
      _selectedIndex = index;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('BottomNavigationBar Sample'),
      ),
      body: Center(
        child: _widgetOptions.elementAt(_selectedIndex),
      ),
      bottomNavigationBar: BottomNavigationBar(
        items: const <BottomNavigationBarItem>[
          BottomNavigationBarItem(
            icon: Icon(Icons.home),
            label: 'Home',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.business),
            label: 'Business',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.school),
            label: 'School',
          ),
        ],
        currentIndex: _selectedIndex,
        selectedItemColor: Colors.amber[800],
        onTap: _onItemTapped,
      ),
    );
  }
}

```

## Drawer

A Material Design panel that slides in horizontally from the edge of a `Scaffold` to show navigation links in an application.

Drawers are typically used with the `Scaffold.drawer` property. The child of the drawer is usually a `ListView` whose first child is a `DrawerHeader` that displays status information about the current user. The remaining drawer children are often constructed with `ListTiles`, often concluding with an `AboutListTile`.

The AppBar automatically displays an appropriate `IconButton` to show the `Drawer` when a `Drawer` is available in the `Scaffold`. The `Scaffold` automatically handles the edge-swipe gesture to show the drawer.

> This example shows how to create a Scaffold that contains an AppBar and a Drawer. A user taps the "menu" icon in the AppBar to open the Drawer. The Drawer displays four items: A header and three menu items. The Drawer displays the four items using a ListView, which allows the user to scroll through the items if need be.

```dart
Scaffold(
  appBar: AppBar(
    title: const Text('Drawer Demo'),
  ),
  drawer: Drawer(
    child: ListView(
      padding: EdgeInsets.zero,
      children: const <Widget>[
        DrawerHeader(
          decoration: BoxDecoration(
            color: Colors.blue,
          ),
          child: Text(
            'Drawer Header',
            style: TextStyle(
              color: Colors.white,
              fontSize: 24,
            ),
          ),
        ),
        ListTile(
          leading: Icon(Icons.message),
          title: Text('Messages'),
        ),
        ListTile(
          leading: Icon(Icons.account_circle),
          title: Text('Profile'),
        ),
        ListTile(
          leading: Icon(Icons.settings),
          title: Text('Settings'),
        ),
      ],
    ),
  ),
)
```

An open drawer may be closed with a swipe to close gesture, pressing the the escape key, by tapping the scrim, or by calling pop route function such as `Navigator.pop`. For example a drawer item might close the drawer when tapped:

```dart
ListTile(
  leading: const Icon(Icons.change_history),
  title: const Text('Change history'),
  onTap: () {
    // change app state...
    Navigator.pop(context); // close the drawer
  },
);
```

# Material Design Color System

A color theme in Material refers to a restrained set of colors that define an interface. The default Material Theme incorporates vivid, colors like the primary and secondary, as well as color slots for backgrounds, surfaces, errors, and more. Defining a color theme. This way is helpful because it helps craft a consistent and rational color story that can be used globally and consistently throughout your app. This helps users perceive content and more easily know how the app works. 

![Material Color](https://media.geeksforgeeks.org/wp-content/uploads/20210202211241/Screenshotfrom20210202211220.png)

One refinement of the color theme is. On Colors. On colors are named so because they appear on other elements. For example, the *on background* color for the below app is black, because the text that appears on the background is black, as well as other elements like icons. That also appears on the background. On colors are useful because they can help ensure that content appearing in various contexts throughout your app remains consistent and readable.

![Example](https://media.geeksforgeeks.org/wp-content/uploads/20210202214241/Screenshotfrom20210202214231.png)

They also give you a steady global control over things like color contrast, so you don’t need to fine-tune the text on every surface with the same color as long as the corresponding on color for those surfaces meets contrast requirements. These colors can also have different emphasis levels, which can be helpful for transferring information in state-rich situations or when you create a dark theme.

![Alt text](https://media.geeksforgeeks.org/wp-content/uploads/20210202231750/Screenshotfrom20210202231700.png)

# Material Typography

Typography in material design is based on a type scale. A type scale is a hierarchy of type styles that can be used for various circumstances throughout a layout. The Material type scale is a mixture of 13 reusable styles that each have an intended application and meaning, all the way from big headline styles to body text captions and buttons. Working out a good type scale for your app keeps typography consistent and significant to users, while providing enough stylistic options to create a compelling appearances and character.

![Typography chart](https://media.geeksforgeeks.org/wp-content/uploads/20210203003954/Screenshotfrom20210203003848.png)

Type is one of the subsystems that allows you to theme Material components, customizing them to create something unique. Each component’s typography fits into a category. For example, the text on a button belongs to the button category, which is shared by the buttons in a dialog box and tab labels. So changing attributes like typeface style, size, and spacing causes a ripple effect. It customizes all the typography belonging to that category.

![Example](https://media.geeksforgeeks.org/wp-content/uploads/20210203004605/Screenshotfrom20210203004553.png)

