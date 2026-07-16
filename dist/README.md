# gumgrok — prebuilt binary

`gumgrok.xz` is the release build of the Grok Build CLI/TUI, renamed to `gumgrok`.

- Platform: Linux x86-64 (ELF, dynamically linked)
- Version: `0.1.220-alpha.4`
- Built with: `cargo build -p xai-grok-pager-bin --release`, then `strip` +
  `xz -9`. (The build target is named `gumgrok` — see
  `crates/codegen/xai-grok-pager-bin/Cargo.toml`.)

It is shipped `xz`-compressed (34 MB) because the uncompressed binary (~155 MB
stripped) exceeds GitHub's 100 MB per-file limit.

## Restore the runnable binary

```sh
xz -d -k gumgrok.xz      # -> gumgrok  (keeps the .xz)
chmod +x gumgrok
./gumgrok --help         # Usage: gumgrok [OPTIONS] [PROMPT] [COMMAND]
```

The executable resolves its own program name from `argv[0]`, so the file must be
named `gumgrok` (or `grok`/`agent`) for the help/usage text to read `gumgrok`.

## Rebuild from source

```sh
cargo build -p xai-grok-pager-bin --release   # -> target/release/gumgrok
```

`protoc` is required for proto codegen (resolve `bin/protoc`, a `protoc` on `PATH`,
or `$PROTOC`).
