# Contributing to the token confidence visualizer

This repository ships a Rust renderer for caller-supplied token confidence
scores and flags. Contributions should stay within that scope.

Useful changes include:

- renderer correctness and accessibility;
- input validation and error handling;
- terminal, HTML, and Markdown output tests;
- documentation of the supplied-data contract; and
- dependency and CI maintenance.

Do not describe the crate as a hallucination detector, fact checker, confidence
estimator, safety system, or production benchmark. New detection or verification
systems require a separate, evidenced project and are outside this repository's
current scope.

Before opening a pull request, run:

```console
cargo fmt --check
cargo test
```

Explain the user-visible behavior changed and include tests where practical.
