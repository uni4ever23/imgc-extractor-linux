# Linux Quick Start

## Requirements

- 64-bit x86 Linux (`x86_64`)
- Standard glibc and OpenSSL libraries
- A completed `.imgcv2`, legacy `.imgc`, or decompressed `.img` source
- Enough destination space for the extracted data plus safety margin

The package includes its .NET runtime. It does not require Windows, Wine, Mono, Python, a desktop environment, root access, or a separate .NET installation.

On current Debian or Ubuntu, install OpenSSL if necessary:

```sh
sudo apt-get install libssl3
```

## Install

```sh
tar -xzf imgc-extractor-1.1.0-linux-x64.tar.gz
cd imgc-extractor-linux-x64
chmod +x imgc-extractor
./imgc-extractor --version
```

## Inspect

```sh
./imgc-extractor inspect "/path/to/game.imgcv2"
```

Inspection opens the source read-only, validates IMGCv2, GPT and supported ext filesystems, inventories the merged output, and verifies whether `game.bin` can be prepared. It does not extract files.

## Extract

```sh
./imgc-extractor extract "/path/to/game.imgcv2" "/path/to/new-output-folder"
```

The output argument is positional; do not use `--output`. The final path must not exist, but its parent must exist.

Every copied file is reread and verified with SHA-256. Supported wrapped executables produce `game.bin.encrypted` containing the exact original and `game.bin` containing the decoded ELF.

The completed folder includes `Extraction-report.json` and `Extraction.log`. Failed or cancelled work remains in a uniquely named `.partial-*` folder.

## Destination filesystems

Use a normal writable Linux filesystem such as ext4, XFS or Btrfs, or a capable mounted network/removable filesystem. FAT32 is rejected because it cannot hold large game files.

## Verify the download

```sh
sha256sum -c imgc-extractor-1.1.0-linux-x64.tar.gz.sha256.txt
```
