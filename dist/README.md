# gumgrok — prebuilt binaries

Release builds of the Grok Build CLI/TUI, renamed to `gumgrok` (build target in
`crates/codegen/xai-grok-pager-bin/Cargo.toml`). Version `0.1.220-alpha.4`.

| File | Platform | Produced by |
|------|----------|-------------|
| `gumgrok.xz` | Linux x86-64 (ELF) | local `cargo build -p xai-grok-pager-bin --release`, then `strip` + `xz -9` |
| `gumgrok-aarch64-apple-darwin.xz` | macOS Apple Silicon (Mach-O, arm64) | native build on a `macos-14` GitHub Actions runner (`.github/workflows/build-macos.yml`), then `strip` + `xz -9` |

Both are shipped `xz`-compressed because the uncompressed binaries (~150+ MB)
exceed GitHub's 100 MB per-file limit. The macOS binary is built natively on a
macOS runner rather than cross-compiled — cross-compiling from Linux is blocked by
`coreaudio-sys` (needs macOS SDK headers), Apple-framework links, and `aws-lc-sys`.

## Restore a runnable binary

```sh
# Linux
xz -d -k gumgrok.xz && chmod +x gumgrok && ./gumgrok --help

# macOS (Apple Silicon)
xz -d -k gumgrok-aarch64-apple-darwin.xz
mv gumgrok-aarch64-apple-darwin gumgrok
chmod +x gumgrok
./gumgrok --help          # Usage: gumgrok [OPTIONS] [PROMPT] [COMMAND]
```

The executable resolves its own program name from `argv[0]`, so the file must be
named `gumgrok` (or `grok`/`agent`) for the help/usage text to read `gumgrok`.

## Rebuild from source

```sh
cargo build -p xai-grok-pager-bin --release   # -> target/release/gumgrok
```

`protoc` is required for proto codegen (resolve `bin/protoc`, a `protoc` on `PATH`,
or `$PROTOC`).
