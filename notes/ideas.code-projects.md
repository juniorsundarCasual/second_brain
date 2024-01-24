---
id: gqj2lcqbwu7umvdjjiwfyop
title: PKMS Server in Rust
desc: ''
updated: 1706080671278
created: 1705217843700
---

# Concept

I don't want to have to rely on VSCode of VSCodium to manage my research and notes. Having some sort of Rust implementation that acts as a server, and then integrating it with Neovim would be preferable.

And rather than using plain Zettel or Obsidian as my baseline, it would be preferable to have something similar to the way Dendron manages information in a hierarchical manner.

# Breakdown

1. In Rust, develop a server with a list of functionalities similar to Dendron:
    - Markdown based
        - Additional support for hyperlinking between texts
    - Hierarchical structure
    - Bi-directional linking
    - Tagging and keyword search
    - Graph view
    - Refactoring
    - Publishing
2. Some additional features to consider would be:
    - Graph view
    - Multi-device support
    - Multi-vault support
3. Integration into Neovim in some way
    - Traversal between notes
    - Some sort of concealer or live-view (WYSIWYG)
    - Calling all the features available in the server