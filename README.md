# GitHub Action for govulncheck

This repository holds the GitHub Action for govulncheck.

[Govulncheck](https://pkg.go.dev/golang.org/x/vuln/cmd/govulncheck) provides a
low-noise, reliable way for Go users to learn about known vulnerabilities that
may affect their dependencies. See details on [Go's support for vulnerability
management](https://go.dev/blog/vuln).

The govulncheck GitHub Action is currently experimental and is under active
development.

## Using the govulncheck GitHub Action

To use the govulncheck GitHub Action add the following step to your workflow:

```yaml
- id: govulncheck
  uses: golang/govulncheck-action@v1
```

By default the govulncheck Github Action will run with the
[latest version of Go](https://go.dev/doc/install) and analyze all packages in
the provided Go module. Assuming you have the latest Go version installed
locally, this is equivalent to running the following on your command line:

```
$ govulncheck ./...
```

To specify a specific Go version or
[package pattern](https://pkg.go.dev/cmd/go#hdr-Package_lists_and_patterns),
use the following syntax:

```yaml
- id: govulncheck
  uses: golang/govulncheck-action@v1
  with:
     go-version-input: <your-Go-version>
     go-package: <your-package-pattern>
```

To use `go.mod` or `go.work` to specify the version, use the following syntax ([further reading](https://github.com/actions/setup-go#getting-go-version-from-the-gomod-file)):
```yaml
- id: govulncheck
  uses: golang/govulncheck-action@v1
  with:
     go-version-file: <your-go-mod-or-work> # 'go.mod' or 'go.work'
     go-package: <your-package-pattern>
```

For example, the code snippet below can be used to run govulncheck against a
repository on every push:

```yaml
on: [push]

jobs:
  govulncheck_job:
    runs-on: ubuntu-latest
    name: Run govulncheck
    steps:
      - id: govulncheck
        uses: golang/govulncheck-action@v1
        with:
           go-version-input: 1.20.6
           go-package: ./...
```
When a vulnerability is found, an error will be displayed for that
[GitHub job](https://docs.github.com/en/actions/using-jobs/using-jobs-in-a-workflow)
with information about the vulnerability and how to fix it. For example:

![image](https://github.com/bkessler-go/prototype-repo/assets/107496148/932a2e5c-730e-4583-90f3-edab3ca06f60)

## Contributing

Our canonical Git repository is located at
https://go.googlesource.com/govulncheck-action. There is a mirror of the
repository at https://github.com/golang/govulncheck-action. See
https://go.dev/doc/contribute.html for details on how to contribute.

## Feedback

The main issue tracker for the time repository is located at

If you want to report a bug or have a feature suggestion, please file an issue
at https://github.com/golang/go/issues, prefixed with `govulncheck-action:` in the title.

## License

Unless otherwise noted, the Go source files are distributed under the BSD-style
license found in the [LICENSE](LICENSE) file.
