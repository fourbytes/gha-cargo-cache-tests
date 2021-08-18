# gha-cargo-cache-tests
Some experimentation on the best methods to cache Rust build dependencies on GitHub Actions. The average timings are manually recorded at the moment.

## Contributions
Any improvement suggestions or PRs for further optimisations are welcome.

## Notes
> `sccache` is setup to cache to an object storage server (for testing this is an S3 compatible object store with Vultr).
> 
> I have not included a 'no changes' test as most of my projects use the current git commit as a source for the version, so every commit results in a rebuild.

## Other Optimisations
Potential optimisations that I've yet to test:
 - Using a self-hosted actions runner with a local Redis server for sccache. Should be faster than S3.

## Caching Methods
### BuildKit Cache Mount
#### Average Timings
| Stage    | Source Changed | Lockfile Changed |
| -------: | -------------: | ---------------: |
| build    | 3m 4s          | 5m 19s           |
| test     | 2m 42s         | 3m 53s           |
| package  | 1m 56s         | 1m 23s           |

### `cargo-chef` (cache to gha)
#### Average Timings
| Stage    | Source Changed | Lockfile Changed |
| -------: | -------------: | ---------------: |
| build    | 3m 6s          | 7m 25s           |
| test     | 46s            | 54s              |
| package  | 45s            | 2m 6s            |

### `sccache`
#### Average Timings
| Stage    | Source Changed | Lockfile Changed |
| -------: | -------------: | ---------------: |
| build    | 3m 21s         | 4m 45s           |
| test     | 44s            | 51s              |
| package  | 1m 6s          | 1m 9s            |

### `sccache` + `cargo-chef` (cache to gha)
#### Average Timings
| Stage    | Source Changed | Lockfile Changed |
| -------: | -------------: | ---------------: |
| build    | 4m 14s         | 6m 26s           |
| test     | 45s            | 52s              |
| package  | 1m 18s         | 4m 46s           |

### `sccache` + `cargo-chef` (cache to registry - split dependencies)
#### Average Timings
| Stage    | Source Changed | Lockfile Changed |
| -------: | -------------: | ---------------: |
| build    | 3m 18s         | 5m 45s           |
| test     | 44s            | 51s              |
| package  | 1m 6s          | 1m 10s           |

### `sccache` + `cargo-chef` (cache to registry)
#### Average Timings
| Stage    | Source Changed | Lockfile Changed |
| -------: | -------------: | ---------------: |
| build    | 3m 39s         | 6m 13s           |
| test     | 50s            | 56s              |
| package  | 1m 9s          | 1m 15s           |

