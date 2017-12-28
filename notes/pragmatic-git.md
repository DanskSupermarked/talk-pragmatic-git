---
title: Pragmatic Git
sansfont: DejaVu Sans
slide-numbers: true
biblatex: true
biblatex-chicago: true
biblatexoptions: [notes, noibid]
bibliography: ../sources.bib
links-as-notes: true
---

# Let's play a game

## Rules

I show a complete commit message

- *Almost* randomly selected
- Token anonymization

Tell me:

- What did the change do?
- Would you like to maintain it?

##

COM-17019 Smiley reports and get directions

\note{
- Displays smiley report
- Nothing about directions
}

##

COM-9746 COM-10607 Selenium

\note{
- Address related changes, in and out of Selenium
}

##

DefaultPriceFeedFacade.java edited online with Bitbucket

\note{
- Use local variable
}

##

Use 'break-word' on facet titles

COM-17407

As 'break-all' will break all words the 'break-word' values works better
even though it does not technically exist.

# \huge Do you spend too much time reasoning about changes?

## Goals

> - Raise expectations of version control
> - Increase confidence in working with Git
> - Stop using `commit -m`, `pull`

\onslide{<4->}{All of this will take more of your time.}

## How?

\begin{center}
\includegraphics{../media/retention-pyramid.jpg}
\end{center}

# You have to learn Git

## Obviously*

\begin{center}
\includegraphics{../media/obvious.jpg}
\end{center}

## Operational misfits

> *scenarios in which Git behaves in a way that is unpredictable or
> inconvenient*

- Saving changes: can't commit unfinished work
- Switching branches: can't switch branch with unfinished work
- Detached head: losing commits made outside of a branch
- File rename: can't find renamed file with replaced contents
- File tracking: unstaged changes not committed
- Untracking file: can't ignore local changes to tracked file

\cite{derosso:misfits}

## Operational misunderstandings

- Saving changes: dual nature of Git commit
- Switching branches: commit duality
- Detached head: real problem, unlikely premise
- File rename: Theseus's paradox
- File tracking: *index* ("staging area") is a fundamental feature
- Untracking file: commit duality/make up your mind

## Where to start learning

```
$ man git
$ man gittutorial
$ man giteveryday
$ man gitglossary
$ man gitrevisions
$ man gitcli
```

## Where to start learning

- \href{https://git-scm.com/book/en/v2}{Pro Git}, by Scott Chacon, Ben Straub,
  et al.
- \href{http://marklodato.github.io/visual-git-guide/index-en.html}{A Visual
  Git Reference}, by Mark Lodato

## Centralised versus distributed

### Centralised

- Requires a server/client setup
- Single master-server knows history truth
- Exclusive file locking

### Distributed

- No concept of *server*
- Every ~~repository~~ branch is its own truth
- Every ~~repository~~ branch has *complete* history
- No file locking

## Command line comfortability

Command completion is exploration in the shell; cued recall

```
$ git log --gr
...aph   ...ep=
```

## Command line comfortability

Improve completion ergonomics by adding this to `~/.inputrc`:

```sh
# Replace completion prefixes with ellipsis.
set completion-prefix-display-length 2
# Case insensitive Tab completion.
set completion-ignore-case on
# Show all results on first Tab-press.
set show-all-if-ambiguous on
set show-all-if-unmodified on
```

## Git environment

Keep Git up to date (currently \href{https://git-scm.com/}{2.15.1}).

- Security fixes
- New features
- Speed improvements
    - Especially on Windows

```
$ sudo add-apt-repository ppa:git-core/ppa
```

## Git environment

You can change many default behaviours with `git config`, e.g.

- Friendlier diff
    ```
    $ git config --global diff.indentHeuristic true
    ```
- Cache `status`
    ```
    $ git config --global core.untrackedCache true
    ```
- Custom word diff
    ```
    $ git config diff.csv.wordRegex \
        '"[^,;\n]+[,;\n]|[,;]"'
    $ printf '*.csv diff=csv\n' >> .git/info/attributes
    $ git diff --word-diff -- '*.csv'
    ```

## Git environment

Avoid changing fundamental behaviour lest you forget about it, e.g.

- don't make `pull --rebase` default behaviour of `pull` with `pull.rebase`;
  prefer explicit alias
    ```
    $ git config alias.pr 'pull --rebase'
    ```
- don't magically transform line endings locally with `core.autocrlf`; prefer
  enforcing global truth
    ```
    $ printf '* text=auto !eol\n' >> .gitattributes
    ```

## Git environment

Keep your head above water with Git-enabled `PS1`:

```
~/foo (master|REBASE-i 1/2) $ grep PS1 ~/.bashrc
PS1='\w$(__git_ps1) \$ '
```

## Index

The *index* is a *cache* of changes *staged* for commit

- Stage changes with
    ```
    $ git add --patch
    ```
- Unstage changes with
    ```
    $ git reset --patch
    ```
- Stage changes from *past* state with
    ```
    $ git checkout --patch <revision>
    ```
- Review staged changes with
    ```
    $ git diff --staged
    ```

## Branch naming

Use *descriptive* and *accurate* branch names

- Succinctly summarises changeset
- Naturally limits changeset scope

## Branch naming

```
$ git checkout COM-
...13515
...14504
...16604
...17441
...17562
...17811
...18549
...18805
...18866
...19662
...19705
```

## Branch naming

- Tickets are not descriptive, and are sometimes inaccurate or imprecise
    - Merge request *source* branch is fixed so renaming is annoying
    - Easy rule of thumb I would like us to outgrow
- "feature", "hotfix", and other prefixes are little better
- Avoid `/` because of silly filesystem edge-case

## Branch naming

> - `disable-variant-selector`
> - Otherwise, `COM-123-disable-variant-selector`

## Dual nature of a Git commit

Every Git commit serves one of two roles:

1. temporary, save-my-work snapshot
1. permanent publication of work

No technical difference, distinction is left to the author

## The model commit patch\footnote{
Note: commits actually \emph{do not} have patches.}

Expected properties:

- Atomic: all or nothing
- Consistent: always valid state

Greatly simplifies review, test, troubleshooting (especially `git bisect`)

- Commit "size" is a red herring
    - As small as possible, as large as necessary
    - Don't bundle unrelated work
    - Don't sacrifice atomicity or consistency

## The model commit message

\begin{center}
\includegraphics{../media/commit-message.png}
\end{center}

## The model commit message

1. Subject line
    - Historically comes from email subject
    - Short; 50-ish characters
    - Imperative mood (present tense)
    - No period
1. Message body detailing change
    - Wrap prose to about 72 characters
    - Plain-text paragraphs, bullet lists, footnotes

\cite{tpope:commit}.

## The model commit message

    Subject line
    <blank line>
    <ticket>[, <ticket>...]
    <blank line>
    Optional, detailed message body

## The model commit message

- Leave ticket out of subject line
    - Takes up space
    - Conveys no information
    - Easy to recover from commit message
- Reference relevant tickets
    - Find all related commits with `git log --grep <ticket>`
    - Placement technically irrelevant but 1\textsuperscript{st} paragraph
      causes fewest stylistic clashes
- Describe change in the message body
    - Summarise problem
    - Discuss solution
    - Compare alternatives

## The model commit message

Exercise: run this and try to make sense of the result:

```
$ git show $(git rev-list --no-merges master | shuf -n 1)
$ git log --no-merges --before='6 months ago'
```

## The model commit message

More self-serving examples of verbiage:

```
$ git log --author 'Mikkel Kjeldsen'
```

## Synchronizing

- Retrieve changes
    ```
    $ git fetch [remote] [--tags] [--prune]
    ```
- Integrate changes
    ```
    $ git rebase [remote/branch]
    $ git merge [remote/branch]
    ```
- Push changes
    ```
    $ git push [remote <branch>]
    $ git push --force-with-lease <remote> <branch>
    ```

## `git pull`

- Means `git fetch && git merge`
- `git pull --rebase` is saner default for centralised workflow
    ```
    $ git fetch && git rebase
    ```
    - Always replays ("re-commits") local changes on top of new remote changes
    - Never replays (known) *rewritten* changes
    - Does The Right Thing™ when sharing branches

## You should *probably* rebase

- Avoid repeat synchronization merges
    ```
          o---+---o---+---o---o topic
         /   /       /         \
    o---o---o---o---o---o---o---+ master
    ```
- "*It worked yesterday*" not useful if broken after merge

## You should *probably* rebase

```
$ git config alias.pr 'pull --rebase'
$ git config alias.force-push \
    'push --force-with-lease --repo='
```

# Tooling

## Tooling

### In general

Most tools are bad for general use but might excel at specifics.

I don't care what you use, only what you do with it.

### In specific

- Browsing
- Editing
- Conflict resolution

## Browsing

Git has some very powerful tools built-in, e.g.

- Draw a graph
    ```
    $ git log --graph --oneline --decorate
    ```
- grep commit log for `<regex>`
    ```
    $ git log --grep <regex>
    ```
- grep patches for `<regex>`
    ```
    $ git log -G <regex>
    ```

## Browsing

More ergonomic alternatives:

- `gitk` + `git-gui`
- `gitg`
- `tig`
- SourceTree
- Tower (formerly Git Tower)

## Editing

Get a decent text editor

- Spell checking
- Text formatting
- Syntax highlighting?

`commit -m` makes it hard to write good messages

## Editing

Configure editor just for Git:

```
$ git config --global core.editor <editor>
```

----------------------------------------------------------------
       Editor `<editor>`
------------- --------------------------------------------------
           ed `ed`

          Vim `vim`

 Sublime Text `"subl -n -w"`

       VSCode `"code --wait"`

         Atom `"atom --wait"`

    Notepad++ `"'C:/Program Files (x86)/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"`
----------------------------------------------------------------

## Editing

Or set `EDITOR` and `VISUAL` to benefit everywhere:

```
$ printf 'VISUAL="subl -n -w"\n' >> ~/.profile
$ printf 'export VISUAL\n' >> ~/.profile
```

## Conflict resolution

You want 3-way merging with ability to edit text manually

--------------------------------------
Platform   Meld    Beyond Compare
-------- -------- --------------------
   Linux &#10004; &#10003;

 Windows &#10003; &#10003;

   macOS &#10007; &#10003;
--------------------------------------

Use `git mergetool` to start conflict resolution

- Meld: \url{http://meldmerge.org/}
- Beyond Compare: \url{https://www.scootersoftware.com/}

## Conflict resolution

### When merging

- Conflicts resolved based on tip of all branches
- `LOCAL` is the branch merged into (ours) and `REMOTE` is the branch being
  merged in (theirs)

### When rebasing

- Conflicts resolved based on tip of destination and previously rebased commit
    - Every commit is replayed, in order, on top of destination
- `LOCAL` is the destination branch (theirs) and `REMOTE` is the branch being
  rebased (ours) -- effectively reversed

# tl;dr communicate

## tl;dr communicate

Commit log is letters to the future

> - Help yourself write them
> - Use tools better
> - Use better tools
