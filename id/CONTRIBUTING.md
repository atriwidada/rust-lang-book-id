# Contributing

We'd love your help! Thanks for caring about the book.

## Where to Edit

All edits should be made in the `id/src` directory.

The `nostarch` directory contains snapshots for sending edits to the publishers
of the print version. The snapshot files reflect what has been sent or not, so
they only get updated when edits are sent to No Starch. **Do not submit pull
requests changing files in the `nostarch` directory, they will be closed.**

## Checking for Fixes

The book rides the Rust release trains. Therefore, if you see a problem on
https://doc.rust-lang.org/stable/book, it may already be fixed on the `main`
branch in this repo, but the fix hasn't gone through nightly -> beta -> stable
yet. Please check the `main` branch in this repo before reporting an issue.

Looking at the history for a particular file can also give more information on
how or whether an issue has been fixed or not if you're trying to figure that
out.

Please also search open and closed issues and open and closed PRs before
reporting a new issue or opening a new PR.

## Licensing

This repository is under the same license as Rust itself, MIT/Apache2. You
can find the full text of each license in the `LICENSE-*` files in this
repository.

## Code of Conduct

The Rust project has [a code of conduct](http://rust-lang.org/policies/code-of-conduct)
that governs all sub-projects, including this one. Please respect it!

## Expectations

Because the book is [printed](https://nostarch.com/rust), and because we want
to keep the online version of the book close to the print version when
possible, it may take longer than you're used to for us to address your issue
or pull request.

So far, we've been doing a larger revision to coincide with [Rust
Editions](https://doc.rust-lang.org/edition-guide/). Between those larger
revisions, we will only be correcting errors. If your issue or pull request
isn't strictly fixing an error, it might sit until the next time that we're
working on a large revision: expect on the order of months or years. Thank you
for your patience!

## Help wanted

If you're looking for ways to help that don't involve large amounts of
reading or writing, check out the [open issues with the E-help-wanted
label][help-wanted]. These might be small fixes to the text, Rust code,
frontend code, or shell scripts that would help us be more efficient or
enhance the book in some way!

[help-wanted]: https://github.com/rust-lang/book/issues?q=is%3Aopen+is%3Aissue+label%3AE-help-wanted

## Translations

We'd love help translating the book! See the [Translations] label to join in
efforts that are currently in progress. Open a new issue to start working on
a new language! We're waiting on [mdbook support] for multiple languages
before we merge any in, but feel free to start!

Please be aware that this book is quite large
```
$ wc src/*.md | tail -1
  23164  162758 1078941 total
23k+ lines
162k+ words
```
If you can translate 100 lines a day, it will take more than 230 days 
(more than 7 months) to finish it.

## Adapted from French
** to be adjusted to Indonesian team workflow as needed*

# Contributing to translate

## Guidelines

### Translation terms

We need to coordinate us with the main English terms translations.

In this purpose, please refer to the file `/id/src/translation-terms.md`
when you need to translate a technical term.

*(PS : see the next process `Add a translation term` on this same page)*

### Translation of Rust code

In rust code, we should translate :

- comment
- string text
- variable name
- struct name (which is custom, that means it not come from *standard library*
  or *external crate*)
- enum name (which is custom, that means it not come from *standard library*
  or *external crate*)

All the standard code (and terminal outputs) should stay in English.

### Files

The name of all the files should not be translated, so just keep them unchanged,
in English.

Please limit each line of Markdown file to 80 characters (including spaces). You
can write your file as you want, but it would be nice to use a tool like
[https://www.dcode.fr/text-splitter](https://www.dcode.fr/text-splitter) on your
translated paragraphs before committing.

### Punctuation

Please use a [non-breaking space](https://en.wikipedia.org/wiki/Non-breaking_space)
instead of space on punctuation who need this space before (like `:`, `!`, `?`,
...).

## Tools

We recommend to use the following tools :

- [Microsoft Visual Studio Code](https://code.visualstudio.com/) for writing
  code, and improve it with following extensions :
    - [`davidanson.vscode-markdownlint`](https://marketplace.visualstudio.com/items?itemName=davidanson.vscode-markdownlint)
    - [`moshfeu.compare-folders`](https://marketplace.visualstudio.com/items?itemName=moshfeu.compare-folders)
    - [`rust-lang.rust`](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust)
- [Deepl](https://www.deepl.com/translator) for translations

## Processes

### Translate flow

*NB : the following term `main translate repository` refers to
<https://github.com/atriwidada/rust-lang-book-id/>*

01. Open or edit an GitHub issue in the *main translate repository* to report to
    other that you are working on `ch00-00-introduction`.
      - if someone has reported to work on the same page you want to translate :
        - if its works is *active*: you should work on another page.
        - else: you can fork his/her repo in the following step 02 (instead of
          the *main translate repository*), and please mention this in the
          existing issue.
      - else: just follow the next instructions.
02. Fork the *main translate repository* in GitHub (on your account)
03. `git clone https://github.com/<your_fork>.git`
    (copy your fork on your hard-disk). You should be on the `french-release`
    branch by default.
04. `cd <your_fork>` (go inside your fork folder your previously downloaded)
05. `git checkout -b <your_new_working_branch_name>` (create your new working
    branch)
06. Copy the file you want to translate : `/src/ch00-00-introduction.md` into
    `/id/src/ch00-00-introduction.md`. Tip: you can use a tool like
    [Castor Whispers](https://github.com/Jimskapt/castor_whispers) in order to
    copy and mass-comment each paragraphs of this file.
07. Add the text for the link to this file in the `/FRENCH/src/SUMMARY.md`.
08. Comment each English paragraphs of this file. The goal is to keep an
    *invisible* version of the English book, in order to easily reflect the
    changes of the English source (english-book) when they occur later, and to
    easily translate them.
09. Write each of your translated paragraph under each commented English
    paragraph.
      - Please quickly read following `Guidelines` currently on this page.
      - A little tip : the [deepl.com](https://www.deepl.com/) translator.
10. (optional) Limit each line of your translation to 80 characters, thank to a
    tool like
    [https://www.dcode.fr/text-splitter](https://www.dcode.fr/text-splitter).
11. (optional) `cd id && mdbook build && cd ..` (build the book in
    `/id/book`). Open its index.html file in your browser, and check its
    correctness. It also should help you for next task.
12. `git add -A && git commit -m "<Description of your work>"` (committing your
    work)
13. (optional) `git rebase -i HEAD~<the number of commits you need to merge>`
    (squash all your commits into one commit)
14. `git push origin` (pushing your work on your fork)
15. In GitHub, create a new pull request from your fork to the main translation
    repository, in order to mark your work ready for a proofreading.
16. After someone proofreading it (and eventually some edits), it would be
    merged on `id-release` branch.

### Update your fork with another fork

01. `git remote add english-book https://github.com/rust-lang/book.git`
    (Add source of the *English main repository*)
02. `git fetch english-book` (fetching the latest changes from the *English main
    repository*)
03. `git merge english-book/master` (merging latest changes from *English main
    repository* on current branch)

It is also the same to update your fork with the main translate repository.

### Add a translation term

*(PS : see previous Guideline `Translation terms` on this same page)*

01. Check if you are one your working branch, or create it (see `Translate flow`
    process)
02. Edit the `/id/src/translation-terms.md` file with your new technical
    term translation. Write it in singular and if necessary, specify the gender
    of the translation in `Remarques` column.

### Translate figures

Let's suppose you want to translate Figure 42-69.
You need to have the `dot` installed, for instance after typing
`sudo apt install graphviz`.

01. Copy the DOT figure you want to translate (for instance, `trpl42-69.dot`)
    from `/dot/` to `/id/dot/`.
02. Edit `/id/dot/trpl42-69.dot` and translate the text into French.
    You should not translate the names and values of attributes.
03. Run `dot id/dot/trpl42-69.dot -Tsvg > FRENCH/src/img/trpl42-69.svg`
04. Edit the new file `id/src/img/trpl42-69.svg`:
    - Within the `<svg>` tag, remove the `width` and `height` attributes, and
      set the `viewBox` attribute to `0.00 0.00 1000.00 1000.00` or other values
      that don't cut off the image.
    - Replace every instance of `font-family="Times,serif"` with
      `font-family="Times,Liberation Serif,serif"`.

[Translations]: https://github.com/rust-lang/book/issues?q=is%3Aopen+is%3Aissue+label%3ATranslations
[mdbook support]: https://github.com/rust-lang-nursery/mdBook/issues/5
