# General

## Workspace

```bash
bin/ # compiled binaries land here
pkg/ # archives (pre compiled packages)
src/ # my sourcecode
    github.com/
        username/
            projectA
            projectB
```

* GOPATH points to workspace
* GOROOT points to go binary
* GOBIN points to path where binaries will be installed with go install

## Commands

* `go version` - version of go
* `go env` - go env vars
* `go help` - go commands
* `go fmt` - format modified code
* `go run` - build and run code and delete after running it
* `go build` - build code, do not delete binary
* `go install` - build project and place binary in bin/
* `go get` - install packages
* `go mod init example.com/hello` - create a go module file
* `go list -m all` - list all dependencies
* `go get package@v1.1.1` - get package with version 1.1.1

## Modules

* `go mod init`- s.a.
* `go build/test`- add new dep. to go.mod
* `go get`- change or add dep.
* `go mod tidy`- clean up deps.

