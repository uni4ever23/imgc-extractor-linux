# `game.bin` Preparation

The extractor treats disk-image decoding and executable preparation as separate layers. IMGCv2 is a block container; supported `game.bin` files inside `game_dynamic` may use an additional product-specific wrapper.

## Discovery

The dynamic ext filesystem is selected by `game_dynamic`. If absent, exactly one unlabelled ext partition must contain `game.bin`, `db_primary.tar.gz`, `golf/` and `lib/`. Ambiguity is rejected.

## Supported forms

- Existing ELF32 little-endian i386: copied without decryption
- Product-14 wrapper revision 2 or 3, at most 128 MiB: decoded and verified
- Others: extracted unchanged and reported unsupported

## Decode and integrity

For supported wrappers, a 256-bit payload key is derived from container fields. SHA-1 and RC4 decode wrapped key material, the per-chunk byte permutation is reversed, and the body is decrypted with AES-256 ECB without padding.

Accepted only if:

- ELF32 i386 structure checks pass
- verification header matches
- internal SHA-1 matches
- embedded DSA-style consistency check passes
- SHA-256 before/after write matches

Signature is container consistency, not publisher authentication/runtime guarantee.

## Preservation

Exact wrapped input becomes `game.bin.encrypted`; disk-verified decoded executable becomes `game.bin`. Existing conflicting backups stop extraction.

No original loader binary, disk image, game payload, or secret external key is included.
