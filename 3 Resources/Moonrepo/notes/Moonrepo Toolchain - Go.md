## 🌙 Moonrepo Toolchain: Go

## 📁 Project Setup

* Create the project folder in the appropriate location:
```bash
# For deployable applications
mkdir apps/<project>

# For shared libraries / packages
mkdir packages/<project>

cd apps/<project>  # or packages/<project>
```

* Initialise a new Go module:
```bash
go mod init github.com/<org>/<project>
```

* Create `moon.yml` in the project folder:
```yaml
language: go
type: application  # or 'library' for packages
```

## 🔧 Toolchain Setup

* Add to `.moon/toolchain.yml`:
```yaml
go:
  version: "1.22.0"
  bins: []
```

* Add to `.prototools`:
```toml
go = "1.22.0"
```

* Add tasks to project `moon.yml`:
```yaml
language: go
type: application  # or 'library' for packages
tasks:
  dev:
    command: go run ./cmd/main.go
  build:
    command: go build -o dist/app ./cmd/main.go
  test:
    command: go test ./...
  lint:
    command: go vet ./...
```

## 📝 Notes

* Go version must be pinned in both `toolchain.yml` and `.prototools`
* Swap `./cmd/main.go` for your actual entrypoint
* For shared Go libraries, omit the `dev` and `start` tasks