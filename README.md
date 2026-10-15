# Side Impactor

Browser-based IPA signing and installation. Pair an iOS device over WebUSB, sign with your Apple Developer account, and install — all from a single web page.

## Quick Start

Clone this repository to your local machine:
```bash
git clone --recurse-submodules https://github.com/lbr77/SideImpactor
```
If you already cloned it, simply update submodules instead:
```bash
git submodule update --init --recursive
```
Finally,
```bash
bun install --ignore-scripts
bun run dev
```

Open `http://localhost:5173`.

## Project Structure

| Path | Purpose |
|---|---|
| `frontend/` | React + Tailwind SPA (Vite) |
| `backend/` | Cloudflare Workers relay |
| `dependencies/webmuxd/` | WebUSB usbmux/lockdown protocol library |
| `wasm/openssl/` | OpenSSL WASM (TLS + pair record generation) |
| `wasm/libcurl-wasm/` | libcurl WASM (Apple API HTTP via WISP proxy) |
| `wasm/zsign-wasm/` | zsign WASM (IPA re-signing) |
| `scripts/` | WASM native build scripts (Rust + Emscripten) |

## Build

```bash
# WASM dist bundles (copies pre-built src→dist, no compiler needed)
bun run build:wasm:dist

# Frontend production build (runs wasm:dist automatically)
bun run build:frontend

# Full WASM recompile from source (requires Rust, Emscripten, precompiled OpenSSL)
bun run build:wasm
```

## Docker

```bash
bun run build:wasm:dist   # ensure WASM dists exist
docker build -t sideload-web .
docker run -p 3000:3000 sideload-web
```

## Cloudflare

```bash
cd backend
bun run deploy
```

## Test

```bash
bun run test            # webmuxd unit tests
bun run test:frontend   # frontend vitest suite (141 tests)
```

## Credits

Projects referenced:
- [libimobiledevice](https://github.com/libimobiledevice/libimobiledevice)
- [webmuxd](https://github.com/hack-different/webmuxd)
- [zsign](https://github.com/nicehash/zsign)
- [AltSign](https://github.com/rileytestut/AltSign/)
- [AltStore](https://github.com/altstoreio/AltStore)
- [openssl-wasm](https://github.com/nicehash/openssl-wasm)
- [anisette-js](https://github.com/lbr77/anisette-js)
- [unicorn](https://github.com/lbr77/unicorn/tree/tci-emscripten)

Friends helped with feedback:
- [@Lakr233](https://github.com/Lakr233)

