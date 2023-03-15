# StreamBuilder<T> class (*Null safety*)

Widget that builds itself based on the latest snapshot of interaction with a Stream.

Widget rebuilding is scheduled by each interaction, using `State.setState`, but is otherwise decoupled from the timing of the stream. The **builder** is called at the discretion of the Flutter pipeline, and will thus receive a timing-dependent sub-sequence of the snapshots that represent the interaction with the stream.

As an example, when interacting with a stream producing the integers 0 through 9, the **builder** may be called with any ordered sub-sequence of the following snapshots that includes the last one (the one with `ConnectionState.done`):

- `AsyncSnapshot<int>.withData(ConnectionState.waiting, null)`
- `AsyncSnapshot<int>.withData(ConnectionState.active, 0)`
- `AsyncSnapshot<int>.withData(ConnectionState.active, 1)`
- ...
- `AsyncSnapshot<int>.withData(ConnectionState.active, 9)`
- `AsyncSnapshot<int>.withData(ConnectionState.done, 9)`

The actual sequence of invocations of the `builder` depends on the relative timing of events produced by the stream and the build rate of the Flutter pipeline.

Changing the `StreamBuilder` configuration to another stream during event generation introduces snapshot pairs of the form:

- `AsyncSnapshot<int>.withData(ConnectionState.none, 5)`
- `AsyncSnapshot<int>.withData(ConnectionState.waiting, 5)`

The latter will be produced only when the new stream is non-null, and the former only when the old stream is non-null.

The stream may produce errors, resulting in snapshots of the form:

- `AsyncSnapshot<int>.withError(ConnectionState.active, 'some error', someStackTrace)`

The data and error fields of snapshots produced are only changed when the state is `ConnectionState.active`.

The initial snapshot data can be controlled by specifying **initialData**. This should be used to ensure that the first frame has the expected value, as the builder will always be called before the stream listener has a chance to be processed.

```dart
import 'dart:async';

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
  final Stream<int> _bids = (() {
    late final StreamController<int> controller;
    controller = StreamController<int>(
      onListen: () async {
        await Future<void>.delayed(const Duration(seconds: 1));
        controller.add(1);
        await Future<void>.delayed(const Duration(seconds: 1));
        await controller.close();
      },
    );
    return controller.stream;
  })();

  @override
  Widget build(BuildContext context) {
    return DefaultTextStyle(
      style: Theme.of(context).textTheme.displayMedium!,
      textAlign: TextAlign.center,
      child: Container(
        alignment: FractionalOffset.center,
        color: Colors.white,
        child: StreamBuilder<int>(
          stream: _bids,
          builder: (BuildContext context, AsyncSnapshot<int> snapshot) {
            List<Widget> children;
            if (snapshot.hasError) {
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
                Padding(
                  padding: const EdgeInsets.only(top: 8),
                  child: Text('Stack trace: ${snapshot.stackTrace}'),
                ),
              ];
            } else {
              switch (snapshot.connectionState) {
                case ConnectionState.none:
                  children = const <Widget>[
                    Icon(
                      Icons.info,
                      color: Colors.blue,
                      size: 60,
                    ),
                    Padding(
                      padding: EdgeInsets.only(top: 16),
                      child: Text('Select a lot'),
                    ),
                  ];
                  break;
                case ConnectionState.waiting:
                  children = const <Widget>[
                    SizedBox(
                      width: 60,
                      height: 60,
                      child: CircularProgressIndicator(),
                    ),
                    Padding(
                      padding: EdgeInsets.only(top: 16),
                      child: Text('Awaiting bids...'),
                    ),
                  ];
                  break;
                case ConnectionState.active:
                  children = <Widget>[
                    const Icon(
                      Icons.check_circle_outline,
                      color: Colors.green,
                      size: 60,
                    ),
                    Padding(
                      padding: const EdgeInsets.only(top: 16),
                      child: Text('\$${snapshot.data}'),
                    ),
                  ];
                  break;
                case ConnectionState.done:
                  children = <Widget>[
                    const Icon(
                      Icons.info,
                      color: Colors.blue,
                      size: 60,
                    ),
                    Padding(
                      padding: const EdgeInsets.only(top: 16),
                      child: Text('\$${snapshot.data} (closed)'),
                    ),
                  ];
                  break;
              }
            }

            return Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: children,
            );
          },
        ),
      ),
    );
  }
}
```

This sample shows a **StreamBuilder** that listens to a Stream that emits bids for an auction. Every time the StreamBuilder receives a bid from the Stream, it will display the price of the bid below an icon. If the Stream emits an error, the error is displayed below an error icon. When the Stream finishes emitting bids, the final price is displayed.