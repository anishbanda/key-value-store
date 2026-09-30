# cpp-kv-store

A key-value store server written in modern C++ (C++20), built with CMake and
run inside Docker so no compiler or libraries need to be installed locally.

## What's included

- `src/` — server source code (networking, storage, protocol parsing/serialization)
- `include/kvstore/` — public headers
- `tests/` — unit tests (GoogleTest)
- `cmake/` — CMake helper modules (warnings, sanitizers)
- `scripts/` — helper scripts for building, testing, and working inside Docker
- `Dockerfile` / `compose.yaml` — development container definition

## Requirements

- Git
- Docker Desktop

The C++ compiler, CMake, GoogleTest, and other build tools all live inside
the Docker container, not on your host machine.

## Setup

1. Clone the repository and `cd` into it.
2. Check your host is ready:
   ```
   scripts/check-host.sh
   ```
3. Build the dev container (one-time, or after `Dockerfile` changes):
   ```
   docker compose build dev
   ```
4. Open a shell inside the container:
   ```
   scripts/shell.sh
   ```
5. From inside that shell, build the project:
   ```
   scripts/build.sh
   ```
6. Run the tests:
   ```
   scripts/test.sh
   ```

Both `build.sh` and `test.sh` accept an optional preset: `debug` (default),
`release`, `asan`, or `tsan`.

## Running the server

After building, the binary is at `./build/<preset>/kvstore_server`, e.g.:

```
./build/debug/kvstore_server
```

See `instructions.md` for a more detailed walkthrough.
