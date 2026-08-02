# pijr-trainee

Training material for the PJ Jr. trainee program — Python fundamentals, data handling and web scraping with Selenium.

## Contents

**Lessons** — four notebooks progressing through Python and data manipulation, with a World Cup dataset used for exercises in lesson 4.

**Selenium handbook** — a written guide split into three levels:

- `Fundamentos/` — introduction and installation, core commands, basic script structure
- `Seletores_e_Elementos/` — HTML locators, filling form fields, waiting strategies
- `Uso_Avancado/` — form components, web scraping

**Assignments** — `TP/python_basico.ipynb` covers Python basics; `TP/TP.ipynb` is the main assignment.

**`tutorial_uv.md`** — a walkthrough of `uv`, the package manager used across the program.

## Structure

```
.
├── aula_1/ … aula_4/     # lesson notebooks (+ CopaDoMundo.csv)
├── TP/                   # assignments
├── Selenium/             # written handbook
├── tutorial_uv.md
└── pyproject.toml
```

## Running

```bash
uv sync --frozen
uv run jupyter lab
```
