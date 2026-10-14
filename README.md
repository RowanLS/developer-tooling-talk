# Better Code, Less Pain

**Developer tooling and why you should care**

Materials accompanying a presentation about improving the reproducibility, correctness, maintainability, and reliability of research software.

The talk introduces professional software-development practices through a small Python research project that progressively acquires developer tooling.

The emphasis is on **why these tools are useful**, not simply how to install them.

## What's included?

```text
developer-tooling-talk/
├── presentation/
│   ├── talk.qmd
│   ├── _quarto.yml
│   └── custom.scss
├── scripts/
│   └── presenter/
│       ├── demo-start
│       ├── demo-next
│       ├── demo-back
│       └── demo-reset
├── rt-demo/
├── README.md
└── LICENSE
```

The `rt-demo` directory is a Git submodule containing a separate repository with the example project and its complete development history.

## Tools covered

| Tool | Purpose |
|---|---|
| Git | Version control and tracking changes |
| GitHub | Repository hosting and collaboration |
| uv | Python environments and dependency management |
| Ruff | Linting, formatting, and complexity checks |
| Pyright | Static type checking |
| pytest | Automated testing |
| pre-commit | Automated checks before commits |
| GitHub Actions | Continuous integration |
| Dependabot | Automated dependency updates |
| Make | Simplifying common project commands |
| Espanso | Personal text-expansion shortcuts |

The presentation also discusses equivalent tooling for JavaScript/TypeScript, R, and Praat.

## Getting started

### Requirements

To explore the demonstration, you will need:

- Git
- Python and uv
- A terminal and text editor

Optional:

- Quarto, to view or modify the presentation
- Make, to run the final project's convenience commands

A GitHub account is not required to download and explore the materials.

### Clone the repository

```bash
git clone --recurse-submodules \
  https://github.com/YOUR-USERNAME/developer-tooling-talk.git

cd developer-tooling-talk
```

The `--recurse-submodules` option downloads the demonstration repository as well.

If you already cloned without this option, run:

```bash
git submodule update --init --recursive
```

## Viewing the presentation

The presentation is written in Quarto Markdown and rendered using Reveal.js.

To preview it:

```bash
quarto preview presentation/talk.qmd
```

To generate a standalone HTML presentation:

```bash
quarto render presentation/talk.qmd
```

Speaker notes are included in the presentation source.

## Exploring the demonstration

The example project analyses reaction-time data and produces a figure.

The project evolves through ten stages, represented by Git tags.

| Tag | Topic | What changes? |
|---|---|---|
| `00-start` | Starting point | A simple script that works locally |
| `01-uv` | Reproducibility | Dependency management and lockfile |
| `02-ruff` | Code quality | Automated linting and formatting |
| `03-types` | Static typing | Explicit interfaces and Pyright |
| `04-complexity` | Maintainability | Complexity checking |
| `05-tests` | Correctness | Refactoring, validation, and pytest |
| `06-precommit` | Automation | Checks before committing |
| `07-ci` | Continuous integration | GitHub Actions |
| `08-dependabot` | Maintenance | Dependency update configuration |
| `09-final` | Reproducible workflow | Makefile and complete project |

Some stages deliberately contain problems that are detected by the corresponding tool.

### Move between stages

Enter the demo repository:

```bash
cd rt-demo
```

Start at the beginning:

```bash
git switch --detach 00-start
```

Inspect the project and try running:

```bash
python analysis.py
```

This first version assumes that the necessary packages are already installed.

Now move to the next stage:

```bash
git switch --detach 01-uv
```

Run:

```bash
uv sync
uv run python analysis.py
```

Continue through the stages using their tags.

### Compare stages

Git can show exactly what changed between two stages:

```bash
git diff 01-uv 02-ruff
```

Or:

```bash
git diff 04-complexity 05-tests
```

This is particularly useful for understanding how the project evolves.

### Experiment with the code

For example, at `03-types`:

```bash
git switch --detach 03-types
uv sync
uv run pyright
```

Try passing a string instead of a `Path` to the analysis function.

Run Pyright again and inspect the diagnostic.

At `05-tests`, try changing the filtering logic:

```python
correct = data[~data["correct"]]
```

Then compare:

```bash
uv run ruff check .
uv run pyright
uv run pytest
```

The key lesson is that **different tools detect different kinds of problems**.

### Preserve your changes

Checking out a tag puts Git into a detached HEAD state. This is normal for exploring historical versions.

If you want to keep your experiments, create a branch before editing:

```bash
git switch --detach 05-tests
git switch -c my-experiments
```

Avoid switching tags with uncommitted changes unless you have saved or stashed them.

## Running the final project

Switch to the final stage:

```bash
git switch --detach 09-final
```

Install the dependencies:

```bash
uv sync --locked
```

Run the checks:

```bash
make check
```

Generate the figure:

```bash
make figure
```

The generated output is `figure1.png`.

## A note on research data

The example illustrates an important distinction between software correctness and scientific validity.

A program can run successfully, pass static analysis, and still produce scientifically incorrect results.

The tests demonstrate how domain-specific assumptions can be encoded and checked automatically.

**Raw research data should not be silently modified to make tests pass.**

If real observations fail validation, investigate their provenance and document any justified corrections or exclusions as part of the processing workflow.

## Presenter utilities

The `scripts/presenter/` directory contains utilities used during the live presentation:

- `demo-start`: return to the first stage
- `demo-next`: advance to the next stage
- `demo-back`: return to the previous stage
- `demo-reset`: restore the current stage

**Warning:** These scripts deliberately discard working-tree changes and may delete untracked files.

They are intended for a disposable presentation checkout, not for participants' normal development work.

Participants should use standard Git commands instead.

## Further reading

- [Git documentation](https://git-scm.com/doc)
- [uv documentation](https://docs.astral.sh/uv/)
- [Ruff documentation](https://docs.astral.sh/ruff/)
- [Pyright](https://github.com/microsoft/pyright)
- [pytest documentation](https://docs.pytest.org/)
- [pre-commit](https://pre-commit.com/)
- [GitHub Actions](https://docs.github.com/en/actions)
- [Dependabot](https://docs.github.com/en/code-security/dependabot)
- [Quarto](https://quarto.org/)

## Licence

Original code, documentation, and presentation materials are provided under the MIT licence, except where otherwise indicated.

The `rt-demo` submodule is separately licensed.

Third-party images, cartoons, logos, and other external materials retain their respective licences and are not covered by the repository's MIT licence.