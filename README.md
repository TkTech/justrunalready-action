# JustRunAlready

GitHub Action for running the JustRunAlready (JRA) bundler on the current host
platform, optionally wrapping the bundle into a final artifact and uploading
it.

The action takes a few args:

- `config` (required): path to your `jra.toml`
- `args` (optional): extra args to pass to `jra bundle` (e.g., `--verbose`)
- `wrap` (optional): `appimage`, `dmg`, or `zip`
- `artifact_name` (optional): override artifact name
- `python-version` (optional): default `3.11`


... and has 1 (optional) output:

- `artifact_path`: resolved path of the uploaded artifact

Usage (single-platform)
```yaml
name: Bundle
on: [push]
jobs:
  bundle:
    runs-on: ubuntu-latest  # or macos-latest / windows-latest
    steps:
      - uses: actions/checkout@v4
      - uses: tktech/justrunalready-action@v1
        with:
          config: examples/minapp/jra.toml
          args: --verbose
          wrap: appimage
```

Usage (matrix for 3 OSes)
```yaml
jobs:
  bundle:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest, macos-latest, windows-latest]
        include:
          - os: ubuntu-latest
            wrap: appimage
          - os: macos-latest
            wrap: dmg
          - os: windows-latest
            wrap: zip
    steps:
      - uses: actions/checkout@v4
      - uses: tktech/justrunalready-action@v1
        with:
          config: examples/minapp/jra.toml
          wrap: ${{ matrix.wrap }}
          args: --verbose
```

