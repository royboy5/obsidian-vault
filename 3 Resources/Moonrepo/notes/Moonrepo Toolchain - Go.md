## 🌙 Moonrepo Toolchain: Go

Not the JS default path. `layer` / `language` / `stack` are moon v2 project fields ([project config](https://moonrepo.dev/docs/config/project), 2026-09-11).

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
layer: 'application'
language: 'go'
stack: 'backend'
```

## 🔧 Toolchain Setup

* Add to `.moon/toolchains.yml`:
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
layer: 'application'
language: 'go'
stack: 'backend'
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

* Go version must be pinned in both `.moon/toolchains.yml` and `.prototools`
* Swap `./cmd/main.go` for your actual entrypoint
* For shared Go libraries, omit the `dev` and `start` tasks