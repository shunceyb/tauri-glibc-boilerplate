# Tauri boilerplate/example to link against older glibc versions

## *If you're having any issues, please open an issue.*

## To-do: instructions for 32-bit ARM / ARMv7 (it's really similar, though)

## Did you come from [Cross-Compiling Tauri Applications for ARM-based Devices](https://v1.tauri.app/v1/guides/building/linux/#cross-compiling-tauri-applications-for-arm-based-devices)?

Have you found yourself successfully cross-compiling a Tauri App for another system/architecture (aarch64-linux-gnu / raspberry pi in my case), just to find out that it won't launch because it is linked against a newer glibc version? I was in that situation as well.

In this repo, we are using zig(zig cc), to link our app against an older glibc version. 

**This repo uses bun, but you can use any javascript runtime/package manager as you like.**

## **It is assumed you are on a Debian-based distro**

## For initial cross-compiling setup, follow the instructions on Tauri's Guide.
## [Tauri V1](https://v1.tauri.app/v1/guides/building/linux/), do all the steps.
### For cross-compiling on Tauri V2, the process is the same, aside from a newer library.

```diff
- sudo apt install libwebkit2gtk-4.0-dev:arm64
+ sudo apt install libwebkit2gtk-4.1-dev:arm64
```

---

### [Install Zig](https://ziglang.org/learn/getting-started/), and add it to your PATH.

### Set your desired glibc version at `.cargo/config.toml` at the root of this repo. List available glibc versions with `zig targets` and look for the glibc array. If you want to target `2.30` for example, you can change the example as shown.

```diff
rustflags = [
-  "-Clink-arg=-target aarch64-linux-gnu.2.35", 
+  "-Clink-arg=-target aarch64-linux-gnu.2.30", 
]
```

### Source the cc\.sh file at the root of the repo. (you should be in the project's directory)
```sh
source cc.sh
```

**What does this do?**
It is setting environment variables for pkg-config, as mentioned on the website. (change **PKG_CONFIG_PATH** accordingly, depending on your target. as for the **PKG_CONFIG_SYSROOT_DIR**, I've set it to **/** instead of the one mentioned in the tutorial as it was prepending it to the library search paths.) This also adds the zig cc wrapper to path, so that config.toml can find it. (This is a workaround, as I couldn't get it to find the wrapper with relative paths.)

## Make sure you have pkg-config installed (I'm not sure if it's pre-installed)
```sh
sudo apt install pkg-config
```

---

## Now, compiling.

### After all that, it should just compile, and the output should be linked against an older glibc version successfully. (Change to your target arch)

```sh
cargo tauri build --target aarch64-unknown-linux-gnu
```

---

# How do I use this in my project?

### There's nothing much in this boilerplate, actually, just steal `.cargo/config.toml`, `wrapper`, `cc.sh`, and follow all the steps above. This is a very long write-up for a very simple process, because I wanted to make it as clear as possible.
