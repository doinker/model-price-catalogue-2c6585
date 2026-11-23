# Model price catalogue

The price list and release files for the local model gateway (`llmgate`).

Three things live here:

| Path | What it is |
|---|---|
| `catalogue.json` | The price map the gateway reads, plus a small `x_meta` block. Fetched by `llmgate-update-check` on a 5-minute schedule with a conditional request, so an unchanged file costs 0 bytes. |
| `dist/llmgate-0.1.0.tar.gz` | The install package. |
| `SHA256SUMS` | Checksums for the package. |

## Install

Linux, 64-bit, x86_64 or ARM64. One package holds both builds.

```bash
curl -fLO https://raw.githubusercontent.com/doinker/model-price-catalogue-2c6585/main/dist/llmgate-0.1.0.tar.gz
curl -fL  https://raw.githubusercontent.com/doinker/model-price-catalogue-2c6585/main/SHA256SUMS -o SHA256SUMS
sha256sum -c SHA256SUMS
tar xzf llmgate-0.1.0.tar.gz
cd llmgate-0.1.0
./bin/install.sh                     # picks your machine's build; asks for a key if it finds none
```

`bin/install.sh` is a two-line picker. It runs `bin/x86_64/llmgate` or `bin/aarch64/llmgate`,
whichever matches `uname -m`, and passes your arguments through.

Check which build you got:

```bash
~/.llmgate/bin/llmgate-update-check --print-build     # x86_64-linux-musl or aarch64-linux-musl
```

`llmgate install` writes only under `~/.llmgate`, plus one provider block in your
agent's own settings. It adds a fallback route so your agent keeps working if the
gateway stops. `llmgate uninstall` puts everything back.

Read `README.md` inside the package for what it does and what it does not do.

## Notes for whoever maintains this

- `catalogue.json` must stay valid JSON with a top-level `data` object. The
  gateway keeps its previous copy when a new file does not parse.
- Keep the shape stable. The update check compares the ETag, so an unchanged
  file costs one conditional request and 0 bytes.
- Do not put a key, a token, or a private address in this repository.
