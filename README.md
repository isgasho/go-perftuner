# go-perftuner

Helper tool for manual Go code optimization.

This tool gives you an easy way to get the Go compiler output regarding specific optimisations. Like: function inlining, variable escape and bounds checks.

# Installation / Quick Start

```bash
# Install go-perftuner:
$ go get -u github.com/cristaloleg/go-perftuner

# Check installation (prints help):
$ go-perftuner

# Run almostInlined sub-command on strings and bytes package:
$ go-perftuner almostInlined strings bytes

# You can use "std" or "..." package name.
# These follow "go build" conventions.
$ go-perftuner almostInlined std
```

# Sub-commands

## almostInlined

Find functions that cross inlining threshold just barely.

```bash
$ go-perftuner almostInlined -threshold=1 std
almostInlined: std: src/strconv/atof.go:371:6: atof64exact: budget exceeded by 1
almostInlined: std: src/strconv/atof.go:405:6: atof32exact: budget exceeded by 1
almostInlined: std: src/reflect/value.go:1199:6: Value.OverflowComplex: budget exceeded by 1
almostInlined: std: src/vendor/golang_org/x/crypto/cryptobyte/builder.go:77:6: (*Builder).AddUint16: budget exceeded by 1
almostInlined: std: src/crypto/x509/x509.go:1858:58: buildExtensions.func2.1.1: budget exceeded by 1
almostInlined: std: src/crypto/x509/x509.go:1878:58: buildExtensions.func2.3.1: budget exceeded by 1
almostInlined: std: src/crypto/x509/x509.go:1890:58: buildExtensions.func2.4.1: budget exceeded by 1
almostInlined: std: src/crypto/tls/handshake_messages.go:1450:6: (*newSessionTicketMsg).marshal: budget exceeded by 1
almostInlined: std: src/net/http/transfer.go:259:6: (*transferWriter).shouldSendContentLength: budget exceeded by 1
```

## escapedVariables

Find variables that are escaped to the heap.

```bash
$ go-perftuner escapedVariables fmt
escapedVariables: fmt: src/fmt/format.go:73:13: make(buffer, cap(buf) * 2 + n)
escapedVariables: fmt: src/fmt/format.go:147:14: make([]byte, width)
escapedVariables: fmt: src/fmt/format.go:208:14: make([]byte, width)
```

## boundChecks

Find slice/array that has bound check.

```bash
$ go-perftuner boundChecks fmt
boundChecks: fmt: src/fmt/format.go:82:16: slice/array has bound checks
boundChecks: fmt: src/fmt/format.go:157:10: slice/array has bound checks
boundChecks: fmt: src/fmt/format.go:159:22: slice/array has bound checks
boundChecks: fmt: src/fmt/format.go:161:10: slice/array has bound checks
```

# Notes

The original implementation was started by [@quasilyte](https://github.com/quasilyte) thanks to him :tada:
