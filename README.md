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
#### Fastest Timings
| Stage    | Source Changed | Lockfile Changed |
| -------: | -------------: | ---------------: |
| build    | 5m 57s         | 7m 55s           |
| test     | 5m 4s          | 4m 25s           |
| package  | 1m 22s         | 1m 45s           |
| total    | 12m 52s        |                  |

### `cargo-chef` (cache to gha)
> Seems to be quite inconsistent and I haven't yet found a fix.
#### Fastest Timings
| Stage    | Source Changed | Lockfile Changed |
| -------: | -------------: | ---------------: |
| build    | 1m 23s         | 8m 28s           |
| test     | 52s            | 51s              |
| package  | 1m 15s         | 6m 58s           |
| total    | 4m 2s          |                  |

### `sccache`
#### Fastest Timings
| Stage    | Source Changed | Lockfile Changed |
| -------: | -------------: | ---------------: |
| build    | 2m 49s         | 6m 12s           |
| test     | 54s            | 53s              |
| package  | 1m 8s          | 1m 10s           |
| total    | 5m 25s         |                  |

### `sccache` + `cargo-chef` (cache to gha)
#### Fastest Timings
| Stage    | Source Changed | Lockfile Changed |
| -------: | -------------: | ---------------: |
| build    | 1m 21s         | 8m 30s           |
| test     | 48s            | 54s              |
| package  | 1m 16s         | 1m 49s           |
| total    | 3m 52s         |                  |

### `sccache` + `cargo-chef` (cache to registry - split dependencies)
#### Fastest Timings
| Stage    | Source Changed | Lockfile Changed |
| -------: | -------------: | ---------------: |
| build    | 2m 57s         | 6m 59s           |
| test     | 46s            | 54s              |
| package  | 1m 24s         | 1m 23s           |
| total    | 5m 40s         |                  |

### `sccache` + `cargo-chef` (cache to registry)
#### Fastest Timings
| Stage    | Source Changed | Lockfile Changed |
| -------: | -------------: | ---------------: |
| build    | 1m 20s         | 7m 16s           |
| test     | 52s            | 55s              |
| package  | 1m 0s          | 59s              |
| total    | 3m 43s         |                  |

