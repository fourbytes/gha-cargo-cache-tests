# gha-cargo-cache-tests
Some experimentation on the best methods to cache Rust build dependencies on GitHub Actions. The average timings are manually recorded at the moment.

## Contributions
Any improvement suggestions or PRs for further optimisations are welcome.

## Notes
> `sccache` is setup to cache to an object storage server (for testing this is an S3 compatible object store with Vultr).
> 
> I have not included a 'no changes' test as most of my projects use the current git commit as a source for the version, so every commit results in a rebuild.

## Caching Methods
### `sccache`
#### Average Timings
| Stage    | Source Changed | Lockfile Changed |
| -------: | -------------: | ---------------: |
| build    | 3m 21s         | 5m 32s           |
| test     | 44s            | 52s              |
| package  | 1m 6s          | 52s              |

### `cargo-chef` (cache to gha)
#### Average Timings
| Stage    | Source Changed | Lockfile Changed |
| -------: | -------------: | ---------------: |
| build    | 3m 6s          | 7m 20s           |
| test     | 46s            | 50s              |
| package  | 45s            | 52s              |

### BuildKit Cache Mount
#### Average Timings
| Stage    | Source Changed | Lockfile Changed |
| -------: | -------------: | ---------------: |
| build    | 3m 4s          | 7m 20s           |
| test     | 2m 42s         | 50s              |
| package  | 1m 56s         | 52s              |

### `sccache` + `cargo-chef` (cache to registry)
#### Average Timings
| Stage    | Source Changed | Lockfile Changed |
| -------: | -------------: | ---------------: |
| build    | 3m 39s         | 7m 20s           |
| test     | 50s            | 50s              |
| package  | 1m 9s          | 52s              |

### `sccache` + `cargo-chef` (cache to registry - split dependencies)
#### Average Timings
| Stage    | Source Changed | Lockfile Changed |
| -------: | -------------: | ---------------: |
| build    | 3m 18s         | 7m 20s           |
| test     | 44s            | 50s              |
| package  | 1m 6s          | 52s              |

### `sccache` + `cargo-chef` (cache to gha)
#### Average Timings
| Stage    | Source Changed | Lockfile Changed |
| -------: | -------------: | ---------------: |
| build    | 4m 14s         | 7m 20s           |
| test     | 45s            | 50s              |
| package  | 1m 18s         | 52s              |
