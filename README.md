# ugt-verify

Where [UGT](https://github.com/auscaster/ugt)'s catalogue verifies recipes. A
recipe is someone else's code, so it is built and run only here, on GitHub's
clean machines, in a repository that holds nothing but this workflow: no
caches, and nothing that feeds UGT's own builds.

**`verify.yml`** builds `ugt` from `auscaster/ugt`'s main and runs each recipe's
verification on amd64 and arm64. It has two modes:

- **verify** checks the recipes one pull request to `auscaster/ugt` changes, at
  its head. Nothing the run builds leaves it.
- **publish** checks merged recipes again, as `main` holds them, and pushes the
  images it verified, unchanged, to `ghcr.io/auscaster/ugt/recipes`, which is
  what `ugt` pulls.

Recipe code runs only in jobs whose token can do nothing; the jobs that read
`auscaster/ugt`, push images and report run none. Each run reports to UGT's
catalogue service, which trusts the report because the run's GitHub Actions
OIDC token proves it came from this workflow on this repository's main.

Only the catalogue service and maintainers start it. A recipe that declares
access a maintainer reviews, such as a device or privileged mode, is held until
a maintainer starts its verification here, with **approved** set.
