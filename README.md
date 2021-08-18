# gha-cargo-cache-tests
Some experimentation on the best methods to cache Rust build dependencies on GitHub Actions.

## Notes
> `sccache` is setup to cache to an object storage server (for testing this is an S3 compatible object store with Vultr).
> I have not included a 'no changes' test as most of my projects use the current git commit as a source for the version, so every commit results in a rebuild.

## Caching Methods
### `sccache`
| Stage | Source Changed | Lockfile Changed |
| ----: | -------------: | ---------------: |
| build | 3m 30s         | 5m 32s           |
| test  | 54s            | 52s              |
### `sccache` + `cargo-chef` (cache to registry)
### `cargo-chef` (cache to gha)
| Stage | Source Changed | Lockfile Changed |
| ----: | -------------: | ---------------: |
| build | 1m 55s         | 7m 20s           |
| test  | 49s            | 50s              |
### Docker BuildKit cache mount
| Stage | Source Changed | Lockfile Changed |
| ----: | -------------: | ---------------: |
| build | 5m 42s         | 6m 28s           |
| test  | 3m 34s         | 2m 25s           |
