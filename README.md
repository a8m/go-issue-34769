Running `go build` from `foo` directory returns:

```
build foo: cannot load bar: malformed module path "bar": missing dot in first path element
```

This error is returned because `bar` directory does not contain any Go file.

[CL 203118](https://go-review.googlesource.com/c/go/+/203118) change the module importer to print more friendly error. For example:
```
main.go:3:8: module bar@ found (v0.0.0-00010101000000-000000000000, replaced by ../bar), but does not contain package
```

If you ask yourself why creating a module without any Go files, well, we tried to build a few Docker images and accidentally forgot to copy some of the Go files to the image. It took us hours to figure out what the "missing dot in first path element" means in this case.
