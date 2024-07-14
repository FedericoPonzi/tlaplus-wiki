# tlaplus-wiki
This is an effort to try to centralize some documentation related to TLA+.

Contribution are very welcome! Please find a page that is still in draft, open an issue and start working on it. Other than new content; fixes, feedback, comments, requests are all welcome. 

---

## Bulding
You will need cargo (rust package manager) and some components:
```
cargo install mdbook
## to show last change date at the bottom of the pages.
cargo install mdbook-last-changed
## ToC
cargo install mdbook-toc
## better latex support
cargo install mdbook-katex
```

and run with:
```
mdbook serve --open
```
