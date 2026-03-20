### Debug executable in GoLand

- [Delve](https://github.com/go-delve/delve?tab=readme-ov-file) is a debugger for Go.
- [Installation](https://github.com/go-delve/delve/tree/master/Documentation/installation) instructions.

Example usage:

```bash
dlv debug ./main.go --headless --listen=:2345 --api-version=2 --accept-multiclient -- <app args>
```

It starts the debugger in headless mode and listens on port 2345.

In GoLand, add a new Go Remote configuration and set the host to localhost and port to 2345 (should be the defaults).

You run it by either hitting Debug in GoLand first and then executing this dlv command,  
or by running this command in the terminal and then hitting Debug in GoLand after.

Either way, it'll stop on breakpoints you set inside GoLand.

Bash shortcut:

```bash
godebug() {
  # $1 is the path to the Go project: "./main.go" or "./cmd/server/main.go" etc.
  # ${@:2} are the arguments to the Go program. This is essentially "all args except the first".
  dlv debug "$1" --headless --listen=:2345 --api-version=2 --accept-multiclient -- "${@:2}"
}
```

### Debug tests run from CLI in GoLand

Instead of `dlv debug` which will compile the program into an executable and only run `main()`, we want to run `dlv test` when debugging tests.

```bash
dlv test ./main.go --headless --listen=:2345 --api-version=2 --accept-multiclient -- <go test args>
```

The procedure in GoLand is the same.

Bash shortcut:

```bash
godebugtest() {
  # $1 is the path to the Go tests: "./..." etc.
  # ${@:2} are the arguments to the Go program. This is essentially "all args except the first".
  dlv test "$1" --headless --listen=:2345 --api-version=2 --accept-multiclient -- "${@:2}"
}
```
