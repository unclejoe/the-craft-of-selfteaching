# Repository Guidelines

## Project Overview

**The Craft of Self-Teaching** (自学是门手艺) by 李笑来 is a Chinese-language book teaching self-teaching methodology through learning Python programming. The project is a flat collection of Jupyter notebooks (`.ipynb`) that serve as the primary content, with a parallel Markdown rendering in `markdown/` generated via `nbconvert`.

License: CC-BY-NC-ND.

---

## Architecture & Data Flow

Flat repository — no package structure, no build system. The book content lives in two parallel formats:

```
.ipynb  ── (primary) ──> JupyterLab reader
   │
   └── nbconvert ──> markdown/*.md ──> GitHub reader
```

Notebooks load data files, Python modules, and images from the same root directory. There is no automated CI/CD pipeline, test suite, or deployment configuration.

---

## Key Directories

| Path | Purpose |
|---|---|
| `.` (root) | 41 Jupyter notebooks, 2 Python source files, data files |
| `markdown/` | 40 `.md` files — nbconvert output of all notebooks |
| `images/` | ~85 image assets (PNG, GIF, JPG, PDF) — chapter illustrations |
| `from-readers/` | 10 reader-contributed self-teaching stories (Markdown) |
| `my-notes/` | Reader-contributed study notes |
| `.venv/` | Python 3.12 virtual environment (gitignored) |

---

## Development Commands

```bash
# Setup
uv venv --python 3.12
uv pip install jupyterlab nbformat ipywidgets

# Launch
source .venv/bin/activate
jupyter lab

# Or without activation
.venv/bin/jupyter lab

# Regenerate Markdown from notebooks
jupyter nbconvert --to markdown --output-dir markdown/ *.ipynb
```

---

## Code Conventions & Common Patterns

### Python (notebook code cells + standalone `.py` files)

- **Naming**: `snake_case` for functions and variables; `UPPER_CASE` for constants.
- **No type annotations** — code is intentionally beginner-friendly.
- **Docstrings**: Triple-quoted `"""..."""`, Sphinx-ish `:param:` / `:returns:` style (see `mycode.py`).
- **Error handling**: Minimal — teaching code uses `if/else` guards, not `try/except`.
- **`if __name__ == '__main__'` guard** used in standalone scripts (`that.py`).

### Notebook-specific patterns

Every code cell in technical chapters starts with the boilerplate:

```python
from IPython.core.interactiveshell import InteractiveShell
InteractiveShell.ast_node_interactivity = "all"
```

This ensures all expression results print, not just the last. Repeated per-cell, never set globally.

- **Comments**: Chinese inline explanations accompany code.
- **`print()`** used for demonstration output, never logging.
- **`%load` magic** used to load larger code examples (e.g., ELIZA chatbot in `Part.3.B.4.regex.ipynb`).
- **`IPython.display`** used for YouTube embeds (`IFrame`) and image display.
- Navigation cells `[Next Page](./next-notebook.ipynb)` at the end of each notebook.

### Markdown (`markdown/` directory)

- **Language**: Chinese prose; code and technical terms in English.
- **Code blocks**: Python fenced blocks with indented plain-text output (nbconvert artifact).
- **Cross-references**: Relative `.md` links spanning chapters and parts.
- **Images**: GitHub raw URLs (`https://raw.githubusercontent.com/selfteaching/the-craft-of-selfteaching/master/images/...`)
- **Footnotes**: HTML anchor-based (`<a href='#fn1' name='fn1b'><sup>[1]</sup></a>`).
- **Blockquotes**: Heavily used for emphasis and key points.

### File naming

- Notebooks: `Part.<N>.<LETTER>.<topic>.ipynb` (e.g., `Part.1.E.5.strings.ipynb`)
- Sub-sections: `Part.<N>.<LETTER>.<N>.<topic>.ipynb` (e.g., `Part.2.D.1-args.ipynb`)
- Front matter: `NN.topic.ipynb` (e.g., `00.cover.ipynb`)
- Appendices: `T-appendix.<topic>.ipynb`
- Markdown: mirrors notebook naming with `.md` extension

---

## Important Files

| File | Purpose |
|---|---|
| `00.cover.ipynb` | Book cover + table of contents (links to all notebooks) |
| `mycode.py` | Teaching example: `is_prime()`, `say_hi()` with docstrings |
| `that.py` | ROT13 decoder — standalone script with `__main__` guard |
| `words_alpha.txt` | 178K-word English dictionary (sorted) — used in regex/string exercises |
| `life-expectancy-china-1960-2016.txt` | CSV data for matplotlib exercises |
| `hdi-china-1870-2015.txt` | CSV data for data processing exercises |
| `regex-target-text-sample.txt` | HTML sample text for regex exercises |
| `symbols.numbers` | Apple Numbers spreadsheet (symbol reference) |
| `markdown/README.md` | Conversion notes, Stylus CSS recommendations, full TOC |

---

## Runtime/Tooling Preferences

| Tool | Requirement |
|---|---|
| **Python** | 3.12+ (managed via `uv`) |
| **Package manager** | `uv` (0.9.26+) |
| **Notebook runtime** | JupyterLab 4.x |
| **Kernel** | Python 3 (ipykernel) |
| **Notebook format** | nbformat v4, minor 2 |
| **External deps** | `matplotlib`, `numpy`, `matplotlib-venn`, `ipywidgets` |
| **Editor** | VS Code (preferred, documented in appendix) |

---

## Testing & QA

- **No test framework** — the project has no `pytest`, `unittest`, or any test runner.
- **Ad-hoc verification**: Notebooks use `print()` and assert-like boolean checks for teaching purposes.
- **`Part.2.D.7-tdd.ipynb`** teaches the TDD concept via iterative `is_leap()` implementation, but does not use a formal testing library.
- **No CI configuration** — no GitHub Actions, no linting, no formatting checks.
- **Notebook state**: All notebooks are self-contained; expected outputs are embedded in cell metadata (not validated).

---

## Known Issues

- `.gitignore` has a typo: `.ipynb_chechpoints` (missing `k` in `checkpoints`).
- `markdown/Q.good-communiation.md` has a typo (should be `communication`).
- `from-readers/liuyunxin-self-teaching-storise.md` has a typo (should be `stories`).