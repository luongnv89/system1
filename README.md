# system1 — field guide & playground

A single-page explainer and live playground for **System One models** — models that
answer *typed questions* (`noul`, `choice`, `score`) about application state and return
probabilities instead of prose.

**Live site:** https://luongnv.com/system1/

## What's inside

- **Playground first** — the hero *is* a live form: point it at any `/v1/systemone`
  endpoint, edit the state, build typed questions, inspect real probabilities.
  Boots on a review classifier (sentiment choice + recommend noul); presets load
  phishing and refund demos.
- What a System One model is, and how it differs from a general LLM
- When to use one (routing, detection, policy, triage) — and when not to
- The `POST /v1/systemone` request/response contract, annotated
- Rules for writing good questions + three worked examples
- Copy-paste curl / Python / JavaScript integration snippets

## Backends the playground speaks to

| Endpoint | Base URL | Auth | Model |
|---|---|---|---|
| Kev (local, OSS) | `http://localhost:8009/v1` | none | `kev-latest` |
| Jev (hosted, TypeSafe) | `https://api.typesafe.ai/v1` | `Authorization: Bearer KEY` | `jev-latest` |

Run a local Kev backend:

```bash
uv sync --extra serve   # in a clone of github.com/jaredpalmer/kev
python -m kev.serve --run jaredpalmer/kev-4b --port 8009
```

`kev.serve` sets `Access-Control-Allow-Origin: *`, so the site can call it straight
from the browser. Two gotchas:

- **Private Network Access**: Chrome/Edge block a public-HTTPS page from fetching
  `http://localhost` or a LAN IP. Serve this page locally (`git clone` → `python3 -m
  http.server` → `http://localhost:8000`), use Firefox/Safari, or expose the
  endpoint over HTTPS.
- **LAN access**: `kev.serve` binds `127.0.0.1` by default — start it with
  `--host 0.0.0.0` to reach it from other devices (e.g. `http://192.168.x.x:8009/v1`).

## Measured numbers cited on the page

From machine-local benchmarks ([m-bench](https://github.com/luongnv89/m-bench), DGX
Spark / GB10): Jev 98.0 % and Kev-27B 98.0 % on the `system1` suite (98 generations);
Jev, Kev-4B and Kev-27B all F1 1.000 on a 16-email adjudicated phishing corpus.

## License

MIT — see [LICENSE](LICENSE).
