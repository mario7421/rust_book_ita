# Il Linguaggio di Programmazione Rust

(The Rust Programming Language)

This repository contains the source of the Italian version of "The Rust Programming Language" book.

## About this fork

This is a fork of **[Spaicrab/rust_book_ita](https://github.com/Spaicrab/rust_book_ita)**, the Italian translation maintained by the [Rust User Group di Verona](https://github.com/Rust-User-Group-VR) (which points its `git-repository-url` at that organisation). The upstream repository is **archived and read-only**, so no pull requests can be opened there; this fork exists to carry the few fixes that can no longer be merged upstream.

All original work remains the property of its authors and translators: the book is by Steve Klabnik and Carol Nichols with contributions from the Rust community, translated into Italian by the Rust User Group di Verona with the work of the students of ITIS G. Marconi. Licensed under Apache-2.0 / MIT, as upstream.

### What differs from upstream

Exactly one fix, in `src/ch07-02-defining-modules-to-control-scope-and-privacy.md` and `src/ch07-03-paths-for-referring-to-an-item-in-the-module-tree.md`:

- **Three `{{#rustdoc_include}}` directives did not resolve**, so `mdbook build` reported `Could not read file for link ... No such file or directory` and left those code blocks empty in the rendered book. The ch07-02 includes pointed at `src/giardino.rs` and `src/giardino/verdure.rs`, but the listing files are `src/garden.rs` and `src/garden/vegetables.rs` (the prose was translated to the Italian module names `giardino`/`verdure` while the listings kept the upstream English ones). The ch07-03 include for `listing-07-08` had a stray `)` inside its closing braces, making the whole path unresolvable.

The paths were corrected; the listing files themselves were left in English, consistent with every other listing in this repository.

Note that `mdbook build` still emits the HTML and exits successfully despite these errors, so the breakage is silent in CI. With the fix, the build is clean and no unexpanded directives remain in the output.

### Known inconsistency left in place

In ch07-02 the prose tells the reader to create `src/giardino.rs` while the code sample next to it uses `mod garden;`. Resolving that mismatch means either translating the listings or amending the prose, which is a broader editorial decision — deliberately out of scope for the build fix above.

## How to build

The Book can easily be built by running `nix build` in the root of the project (N.B.: First make sure that the Nix package manager is available and configured to execute the `nix` commands and to support the Flake interface).

If you have `mdbook` available without Nix, `mdbook build` alone is enough; it writes the HTML to `book/`.

To produce an EPUB for an e-reader, render with `mdbook build` and convert the single-page `book/print.html` (strip its sidebar first, otherwise the converter crawls every chapter page and the output ends up with duplicated content):

```sh
ebook-convert print_clean.html rust_book_ita.epub --chapter "//h:h1"
```

## How to contribute

Along with the scripts needed to build a distribution of the Book, the Flake present in this repository is equipped with everything that is necessary to spawn a full-fledged environment useful to work on it; therefore, you can get the correct versions of `mdbook`, `cargo` and `rustc` just by typing `nix develop` in your shell.

Once everything is set up, you can quickly check your work by launching `mdbook serve --open` and a locally-hosted page with your version of the Book should pop up in your default browser.

In case you want to contribute your changes, you then only have to commit your improvements, create a Pull Request and wait for it to be merged.

Contributions to the Italian text are welcome here, since upstream no longer accepts them. Please keep the convention already established in the repository: prose is translated, but code listings under `listings/` stay in English and their file names are left untouched.