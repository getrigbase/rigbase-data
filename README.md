# RigBase data

**Measured local-LLM speeds on real home hardware.**

[getrigbase.com](https://getrigbase.com) · [Live download](https://getrigbase.com/data/) · [Hugging Face](https://huggingface.co/datasets/rigbase/rigbase-local-llm-benchmarks) · Licence: [CC BY 4.0](LICENSE)

This repository mirrors the free RigBase dataset:

| | |
|---|---|
| Builds | **150** |
| Measured results | **358** |
| Models | **51** |
| Snapshot generated | **2026-10-27** (from getrigbase.com/data/) |

Every number links to its original public source (llama.cpp discussions, llama-bench databases, blogs, GitHub repos, Mia's AI Lab recipes, and similar). RigBase collects and normalises these numbers; it does **not** re-measure them or invent estimates for the `tps` column.

## Files

- `builds.csv` — one row per measured result (358 rows)
- `builds.json` — one object per build with parts + nested results
- `LICENSE` — CC BY 4.0
- `CITATION.cff` — machine-readable citation

## Quick start

```bash
# CSV
curl -LO https://getrigbase.com/data/builds.csv

# Or clone this repo
git clone https://github.com/getrigbase/rigbase-data.git
```

```python
import pandas as pd
df = pd.read_csv("builds.csv")
# Plain decode only (drop speculative / MTP / draft runs):
plain = df[df["spec_decode"].fillna("none").eq("none")]
print(plain.groupby("model")["tps"].max().sort_values(ascending=False).head(10))
```

## Columns (CSV)

| column | meaning |
|---|---|
| `build_slug`, `build_title`, `page_url` | Build id, name, and RigBase page |
| `gpus`, `vram_gb`, `cpu`, `ram`, `power` | Hardware as stated by the source |
| `model`, `quant`, `engine` | What was run, as stated by the source |
| `tps` | Generation (decode) tokens/sec reported by the source |
| `spec_decode` | `none`, or the speculative-decoding method (MTP, DFlash, …) |
| `prefill_tps`, `prefill_tokens`, `prefill_metric` | Prefill speed only where the source reports one; never estimated |
| `est_ttft_8k_s_calculated` | Calculated (not measured): 8192 / prefill_tps when prefill_tokens ≥ 512 |
| `ttft_s_measured`, `ttft_prompt_tokens` | TTFT as measured by the source |
| `source` | Original public measurement URL |
| `source_checked_by_rigbase` | `yes` = RigBase re-read the number at the source |

Full column notes: https://getrigbase.com/data/

## Caveats

- Numbers come from different people, engines, quants and settings. Compare rows from the same source when you compute ratios.
- Filter on `spec_decode == "none"` for plain decode speed.
- Calculator pages on the site (VRAM, KV cache) are **arithmetic**, not measurements. They are not in this dataset.

## Related

- Browse builds: https://getrigbase.com/
- Can I run it? (VRAM + KV cache calculator): https://getrigbase.com/can-it-run/
- DGX Spark recipes: https://getrigbase.com/dgx-spark/
- Submit a correction or build: https://getrigbase.com/submit/

## Attribution

Required by CC BY 4.0:

> RigBase (getrigbase.com), CC BY 4.0. Each row keeps its original source URL.

Please also keep or cite the per-row `source` URL when you reuse an individual number.

## Updates

The live files at https://getrigbase.com/data/ are the source of truth. This GitHub mirror is refreshed when the site dataset changes. Issues and PRs that fix a sourced number (with a link) are welcome.
