# clarotech/openEHR-docs

Documentation site for the Clarotech openEHR C# libraries, built with [DocFX v2](https://dotnet.github.io/docfx/)
and deployed to [GitHub Pages](https://clarotech.github.io/openEHR-docs/).

## Covered libraries

| Repo | NuGet package | Description |
|---|---|---|
| [clarotech/openEHR-datatypes](https://github.com/clarotech/openEHR-datatypes) | `Clarotech.OpenEHR.RM.Datatypes` | openEHR RM data types |
| [clarotech/openEHR-ehr](https://github.com/clarotech/openEHR-ehr) | `Clarotech.OpenEHR.RM` | openEHR RM structural classes |
| [clarotech/openEHR-json](https://github.com/clarotech/openEHR-json) | `Clarotech.OpenEHR.RM.Json` | JSON serialisation/deserialisation for the above two libraries |

## Local setup

### 1 — Check out sibling repos

All three library repos must be cloned **as siblings** of this repo:

```
parent-directory/
├── openEHR-docs/         ← this repo
├── openEHR-datatypes/
├── openEHR-ehr/
└── openEHR-json/         ← clone when available
```

```bash
cd ..
git clone https://github.com/clarotech/openEHR-datatypes.git
git clone https://github.com/clarotech/openEHR-ehr.git
git clone https://github.com/clarotech/openEHR-json.git   # when public
```

### 2 — Install .NET tools

```bash
cd openEHR-docs
dotnet tool restore          # installs DocFX from .config/dotnet-tools.json
```

### 3 — Restore NuGet packages

```bash
dotnet restore ../openEHR-datatypes/openEHR-datatypes/openEHR-datatypes.csproj
dotnet restore ../openEHR-ehr/openEHR-ehr/openEHR-ehr.csproj
# dotnet restore ../openEHR-json/openEHR-json/openEHR-json.csproj
```

### 4 — Build

```bash
# Generate API reference YAML + build the site
dotnet tool run docfx docfx.json

# Or run both steps separately for debugging:
dotnet tool run docfx metadata docfx.json   # → writes api/datatypes/, api/ehr/, api/json/
dotnet tool run docfx build    docfx.json   # → writes _site/
```

### 5 — Serve locally

```bash
dotnet tool run docfx serve _site
# Browse to http://localhost:8080
```

## Repository layout

```
openEHR-docs/
├── docfx.json                  DocFX build configuration
├── filter.yml                  API filter rules (exclude internal types, test classes)
├── toc.yml                     Root navigation (6 top-level sections)
├── index.md                    Landing page
├── .config/
│   └── dotnet-tools.json       Pins DocFX version (run `dotnet tool restore`)
├── .github/
│   └── workflows/
│       └── docs.yml            CI: build + deploy to gh-pages on push to main
├── docs/
│   ├── introduction/           Overview, Getting Started, openEHR Primer
│   ├── datatypes/              Guide + API ref for Clarotech.OpenEHR.RM.Datatypes
│   ├── ehr/                    Guide + API ref for Clarotech.OpenEHR.RM
│   ├── json/                   Guide + API ref for Clarotech.OpenEHR.RM.Json
│   ├── integration/            End-to-end worked examples
│   └── changelog/              Per-library release notes
├── images/
│   └── logo.svg                Placeholder logo (TODO: replace)
└── templates/
    └── clarotech/
        └── public/
            └── main.css        Custom CSS overrides (TODO: brand colours)
```

## Deployment

The GitHub Actions workflow [`.github/workflows/docs.yml`](.github/workflows/docs.yml)
runs on every push to `main` and deploys the built site to the `gh-pages` branch.

Enable GitHub Pages in the repo settings:
> **Settings → Pages → Source:** `Deploy from a branch` → branch: `gh-pages`, folder: `/ (root)`

The site will be available at: **https://clarotech.github.io/openEHR-docs/**

## Branding TODOs

- [ ] Replace `images/logo.svg` with the real Claro Consulting logo
- [ ] Add `images/favicon.ico`
- [ ] Update CSS variables in `templates/clarotech/public/main.css` with brand colours
- [ ] Update `_appFooter` in `docfx.json` with final legal copy
- [x] ~~openEHR-json metadata entry~~ — live

## Licence

Documentation content: [CC-BY 4.0](https://creativecommons.org/licenses/by/4.0/)  
Libraries: [Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0)
