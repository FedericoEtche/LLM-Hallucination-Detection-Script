# LLM Token Confidence Visualizer

An experimental Rust renderer for token-level confidence data supplied by the
user. It can display the same analysis in a terminal, an HTML file, or Markdown.

> **Scope:** this repository does not detect hallucinations, verify facts,
> retrieve evidence, or establish whether model output is safe or correct. It
> has no published accuracy benchmark. Treat every visualization as a display of
> the input scores and flags, not as an independent assessment of the text.

## What ships

The root Rust crate provides:

- terminal, HTML, and Markdown rendering;
- a CLI for text plus user-supplied token confidence and flag data;
- Rust types and rendering functions for library use; and
- a demo path that uses synthetic/mock analysis data.

No model weights, confidence estimator, fact-checking service, Python detector,
C++ engine, web API, server, or deployment configuration is included.

## Quick start

Build and run the synthetic demo:

```console
cargo run -- --demo
```

Render supplied text and analysis data:

```console
cargo run -- --text-file sample_text.txt --confidence-file demo_data.json
```

Write the demo as HTML:

```console
cargo run -- --demo --format html --output report.html
```

Run `cargo run -- --help` for the complete CLI option list.

## Input contract

Outside demo mode, the CLI requires text (`--text` or `--text-file`) and a JSON
analysis document (`--confidence` or `--confidence-file`). The document follows
this shape:

```json
{
  "tokens": [
    {"text": "Example", "confidence": 0.92},
    {"text": " claim", "confidence": 0.35}
  ],
  "flags": [
    {
      "start": 1,
      "end": 2,
      "flag": "uncertain",
      "description": "Confidence score supplied by the caller is low."
    }
  ]
}
```

The confidence values and flags are supplied by the caller; the crate does not
derive or validate them against external evidence.

## Rust library

```rust
use llm_token_visualizer::{
    visualize_tokens, TokenAnalysis, VisualizationConfig,
};

let analysis: TokenAnalysis = serde_json::from_str(json_input)?;
let rendered = visualize_tokens(
    "Example claim",
    &analysis,
    "markdown",
    Some(VisualizationConfig::default()),
)?;
println!("{rendered}");
```

The exported `quick_analyze` helper creates mock analysis for demonstrations. It
is not a detector or a confidence-scoring implementation.

## Limitations

- Scores are only as meaningful as the upstream process that produced them.
- Flags are rendered, not independently verified.
- No factuality, calibration, robustness, or safety benchmark is provided.
- A visually high-confidence token is not evidence that a claim is true.
- Decisions based on the output require independent verification appropriate to
  the use case.

## License status

This checkout does not include a verified license grant. Do not assume rights to
reuse, modify, or redistribute the code without permission from the rights
holder.
