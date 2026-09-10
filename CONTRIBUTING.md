# Contributing Guide

The repository is designed for teammates with different levels of Git and data experience.

## The easiest way to add a use case

1. Open the [`usecases/`](usecases/README.md) folder on GitHub.
2. Open `TEMPLATE.md` and copy its contents.
3. Choose **Add file → Create new file**.
4. Name the file `your-name-short-idea.md`.
5. Paste the template, replace every placeholder, and add working source links.
6. In the commit box, write `Add <short idea> use case`.
7. Choose to create a new branch and open a pull request.
8. Ask at least one teammate to review the proposal.

If branches feel unfamiliar, a teammate can help with the final two steps. Do not overwrite another person's proposal.

## Branch names

- `usecase/short-name`
- `data/short-task`
- `analysis/short-task`
- `docs/short-task`
- `fix/short-task`

## Pull requests

Keep each pull request focused. Explain what changed, why it matters, how it was checked, and any decision the group still needs to make. Do not merge your own analysis change without another teammate reading it.

## File conventions

- Use lowercase filenames with hyphens.
- Number notebooks in the order they should run: `01_`, `02_`, and so on.
- Put reusable logic in `src/`, not in duplicated notebook cells.
- Never edit files in `data/raw/` after collection.
- Do not commit passwords, API keys, private data, downloaded environments, or large generated files.
- Prefer relative links inside repository Markdown files.

## Commit messages

Use a short action phrase, for example:

- `Add public-transport use case`
- `Document data quality checks`
- `Fix date parsing in trip loader`
- `Update presentation limitations`

## Before merging analysis

- Run the changed notebook or code from a clean start.
- Confirm that paths work from the repository root.
- Check that no secret or private dataset is included.
- Update the data dictionary or decision record if the change affects them.
- Make sure figures include titles, labels, units, and readable legends.
