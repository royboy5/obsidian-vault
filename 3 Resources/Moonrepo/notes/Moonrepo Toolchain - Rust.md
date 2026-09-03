## 🌙 Moonrepo Toolchain: Rust

[Moon Rust Docs](https://moonrepo.dev/docs/guides/rust/handbook)

## 📁 Project Setup

* Create the project folder in the appropriate location:
```bash
# For deployable applications
mkdir apps/<project>

# For shared libraries / packages
mkdir packages/<project>

cd apps/<project>  # or packages/<project>
```

* Initialise a new Rust project:
```bash
# For applications
cargo init

# For libraries
cargo init --lib
```

* Create `moon.yml` in the project folder:
```yaml
language: rust
type: application  # or 'library' for packages
```

## 🔧 Toolchain Setup

* Add to `.moon/toolchain.yml`:
```yaml
rust:
  version: "1.78.0"
  components: [clippy, rustfmt]
  edition: "2021"
```

* Add tasks to project `moon.yml`:
```yaml
language: rust
type: application  # or 'library' for packages
tasks:
  build:
    command: cargo build
  test:
    command: cargo test
  lint:
    command: cargo clippy
```

## 📝 Notes

* Rust is managed natively by moon's toolchain — no need to pin separately in `.prototools`
* `clippy` and `rustfmt` are installed automatically via the `components` list
* Use `cargo init --lib` for shared Rust packages consumed by other projects