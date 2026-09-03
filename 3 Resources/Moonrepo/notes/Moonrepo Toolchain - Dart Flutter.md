## 🌙 Moonrepo Toolchain: Dart / Flutter

## 📁 Project Setup

* Create the project folder in the appropriate location:
```bash
# For deployable applications
mkdir apps/<project>

# For shared libraries / packages
mkdir packages/<project>

cd apps/<project>  # or packages/<project>
```

* Initialise a new Flutter project:
```bash
# For applications
flutter create .

# For shared libraries / packages
flutter create --template=package .
```

* Create `moon.yml` in the project folder:
```yaml
language: other
type: application  # or 'library' for packages
```

## 🔧 Toolchain Setup

* Add to `.prototools` at the workspace root with latest as a starting point:
```toml
flutter = "latest"
```

* Install Flutter via proto:
```bash
proto install flutter
```

* Pin the installed version:
```bash
proto pin flutter
```

* Verify `.prototools` now shows a pinned version e.g.:
```toml
flutter = "3.44.0"
```

* Add tasks to project `moon.yml`:
```yaml
language: other
type: application  # or 'library' for packages
platform: system
toolchain:
  default: system
tasks:
  dev:
    command: flutter run
  build:
    command: flutter build apk
  test:
    command: flutter test
  lint:
    command: flutter analyze
  format:
    command: dart format .
```

## 📝 Notes

* Moon has no native Dart/Flutter toolchain support — proto handles the install instead
* Dart comes bundled with Flutter via proto — no need to pin it separately
* Flutter build target (`apk`, `ios`, `web`) will vary per project — update accordingly
* For shared Flutter packages, omit the `dev` and `build` tasks