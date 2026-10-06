# sdlc-docs

Engineering platform modernization and AI-ready SDLC documentation, published as a [Hugo](https://gohugo.io/) site using the [Hextra](https://imfing.github.io/hextra/) theme.

## Prerequisites

- Hugo (v0.146+)
- Go (the theme is pulled in as a Hugo module)

## Local preview

```sh
hugo server          # http://localhost:1313
hugo server -D       # include draft pages
```

## Build

```sh
hugo --gc --minify   # output in public/
```

## Layout

```
content/
├── _index.md          # Home page
└── docs/
    ├── strategy/      # Vision, principles, roadmap
    ├── sdlc/          # SDLC phases, controls, standards
    └── tooling/       # Tooling evaluation and selection
```

Each folder needs an `_index.md` to show up as a sidebar section. Pages are ordered by `weight` in their front matter. Pages marked `draft: true` are only shown with `-D`.
