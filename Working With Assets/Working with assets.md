# Working with Assets

Assets are resources such as images, fonts, and other files that are included in your app. To use assets in Flutter, you need to specify them in your app’s `pubspec.yaml` file and then access them in your code.

Here’s how to work with assets in Flutter:

1. Add assets to your app’s `pubspec.yaml` file:
1. Access assets in your code

The `pubspec.yaml` file is used to manage dependencies, assets, and other settings in your Flutter app. The `flutter` section is used to specify assets that should be included with the app. The path specified in the `assets` section should be relative to the `pubspec.yaml` file.

# Adding assets and images

# Specifying assets

Flutter uses the `pubspec.yaml` file, located at the root of your project, to identify assets required by an app.

Here is an example:

```yaml
flutter:
  assets:
    - assets/my_icon.png
    - assets/background.png

```

To include all assets under a directory, specify the directory name with the / character at the end:

```yaml
flutter:
  assets:
    - directory/
    - directory/subdirectory/

```

>  **Note:** Only files located directly in the directory are included unless there are files with the same name inside a subdirectory (see Asset Variants). To add files located in subdirectories, create an entry per directory.

## Asset variants

The build process supports the notion of asset variants: different versions of an asset that might be displayed in different contexts. When an asset’s path is specified in the `assets` section of `pubspec.yaml`, the build process looks for any files with the same name in adjacent subdirectories. Such files are then included in the asset bundle along with the specified asset.

For example, if you have the following files in your application directory:

```yaml
.../pubspec.yaml
.../graphics/my_icon.png
.../graphics/background.png
.../graphics/dark/background.png
...etc.
```

And your pubspec.yaml file contains the following:

```yaml
flutter:
  assets:
    - graphics/background.png
```
Then both graphics/background.png and graphics/dark/background.png are included in your asset bundle. The former is considered the *main* asset, while the latter is considered a *variant*.

If, on the other hand, the graphics directory is specified:

```yaml
flutter:
  assets:
    - graphics/

```

Then the graphics/my_icon.png, graphics/background.png and graphics/dark/background.png files are also included.

Flutter uses asset variants when choosing resolution-appropriate images. In the future, this mechanism might be extended to include variants for different locales or regions, reading directions, and so on.

# Loading assets

Your app can access its assets through an AssetBundle object.

The two main methods on an asset bundle allow you to load a string/text asset (`loadString()`) or an image/binary asset (`load()`) out of the bundle, given a logical key. The logical key maps to the path to the asset specified in the `pubspec.yaml` file at build time.

## Loading text assets

Each Flutter app has a `rootBundle` object for easy access to the main asset bundle. It is possible to load assets directly using the `rootBundle` global static from `package:flutter/services.dart`.

However, it’s recommended to obtain the `AssetBundle` for the current `BuildContext` using `DefaultAssetBundle`, rather than the default asset bundle that was built with the app; this approach enables a parent widget to substitute a different `AssetBundle` at run time, which can be useful for localization or testing scenarios.

Typically, you’ll use `DefaultAssetBundle.of()` to indirectly load an asset, for example a JSON file, from the app’s runtime `rootBundle`.

Outside of a `Widget` context, or when a handle to an `AssetBundle` is not available, you can use `rootBundle` to directly load such assets. For example:

```dart
import 'package:flutter/services.dart' show rootBundle;

Future<String> loadAsset() async {
  return await rootBundle.loadString('assets/config.json');
}
```