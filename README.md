# thecqzz.github.io

Personal website of Qizhao Chen, built with the [al-folio](https://github.com/alshedivat/al-folio) Jekyll theme and hosted on GitHub Pages.

## Where things live

- Bio and home page: `_pages/about.md`
- Publications: `_bibliography/papers.bib` (thumbnails in `assets/img/publication_preview/`, PDFs in `assets/pdf/`)
- Projects: one Markdown file per project in `_projects/` (images in `assets/img/projects/`)
- News: one Markdown file per item in `_news/`
- CV: the CV tab opens `assets/pdf/Qizhao_Chen_CV.pdf` directly. Replace that file to update it.
- Site settings, name, and links: `_config.yml` and `_data/socials.yml`

## Preview locally

```bash
docker compose up
# open http://localhost:8080
```

## Deploy

Push to `main`. The workflow in `.github/workflows/deploy.yml` builds the site and publishes it to the `gh-pages` branch.
In the repository settings, set GitHub Pages to deploy from the `gh-pages` branch (root folder).

Before committing, format the files:

```bash
npm install
npx prettier . --write
```
