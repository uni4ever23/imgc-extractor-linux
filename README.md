# IMGC Extractor for Linux

A native self-contained `linux-x64` Terminal tool for extracting supported Golden Tee disk-image layouts directly from completed HDD Raw Copy Tool `.imgcv2`, legacy `.imgc`, or raw `.img` files.

## Download

Download the latest archive from the [Releases page](https://github.com/uni4ever23/imgc-extractor-linux/releases/latest). The package includes its .NET runtime and does not require Windows, Wine, Mono, Python, or a desktop environment.

## Linux requirement

A normal 64-bit glibc Linux system with OpenSSL is required. On current Debian or Ubuntu:

```sh
sudo apt-get install libssl3
```

## Quick start

```sh
tar -xzf imgc-extractor-1.1.0-linux-x64.tar.gz
cd imgc-extractor-linux-x64
chmod +x imgc-extractor
./imgc-extractor inspect "/path/to/image.imgcv2"
./imgc-extractor extract "/path/to/image.imgcv2" "/path/to/new-output-folder"
```

The output path is positional and must not already exist. The source is opened read-only.

## What it does

- Reads completed IMGCv2 2.0-2.2, legacy IMGC, and raw IMG files without creating a full intermediate image.
- Supports raw, zero-fill, FF-fill, and LZ4 IMGCv2 records with indexed random access.
- Discovers GPT/ext partitions and merges the supported `game_static`, `game_dynamic`, and `db_backup` layout.
- Rereads every copied file and verifies it with SHA-256.
- Verifies supported product-14 `game.bin` wrappers, preserves the original as `game.bin.encrypted`, and publishes the decoded ELF as `game.bin`.
- Refuses incomplete images and existing output folders; failed work remains in a uniquely named partial directory with diagnostics.

## Documentation

- [Linux Quick Start](docs/LINUX-QUICKSTART.md)
- [IMGCv2 Support](docs/IMGCV2-SUPPORT.md)
- [`game.bin` Preparation](docs/GAME-BIN-PREPARATION.md)

## Validation

The packaged x86-64 ELF was executed inside clean Debian Bookworm Linux. Version/help output, IMGCv2 indexing, GPT/ext discovery, complete extraction, collision checks, file-symlink materialization, SHA-256 rereads, Linux-labelled report generation, and verified completion all passed.

## License and acknowledgements

The extraction core is derived from MIT-licensed IMGC Extractor 1.0.2. Legacy IMGC behavior is based on `shiz/unimgc` under the WTFPL. Read-only ext support uses DiscUtils under MIT. IMGCv2 LZ4 decoding uses K4os.Compression.LZ4 under MIT.

No disk images, game payloads, vendor executables, original loader binaries, or private samples are distributed.
