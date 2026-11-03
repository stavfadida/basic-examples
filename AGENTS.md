# Repository Guidelines

## Project Scope & Structure

This project generates Python code intended to run in Google Colab notebooks. Keep examples as `.ipynb` notebooks or clearly labeled Python cells/scripts that can be copied into Colab. Organize material by topic or example; avoid adding non-Python application code or unrelated runtime targets.

## Build, Run, and Dependencies

There is no local build system or dependency manifest. Run generated code in Google Colab. Put any required `pip` installs in a clearly marked setup cell near the top of the notebook, and document required secrets or Colab settings before use. Keep cells runnable in order and avoid relying on undeclared files from a local machine.

## Python Style

Use Python 3 conventions, four-space indentation, `snake_case` for variables and functions, and descriptive names. Keep cells focused, with brief Markdown headings and comments where they clarify a step. Prefer explicit imports and reproducible setup; pin package versions when compatibility requires it.

## Testing Guidelines

No automated test framework is configured. Validate notebook examples by running all cells from a fresh Colab runtime, in order, and confirm outputs and required dependencies. Add focused tests only when reusable Python modules are introduced, and document how to run them.

## Commit & Pull Request Guidelines

No Git history is available to establish a commit convention. Use short, imperative commit subjects (for example, `Add LangGraph notebook example`). Pull requests should explain the example and its prerequisites, list validation performed, and include screenshots or representative output for notebook changes.

## Security & Configuration

Never commit API keys, tokens, or personal data in notebook cells or outputs. Use Colab Secrets for credentials, and clear sensitive cell outputs before committing notebooks.
