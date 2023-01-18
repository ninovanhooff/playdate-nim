# Nim Playdate
![Playdate Simulator 2023-01-06 at 19 41 01](https://user-images.githubusercontent.com/19392104/211077589-09d1c9ee-02a4-4804-8c2b-6a8ad1850ec3.png)

## About
Playdate Nim bindings, aiming to provide:
- C comparable performance
- Concise and easy syntax
- More ergonomic API over the C API
- Automatic memory management

Here's a quick comparison between the main languages usable on the Playdate:
Language | C | Lua | Nim  |
:---- | ---- | ---- | ----
**Performance** | ✔ Great | Decent | ✔ Great
**Memory Management** | ✖ No management | ✔ Garbage collected | ✔ Reference counted
**Memory usage** | ✔ Lowest | Acceptable | ✔ Low
**Typing** | Static | Dynamic | Static
**API** | Low level | High level | Mixed
**Syntax** | Quite easy | ✔ Easiest & concise | ✔ Easy & concise
**Error checking** | Basic compile + runtime | ✖ Mostly runtime | ✔ Compile time + runtime
**OOP** | ✖ Not supported | ✔ Supported | ✔ Supported

## Getting Started

At the moment, this is just a template that contains the required code to wrap and use the Playdate SDK in Nim.

The bindings will become a Nim package when stable enough.

### Prerequisites

- Playdate SDK
- Nim 1.6.10+
- `PLAYDATE_SDK_PATH` environment variable

## Usage

`src/main.nim` contains a basic example of the bindings utilization.

Here's also a minimal snippet to make a Nim application:
```nim
import playdate/api

var nimLogoBitmap: LCDBitmap

proc update(): int {.raises: [].} =
    nimLogoBitmap.draw(168, 88, kBitmapUnflipped)

# This is the application entrypoint and event handler
proc handler(event: PDSystemEvent, keycode: uint) {.raises: [].} =
    if event == kEventInit:
        # Errors are handled through exceptions, this is an inline try/except
        nimLogoBitmap = try: playdate.graphics.newBitmap("/images/nim_logo") except: nil
        # playdate is the global PlaydateAPI instance, available when playdate/api is imported 
        playdate.system.setUpdateCallback(update)

# Used to setup the SDK entrypoint
initSDK()
```

A pre-compiled pdx is also provided, please test it on your device!

Compile the project (pdx) for the simulator using:
```sh
nim simulator
```
For the device (compile only, no pdx):
```sh
nim device
```
For simulator + device (pdx):
```sh
nim all
```

The template also has a VSCode launch configuration file to build, start and debug the Nim application from the editor.

---
This is still a work in progress, here's what is still missing right now:
- various playdate.sound funcionalities (but FilePlayer and SamplePlayer are available)
- playdate.json, but you can use Nim std/json, which is very convenient
- playdate.lua, interfacing with Lua and providing classes/functions
- playdate.scoreboards, undocumented even in the official C API docs
