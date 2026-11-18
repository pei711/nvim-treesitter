# Troubleshooting

Start with `:checkhealth nvim-treesitter`. It reports installed parsers and queries, and points out common setup problems.

## A parser will not install or update

This branch requires Neovim 0.12 or later, `curl`, `tar`, a C compiler, and `tree-sitter-cli` 0.26.1 or later. Install the CLI with your system package manager; the project does not support installing it through npm. See the [requirements](../README.md#requirements) for details.

Read `:TSLog` for messages from the last parser operation. Confirm that the language is listed in [SUPPORTED_LANGUAGES.md](../SUPPORTED_LANGUAGES.md), then try `:TSInstall <language>` or `:TSUpdate <language>`. Both commands run asynchronously. If a parser is already installed but appears stale or broken, `:TSInstall! <language>` forces a fresh installation.

If you configured a custom `install_dir`, check that it is the directory you intended. The setup option prepends that directory to Neovim's `runtimepath`; the default is `stdpath('data') .. '/site'`.

## A Treesitter feature is not active

Installing a parser does not enable features automatically. Check that the language has the relevant query in `queries/<language>` and that your Neovim configuration enables the feature. For example, highlighting requires calling `vim.treesitter.start()` for the buffer's filetype. The [supported features section](../README.md#supported-features) has setup examples for highlighting, folds, and indentation.

## Highlighting or another query behaves unexpectedly

Check that the language parser is installed and that the corresponding query is available on `runtimepath`. Queries earlier on `runtimepath` take precedence unless they use the `; extends` modeline. See `:h treesitter-query-modelines` for how query files are combined.

## Still stuck?

Include the output of `:checkhealth nvim-treesitter` and `nvim --version`, the language and feature involved, and the smallest steps that reproduce the problem. If parser installation failed, include the relevant `:TSLog` output as well.
