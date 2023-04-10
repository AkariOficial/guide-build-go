# Building Packages Golang for architecture and system.
> A README.MD to help people with `go build` and `GOOS`/ `GOARCH`

#### Possible Platforms for `GOOS` and `GOARCH`
> Before showing how to control the build process to build binaries for different platforms, let’s first inspect what kinds of platforms Go is capable of building for, and how Go references these platforms using the environment variables `GOOS` and `GOARCH`.

> The Go tooling has a command that can print a list of the possible platforms that Go can build on. This list can change with each new Go release, so the combinations discussed here might not be the same on another version of Go. At the time of writing this tutorial, the current Go release is 1.13.

#### To find this list of possible platforms, run the following:
> **Output**
> ```
> go tool dist list
> ```

#### You will receive an output similar to the following:
> **Output**
> ```
> aix/ppc64
> android/386
> android/amd64
> android/arm
> android/arm64
> darwin/amd64
> darwin/arm64
> dragonfly/amd64
> freebsd/386
> freebsd/amd64
> freebsd/arm
> freebsd/arm64
> freebsd/riscv64
> illumos/amd64
> ios/amd64
> ios/arm64
> js/wasm
> linux/386
> linux/amd64
> linux/arm
> linux/arm64
> linux/loong64
> linux/mips
> linux/mips64
> linux/mips64le
> linux/mipsle
> linux/ppc64
> linux/ppc64le
> linux/riscv64
> linux/s390x
> netbsd/386
> netbsd/amd64
> netbsd/arm
> netbsd/arm64
> openbsd/386
> openbsd/amd64
> openbsd/arm
> openbsd/arm64
> openbsd/mips64
> plan9/386
> plan9/amd64
> plan9/arm
> solaris/amd64
> windows/386
> windows/amd64
> windows/arm
> windows/arm64
> ```

#### When you run a command like go build, Go uses the current platform’s `GOOS` and `GOARCH` to determine how to build the binary. To find out what combination your platform is, you can use the go env command and pass GOOS and GOARCH as arguments:
> ```
> go env GOOS GOARCH
> ```
#### In testing this example, we ran this command on Linux/Android on a machine with an ARM64 architecture, so we will receive the following output:
> ```
> go env GOOS GOARCH
> ```

> **Output**
> ```
> linux
> arm64
> ```
> Here the output of the command tells us that our system has `GOOS=linux` and `GOARCH=arm64`.

#### Using Your Local `GOOS` and `GOARCH` Environment Variables

> Earlier, you ran the `go env GOOS GOARCH` command to find out what OS and architecture you were working on. When you ran the `go env` command, it looked for the two environment variables `GOOS` and `GOARCH`; if found, their values would be used, but if not found, then Go would set them with the information for the current platform. This means that you can change `GOOS` or `GOARCH` so that they do not default to your local OS and architecture. </br><br> The go build command behaves in a similar manner to the go env command. You can set either the `GOOS` or `GOARCH` environment variables to build for a different platform using go build.

-------

#### If you are not using a Windows system, build a `windows` binary of app by setting the `GOOS` environment variable to windows when running the `go build` command:
> **Output**
> ```
> GOOS=windows \
>       go build -o builds/windows/example_for_windows
> ```

#### Another example with windows specifying the `arm64` architecture command:
> **Output**
> ```
> GOOS=windows \
>       GOARCH=arm64 \
>       go build -o builds/windows/arm64/example_for_windows_arm64
> ```

#### Now list the files in your current directory:
> **Output**
> ```
> tree # or ls -la
> ```

#### The output of listing the directory shows there is now an app.exe Windows executable in the project directory:
> **Output**
> ```
> tree builds/windows/
> builds/windows/
> └── calculate_windows
>
> 1 directory, 1 file
> ```
> Note: perhaps when using <br> `build -o` you should specify the `.exe` extension in case it doesn't work when in a **windows** environment. </br> </br> **Example:**
> ```
> GOOS=windows \
>       GOARCH=arm64 \
>       go build -o builds/windows/arm64/windows_arm64.exe
> ```

-------

#### Example with `linux` using `GOARCH` and `GOOS`  specified:
> **Output**
> ```
> GOOS=linux \
>       GOARCH=arm64 \
>       go build -o builds/linux/arm64/example_for_linux_arm64
> ```

#### Example with `android` using `GOARCH` and `GOOS`  specified:
> **Output**
> ```
> GOOS=android \
>       GOARCH=arm64 \
>       go build -o builds/android/arm64/example_for_android_arm64
> ```

-------

#### Conclusion
> The ability to generate binaries for multiple platforms that require no dependencies is a powerful feature of the Go toolchain. In this tutorial, you used this capability by adding build tags and filename suffixes to mark certain code snippets to only compile for certain architectures. You created your own platorm-dependent program, then manipulated the `GOOS` and `GOARCH` environment variables to generate binaries for platforms beyond your current platform. This is a valuable skill, because it is a common practice to have a continuous integration process that automatically runs through these environment variables to build binaries for all platforms.
