# eval-chembench

**ChemBench: Are large language models superhuman chemists?**

**Paper:** https://arxiv.org/pdf/2404.01475v2

ChemBench is designed to reveal limitations of current frontier models for use in the chemical
sciences. It consists of 2786 question-answer pairs compiled from diverse sources. Our corpus
measures reasoning, knowledge and intuition across a large fraction of the topics taught in
undergraduate and graduate chemistry curricula. It can be used to evaluate any system that can
return text (i.e., including tool-augmented systems).

## At a glance

| | |
|---|---|
| Upstream | [`src/inspect_evals/chembench`](https://github.com/UKGovernmentBEIS/inspect_evals/tree/main/src/inspect_evals/chembench) |
| Group | Knowledge |
| Total samples | 2,786 |
| Execution class | `plain` |
| Cost class | `medium` |
| Flags | no sandbox, no network |
| Tags | — |

### Tasks

| Task | Samples |
|---|---|
| `chembench` | 2,786 |

### External assets

- `huggingface` — `jablonkagroup/ChemBench` (pinned)

## Running one problem

OpenEvalz is problem-level: the atomic unit is a single sample, not the whole eval.

```bash
inspect eval inspect_evals/chembench \
  --sample-id "<sample-id>" \
  --model openai-api/trustedrouter/<model> \
  --token-limit 200000
```

> **Two things that bite here, both verified in Inspect's source.**
>
> 1. **`--cost-limit` does not work on this routing path.** Inspect only records cost when its
>    pricing table resolves the model, and `_model_info.py` strips only `azure|bedrock|vertex`
>    prefixes — so `trustedrouter/<model>` never resolves and the cap silently never binds. The
>    real spend cap is enforced **server-side by TrustedRouter** via the delegated key's
>    `limit_microdollars` and spend window. Use `--token-limit` as the in-process bound.
> 2. **`--sample-id` matches with `fnmatch`.** A glob silently selects many samples and only warns.
>    Always pass a literal id.

## Reproducibility

`bundle.template.json` is the contract. A run that cannot emit a complete bundle does not publish.
Every image is pinned by `sha256` digest and every dataset by revision.

## Licensing

OpenEvalz wrapper code in this repository is **Business Source License 1.1** (see `LICENSE`) —
Licensor Lore Hex Corp, Change Date four years from publication, Change License Apache 2.0, no
Additional Use Grant. Same terms as TrustedRouter. Source-available, not open source: you may read,
modify and make non-production use of it, but production use needs a commercial licence
(licensing@openevalz.com).

**The packaged evaluation is NOT relicensed.** The task code, dataset and container images come from
upstream under their own terms — inspect_evals is MIT (UK AI Security Institute), and individual
datasets and images carry their own, sometimes unstated, licences. BSL covers only the OpenEvalz
packaging around them. See `NOTICE.md`, which must be completed before this repo publishes anything.
