This file is meant to be a project-agnostic dossier on how our codebases should be architected, operated, and iterated. It is not meant to be exhaustive, but it is meant to be prescriptive — and also very good with which to bootstrap an `AGENTS.md` file.

## Codebase organization

- Arbitrary scripts should be written in Python unless that's obviously unfeasible, to let us take advantage of [inline dependencies](https://simonwillison.net/2024/Dec/19/one-shot-python-tools/#inline-dependencies-and-uv-run), and exist in a `scripts/` directory.
- Every repository must have a [justfile](https://github.com/casey/just) with the following commands: `test`, `bootstrap`, `install`, `lint`, `build`, and `dev`.
- Code changes should be backed by unit tests whenever possible.
- Tests should be colocated with the files they're testing. Instead of a test for `foo/bar/baz.py` existing in `tests/foo/bar/baz.py`, it should be in `foo/bar/baz--test.py`.

### Python

- Use `pytest` as the general testing library. Avoid class-based tests.
- Use `ruff` as the linter and formatter. 
- Use `uv` as the dependency manager.

### Frontend

- Prefer using `bun` as a package manager and/or runtime over `npm` or `pnpm`.0
- Use `biome` as a lint tool and `knip` to detect dead codepaths.
- Use `typescript` unless the backing JavaScript is so small so as to not even warrant a build step.
- Use `tailwind` as a CSS library.
- Use [Heroicons](https://heroicons.com/) as an icon library.

#### SEO

- All marketing sites should have a sitemap.xml and proper meta tags.
- All marketing sites (or sites with a user-facing component) should be registered on [Google Search Console](https://search.google.com/search-console/about) and [Ahrefs](https://ahrefs.com/).

## Vendors

- Use Stripe for payments.
- Use Postmark for transactional email.
- Use Buttondown (😎) for broadcast email.
- Use Fathom for marketing site analytics.
- Use Sentry for error tracking.
- Use Axios for logging.
- Use Cloudflare for DNS.
- Use Ahrefs for SEO.

## Git 

- Use [squash merges](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/incorporating-changes-from-a-pull-request/about-pull-request-merges#squash-and-merge-your-commits).
- Pull requests should be backed by GitHub actions which run the lint and test actions outlined in the `justfile`.
- Whenever possible, break your code changes and pull requests up into _atomic_ chunks — meaning that a given change only does one thing. This makes pull requests easier to review and bugs/incidents easier to diagnose.

## Miscellany

- Register the outbound sending domain in [Google Postmaster Tools](https://postmaster.google.com/).
- Never use `example.com` or other such obviously generic placeholders: try and have your dummy data be as realistic as possible.
