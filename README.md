# ugt-verify

Where [UGT](https://github.com/auscaster/ugt)'s catalogue verifies recipes. A
recipe is someone else's code, so it is built and run only here, on GitHub's
clean machines, in a repository that holds nothing but these workflows: no
secrets, no caches, and nothing that feeds UGT's own builds.

- **`verify.yml`** verifies the recipes one pull request to `auscaster/ugt`
  changes, on amd64 and arm64, with `ugt` from `auscaster/ugt`'s main. A
  verified image is staged in `ghcr.io/auscaster/ugt-candidates`. The run reports
  to UGT's catalogue service, which trusts the report because the run's GitHub
  Actions OIDC token proves it came from this workflow on this repository's
  main.
- **`promote.yml`** copies a merged recipe's verified images, by digest, to
  `ghcr.io/auscaster/ugt-recipes`, which is what `ugt` pulls. Nothing is rebuilt.

Only the catalogue service and maintainers start them. A recipe that declares
access a maintainer reviews, such as a device or privileged mode, is held until
a maintainer starts its verification here, with **approved** set.
