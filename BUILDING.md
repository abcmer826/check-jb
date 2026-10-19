# Building

Check-JB is an iOS app (ObjC++). The build needs an Apple toolchain: `clang++`,
an iPhoneOS SDK, and `ldid` for the code signature.

## macOS (recommended)

```bash
brew install ldid
export SDKROOT="$(xcrun --sdk iphoneos --show-sdk-path)"
export TARGET="arm64-apple-ios15.0"
./build          # -> p.ipa
```

## Theos / Linux

```bash
export THEOS_SDK_ROOT=/path/to/iPhoneOS.sdk
export TARGET=arm64-apple-ios15.0
./build
```

## CI

`.github/workflows/build.yml` builds on `macos-14` on every push to `main` and
on manual dispatch. It uploads `p.ipa` as an artifact and attaches it to a
`build-<timestamp>` release.

## Notes

- `libs/libxmz` is vendored in-tree (upstream submodule `xmz-team/libxmz`
  @ `b74a164`), so no `--recurse-submodules` clone is required.
- `build` honours `CXX`, `TARGET`, `STD` (default `gnu++17`, needed for
  `std::filesystem` in `libxmz/fs.hpp`), `EXTRA_CXXFLAGS` and `LDID`.
- The binary is signed ad-hoc with `ens.xml` entitlements (`ldid -S`), which is
  what makes it runnable on a jailbroken device. It is not App Store / TestFlight
  signed.
