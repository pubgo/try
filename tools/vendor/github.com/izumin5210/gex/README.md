# gex

[![CI](https://github.com/izumin5210/gex/workflows/CI/badge.svg)](https://github.com/izumin5210/gex/actions?workflow=CI)
[![GoDoc](https://godoc.org/github.com/izumin5210/gex?status.svg)](https://godoc.org/github.com/izumin5210/gex)
[![GitHub release (latest SemVer)](https://img.shields.io/github/v/release/izumin5210/gex)](https://github.com/izumin5210/gex/releases/latest)
[![Go Report Card](https://goreportcard.com/badge/github.com/izumin5210/gex)](https://goreportcard.com/report/github.com/izumin5210/gex)
[![License](https://img.shields.io/github/license/izumin5210/gex.svg)](./LICENSE)

The implementation of clarify best practice for tool dependencies.

See https://github.com/golang/go/issues/25922#issuecomment-412992431


## Features

- Manage versions of tools dependencies, and build them with specified version
- **Does not introduce new mechanisms** to manage tool dependencies
- **Only 2 commands** that you use: `--add` and `--build`
- All you need to **execute `go generate ./tools.go`** if you want only to use tools


## Usage

### `gex --add [packages...]`
Add a new tool to dependencies:

```
$ gex --add github.com/golang/mock/mockgen
```

The tool will be managed in `tools.go` and its version will be managed by [Modules](https://github.com/golang/go/wiki/Modules) or [dep](https://golang.github.io/dep/).

```
$ cat tools.go
// Code generated by github.com/izumin5210/gex. DO NOT EDIT.

// +build tools

package tools

// tool dependencies
import (
        _ "github.com/golang/mock/mockgen"
)

// If you want to use tools, please run the following command:
//  go generate ./tools.go
//
//go:generate go build -v -o=./bin/reviewdog github.com/golang/mock/mockgen

$ cat go.mod | grep mock
        github.com/golang/mock v1.1.1 // indirect
```


### `go generate ./tools.go`
Build executable binaries into `$PWD/bin`.

```
$ go generate ./tools.go
```


### `gex [command] [args...]`
Execute command that managed in `tools.go` and `go.mod`.
`gex` will build the executable binary automatically if needed.

```
$ gex mockgen
# prints mockgen's help text...
```


## Installation

### macOS

```console
$ brew install izumin5210/tools/gex
```

### Other platforms

You can download prebuilt binaries for each platform in the [releases](https://github.com/izumin5210/gex/releases) page.

### Build from source

```console
$ go get github.com/izumin5210/gex/cmd/gex
```


## Requirements

gex depends on [dep](https://golang.github.io/dep/) or [Modules](https://github.com/golang/go/wiki/Modules) to manage tool dependencies,