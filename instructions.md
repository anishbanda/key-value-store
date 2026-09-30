# How to Use This Project

This is a key-value store server written in C++, built with CMake, and run
inside Docker so you don't have to install a compiler or any libraries on
your own computer.

## 1. What you need installed

Just two things on your Mac:

- **Git** — you probably already have it.
- **Docker Desktop** — download from docker.com and open it once so it's
  running in the background.

Everything else (the C++ compiler, CMake, test tools, etc.) lives inside a
Docker container, not on your computer.

To check you have both set up correctly, run:

```
scripts/check-host.sh
```

It will print `OK` or `MISS` next to each requirement.

## 2. Build the container (one-time setup)

```
docker compose build dev
```

This downloads Ubuntu and installs all the C++ build tools inside a
container image called `dev`. You only need to redo this if the
`Dockerfile` changes.

## 3. Open a shell inside the container

```
scripts/shell.sh
```

This drops you into a bash shell *inside* the container, with the project
folder mounted at `/workspace`. Any changes you make to files on your Mac
show up instantly inside the container, and vice versa. Run all the
commands below from inside this shell.

## 4. Build the project

```
scripts/build.sh
```

This compiles the code. By default it builds the "debug" version. You can
also run:

```
scripts/build.sh release   # optimized build
scripts/build.sh asan      # debug build with AddressSanitizer/UBSan (catches memory bugs)
scripts/build.sh tsan      # debug build with ThreadSanitizer (catches data races)
```

## 5. Run the tests

```
scripts/test.sh
```

This builds the project and then runs the full test suite. It accepts the
same preset names as `scripts/build.sh` (e.g. `scripts/test.sh asan`).

## 6. Run the server

After building, the server binary lives in the build folder for whichever
preset you used, e.g.:

```
./build/debug/kvstore_server
```

Right now it's an early scaffold, so it just prints a message and exits —
the actual key-value server logic (accepting connections, storing data)
is still being built out.

You can check the version with:

```
./build/debug/kvstore_server --version
```

## Typical day-to-day workflow

1. `scripts/shell.sh` — enter the container (once per terminal session).
2. Edit code on your Mac using your normal editor.
3. `scripts/test.sh` — rebuild and run tests after each change.
4. When you're done, just close the shell or press Ctrl+D.

## Quick reference

| Command | What it does |
|---|---|
| `scripts/check-host.sh` | Checks your Mac has Docker/Git ready |
| `docker compose build dev` | Builds the container image (one-time / after Dockerfile changes) |
| `scripts/shell.sh` | Opens a shell inside the container |
| `scripts/build.sh [preset]` | Compiles the code |
| `scripts/test.sh [preset]` | Compiles and runs the tests |

Presets: `debug` (default), `release`, `asan`, `tsan`.
