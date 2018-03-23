---
title: "Atom Text Editor"
date: 2017-07-25T00:20:00+08:00
categories: [Software]
tags: [Atom]
toc: true
math: false
---

# [Official Website](https://atom.io/)

## Popular Community Packages

### [atom-beautify](https://atom.io/packages/atom-beautify)

Beautify HTML, CSS, JavaScript, PHP, Python, Ruby, Java, C, C++, C#, Objective-C, CoffeeScript, TypeScript, Coldfusion, SQL, and more in Atom.

*   Installation:
    *   Terminal: ``apm install atom-beautify``
    *   Built-in package manager
*   Usage:
    1.  Right click and select "Beautify editor contents".
    2.  Scope: selected contents or the whole file if not.
*   [My uncrustify.cfg](https://gist.github.com/Superdanby/40de920a0e94c2e8b8389b2a0a34765b)

### [autoclose-html](https://atom.io/packages/autoclose-html)

Will automatically add closing tags when you complete the opening tag.

*   Installation:
    *   Terminal: ``apm install autoclose-html``
    *   Built-in package manager

### [autocomplete-clang](https://atom.io/packages/autocomplete-clang)

This package provides completions by clang like emacs auto-complete-clang.el using autocomplete-plus.

*   Installation:
    *   Terminal: ``apm install autocomplete-clang``
    *   Built-in package manager

### [autocomplete-python](https://atom.io/packages/autocomplete-python)

Python packages, variables, methods and functions with their arguments autocompletion in Atom powered by your choice of Jedi or Kite.

*   Installation:
    *   Terminal: ``apm install autocomplete-python``
    *   Built-in package manager

### [color-picker](https://atom.io/packages/color-picker)

Currently reads `HEX`, `HEXa`, `RGB`, `RGBa`, `HSL`, `HSLa`, `HSV`, `HSVa`, `VEC3` and `VEC4` colors – and is able to convert between the formats.

*   Installation:
    *   Built-in package manager
*   Usage:
    1.  Point to the color code you want to modify.
    2.  Right click and select "Color Picker", or hit `CTRL-ALT-C`.

### [file-icons](https://atom.io/packages/file-icons)

File-specific icons in Atom for improved visual grepping.

*   Installation:
    *   Terminal: ``apm install file-icons``
    *   Built-in package manager

### [fonts](https://atom.io/packages/fonts)

Choose your favorite fonts.

*   Installation:
    *   Built-in package manager
*   Usage:
    1.  Contents: Choose your desired fonts from its settings page
    2.  UI:
        1.  Open "Config Folder" under Settings.
        2.  Add ``atom-workspace { font-family: Desired_Font; }`` to your style.less file.

### [git-plus](https://atom.io/packages/git-plus)

Git integration. Do git things inside Atom.

*   Installation:
    *   Built-in package manager
*   Usage:
    *   Show Git-Plus Palette: `Ctrl-Shift-H`

**Tips:**

1.  [This package is available only if you are using ssh and don't prompt for password every session.](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)

2.  If Your project folder is at root git-plus can detect it automatically.

3.  You can add multiple project folders at a time.

### [highlight-selected](https://atom.io/packages/highlight-selected)
Highlight strings matching the selected text throughout the file.

*   Installation:
    *   Built-in package manager
*   Usage:
    *   Double click or select the word you want to Highlight.

### [linter](https://atom.io/packages/linter)

A real time debugger base. It provides a top-level API to its consumer that allows them to visualize errors and other kind-of messages, easily.

*   Installation:
    *   Built-in package manager

You'll have to install the linter for the specific language you want, like [linter-flake8](https://atom.io/packages/linter-flake8), [linter-gcc](https://atom.io/packages/linter-gcc), etc.

**About [Lint](https://en.wikipedia.org/wiki/Lint_(software))** and **[Heisenbug](https://en.wikipedia.org/wiki/Heisenbug)**

### [markdown-themeable-pdf](https://atom.io/packages/markdown-themeable-pdf)

Themeable markdown converter (Print to PDF, HTML, JPEG or PNG)

*   Installation:
    *   Terminal: ``apm install markdown-themeable-pdf``
    *   Built-in package manager
*   Usage:
    *   Right click and select Markdown to PDF

### [mininap](https://atom.io/packages/minimap)

A preview of the full source code.

*   Installation:
    *   Terminal: ``apm install minimap``
    *   Built-in package manager

### [minimap-find-and-replace](https://atom.io/packages/minimap-find-and-replace)

Displays the search matches in the minimap.

*   Installation:
    *   Built-in package manager

### [minimap-git-diff](https://atom.io/packages/minimap-git-diff)

A minimap binding for the Atom git-diff package.

*   Installation:
    *   Built-in package manager

### [minimap-highlight-selected](https://atom.io/packages/minimap-highlight-selected)

A minimap binding for the highlight-selected package.

*   Installation:
    *   Built-in package manager

### [minimap-linter](https://atom.io/packages/minimap-linter)

Atom package to display linter markers on minimap.

*   Installation:
    *   Built-in package manager

### [script](https://atom.io/packages/script)

Run scripts based on file name, a selection of code, or by line number.

*   Installation:
    *   Terminal: ``apm install script``
    *   Built-in package manager

### [sync-settings](https://atom.io/packages/sync-settings)

Synchronize settings, keymaps, user styles, init script, snippets and installed packages across Atom instances.

*   Installation:
    *   Terminal: ``apm install sync-settings``
    *   Built-in package manager
*   [Setup](https://atom.io/packages/sync-settings#setup)
*   Usage:
    *   Backup: `sync-settings:backup`
    *   Restore: `sync-settings:restore`
