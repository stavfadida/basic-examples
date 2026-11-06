# Repository Guidelines

## Project Structure & Module Organization

This project generates only Python code intended to run in Google Colab. Each example is kept as a self-contained Jupyter notebook in the repository root; for example, `ticket_router.ipynb` demonstrates a LangGraph ticket-routing flow. There are currently no separate source, test, or asset directories. Add new notebooks with descriptive, lowercase filenames such as `multi_step_agent.ipynb`, and organize longer examples with clear Markdown sections.

## Build, Test, and Development

There is no local build system, dependency manifest, or automated test command. Open a notebook in Google Colab and run its cells from top to bottom in a fresh runtime. Put required `%pip install` commands in a setup section near the beginning. Keep examples self-contained and explain any required Colab settings or secrets before the relevant code.

## Coding Style & Naming

Use Python 3, four-space indentation, `snake_case` for variables and functions, and descriptive names for graph nodes and state fields. Keep code cells focused and ordered so earlier cells define everything later cells use. Use explicit imports and brief comments only where they clarify behavior. Pin package versions when an example depends on a specific version for compatibility.

## Testing Guidelines

Validate notebook changes by running every cell in order in a fresh Colab runtime, checking both the output and dependency setup. No test framework or coverage target is configured. If reusable Python modules are added later, include focused tests and document their command and prerequisites.

## Commit & Pull Request Guidelines

The available Git history uses short, imperative commit subjects (for example, `Add ticket router notebook`); follow that style. A pull request should describe the example and its prerequisites, state how it was validated, and include representative output or screenshots for notebook changes.

## Security & Configuration

Never commit API keys, tokens, personal data, or sensitive notebook outputs. Store credentials in Colab Secrets, document the required secret names, and clear sensitive outputs before committing.
