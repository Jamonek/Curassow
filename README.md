# Curassow

Curassow is a Swift Nest HTTP Server. It uses the pre-fork worker model and
it's similar to Python's Gunicorn and Ruby's Unicorn.

It exposes a Nest compatible interface for your application, allowing you to
use Currasow with any Nest compatible web frameworks of your choice.

## Usage

To use Currasow, you will need to install it via the Swift Package Manager,
you can add it to the list of dependencies in your `Package.swift`:

```swift
import PackageDescription

let package = Package(
  name: "HelloWorld",
  dependencies: [
    .Package(url: "https://github.com/kylef/Currasow.git", majorVersion: 0, minor: 1),
  ]
)
```

Afterwards you can place your web application implementation in `Sources`
and add the runner inside `main.swift` which exposes a command line tool to
run your web application:

```swift
import Curassow
import Inquiline


serve { request in
  return Response(.Ok, contentType: "plain/text", body: "Hello World")
}
```

```shell
$ swift build --configuration release
```

```shell
$ ./.build/release/HelloWorld --workers 3
[arbiter] Listening on port 0.0.0.0:8000
[arbiter] Started worker process 18405
[arbiter] Started worker process 18406
[arbiter] Started worker process 18407
```

### FAQ

#### What platforms does Currasow support?

Currasow is built for Linux. Unfortunately OS X's version of Swift doesn't
expose the underlying `fork()` API so it's not possible to implement a
pre-forking HTTP server for OS X at the moment.

This makes Currasow Linux only, however since it's built around the Nest
specification, you can mix and match any server combination. You can use
another Nest compatible web server on OS X for development, and the high
performance pre-forking server on Currasow on Linux.

## License

Currasow is licensed under the BSD license. See [LICENSE](LICENSE) for more
info.