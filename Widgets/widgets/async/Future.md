# FutureBuilder<T> class (*Null safety*)

Widget that builds itself based on the latest snapshot of interaction with a `Future`.

The `future` must have been obtained earlier, e.g. during `State.initState`, `State.didUpdateWidget`, or `State.didChangeDependencies`. It must not be created during the `State.build` or `StatelessWidget.build` method call when constructing the `FutureBuilder`. If the `future` is created at the same time as the `FutureBuilder`, then every time the `FutureBuilder`'s parent is rebuilt, the asynchronous task will be restarted.

A general guideline is to assume that every build method could get called every frame, and to treat omitted calls as an optimization.

## Timing

Widget rebuilding is scheduled by the completion of the future, using `State.setState`, but is otherwise decoupled from the timing of the future. The `builder` callback is called at the discretion of the Flutter pipeline, and will thus receive a timing-dependent sub-sequence of the snapshots that represent the interaction with the future.

A side-effect of this is that providing a new but already-completed future to a `FutureBuilder` will result in a single frame in the `ConnectionState.waiting` state. This is because there is no way to synchronously determine that a `Future` has already completed.

## Builder contract

For a future that completes successfully with data, assuming `initialData` is null, the `builder` will be called with either both or only the latter of the following snapshots:

- `AsyncSnapshot<String>.withData(ConnectionState.waiting, null)`
- `AsyncSnapshot<String>.withData(ConnectionState.done, 'some data')`

If that same future instead completed with an error, the `builder` would be called with either both or only the latter of:

- `AsyncSnapshot<String>.withData(ConnectionState.waiting, null)`
- `AsyncSnapshot<String>.withError(ConnectionState.done, 'some error', someStackTrace)`

The initial snapshot data can be controlled by specifying `initialData`. You would use this facility to ensure that if the `builder` is invoked before the future completes, the snapshot carries data of your choice rather than the default null value.

The data and error fields of the snapshot change only as the connection state field transitions from waiting to done, and they will be retained when changing the `FutureBuilder` configuration to another future. If the old future has already completed successfully with data as above, changing configuration to a new future results in snapshot pairs of the form:

- `AsyncSnapshot<String>.withData(ConnectionState.none, 'data of first future')`

- `AsyncSnapshot<String>.withData(ConnectionState.waiting, 'data of second future')`

In general, the latter will be produced only when the new future is non-null, and the former only when the old future is non-null.

A **FutureBuilder** behaves identically to a **StreamBuilder** configured with `future?.asStream()`, except that snapshots with `ConnectionState.active` may appear for the latter, depending on how the stream is implemented.

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
  final Future<String> _calculation = Future<String>.delayed(
    const Duration(seconds: 2),
    () => 'Data Loaded',
  );

  @override
  Widget build(BuildContext context) {
    return DefaultTextStyle(
      style: Theme.of(context).textTheme.displayMedium!,
      textAlign: TextAlign.center,
      child: FutureBuilder<String>(
        future: _calculation, // a previously-obtained Future<String> or null
        builder: (BuildContext context, AsyncSnapshot<String> snapshot) {
          List<Widget> children;
          if (snapshot.hasData) {
            children = <Widget>[
              const Icon(
                Icons.check_circle_outline,
                color: Colors.green,
                size: 60,
              ),
              Padding(
                padding: const EdgeInsets.only(top: 16),
                child: Text('Result: ${snapshot.data}'),
              ),
            ];
          } else if (snapshot.hasError) {
            children = <Widget>[
              const Icon(
                Icons.error_outline,
                color: Colors.red,
                size: 60,
              ),
              Padding(
                padding: const EdgeInsets.only(top: 16),
                child: Text('Error: ${snapshot.error}'),
              ),
            ];
          } else {
            children = const <Widget>[
              SizedBox(
                width: 60,
                height: 60,
                child: CircularProgressIndicator(),
              ),
              Padding(
                padding: EdgeInsets.only(top: 16),
                child: Text('Awaiting result...'),
              ),
            ];
          }
          return Center(
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: children,
            ),
          );
        },
      ),
    );
  }
}

```

This sample shows a **FutureBuilder** that displays a **loading spinner** while it loads data. It displays a success icon and text if the **Future** completes with a result, or an error icon and text if the **Future** completes with an error. Assume the `_calculation` field is set by pressing a button elsewhere in the UI.