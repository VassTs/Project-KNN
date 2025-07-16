<h1 align="center">Vamana-Based Approximate Nearest Neighbor Search with Filtered Extensions</h1>

## Overview

Implementation and extension of the approximate K‑NN graph construction challenge from [SIGMOD 2023’s Programming Contest](https://people.cs.rutgers.edu/~dd903/sigmodpc2023/index.html), developed across three phases with full test coverage:
1. Core Vamana algorithm implementation  ([Subramanya et al., 2019](https://dl.acm.org/doi/abs/10.5555/3454287.3455520))  
2. Filtered extensions for label-constrained search ([Gollapudi et al., 2023](https://dl.acm.org/doi/10.1145/3543507.3583552))
3. Performance optimizations via parallelization and memory-efficient indexing

## Execution Instructions
**IMPORTANT:** All commands should be executed from the directory containing the `Makefile`.

### Makefile Commands:
- **`make`**: Compiles the executables in the `./bin/` directory.
- **`make test`**: Combines all tests into a single executable located at `./bin/test`.
- **`make groundtruth`**: Builds the `calculate_groundtruth` executable in `./bin/` and generates groundtruth for `k = 100`.
- **`make graphs`**: Builds `filtered_main_graph` and `stitched_main_graph` in `./bin/` and generates graphs and maps with parameters:
  - `L = 100`
  - `R = 96`
  - `a = 1.2`
  - `t = 55` (filtered)
  - `R_stitched = 98` (stitched)
- **`make valgrind`**: Runs `./bin/test` using `valgrind` for memory analysis.
- **`make clean`**: Deletes the following directories:
  - `./bin/` (executables)
  - `./build/` (object files)

  **Note:** This does *not* delete `graphs`, `maps`, and `groundtruth (gt)` files located in `./datasets/smallscale/`.

---

## Execution Details

### `calculate_groundtruth`
Calculates the groundtruth for a given `k` and saves the output as `gt_k=x.bin` (e.g., `gt_k=100.bin`) in `./datasets/smallscale/`.

**Usage:**
```bash
./bin/calculate_groundtruth <k>
```
Example:
```bash
./bin/calculate_groundtruth 100
```

### `filtered_main_graph`
Generates the graph and map using the **FilteredVamana** algorithm without query processing.

**Usage:**
```bash
./bin/filtered_main_graph <L> <R> <a> <t> <base_file_path>
```
Example:
```bash
./bin/filtered_main_graph 110 96 1.2 55 ./datasets/smallscale/dummy-data.bin
```

### `stitched_main_graph`
Generates the graph and map using the **StitchedVamana** algorithm without query processing.

**Usage:**
```bash
./bin/stitched_main_graph <L> <R> <a> <R_stitched> <base_file_path>
```
Example:
```bash
./bin/stitched_main_graph 110 96 1.2 98 ./datasets/smallscale/dummy-data.bin
```

### `filtered_main`
Executes the **FilteredVamana** algorithm. It can be run in three ways:

1. **Command Line Inputs:**
   ```bash
   ./bin/filtered_main <k> <L> <R> <a> <t> <base_file_path> <queries_file_path> <groundtruth_file_path>
   ```
   Example:
   ```bash
   ./bin/filtered_main 100 110 96 1.2 55 ./datasets/smallscale/dummy-data.bin ./datasets/smallscale/dummy-queries.bin ./datasets/smallscale/gt_k=100.bin
   ```

2. **Using Pre-generated Graph:**
   ```bash
   ./bin/filtered_main <k> <graph_filename> <map_filename> <queries_file_path> <groundtruth_file_path>
   ```
   Example:
   ```bash
   ./bin/filtered_main 100 filtered_graph_L=110_a=1.200000_R=96_taph=55 filtered_map_L=110_a=1.200000_R=96_taph=55 ./datasets/smallscale/dummy-queries.bin ./datasets/smallscale/gt_k=100.bin
   ```

3. **Using Configuration File:**
   ```bash
   ./bin/filtered_main <input_file>
   ```
   Example config file:
   ```txt
   k = 100
   L = 110
   R = 96
   a = 1.2
   t = 55
   base_file = ./datasets/smallscale/dummy-data.bin
   queries_file = ./datasets/smallscale/dummy-queries.bin
   groundtruth_file = ./datasets/smallscale/gt_k=100.bin
   ```
   Example execution:
   ```bash
   ./bin/filtered_main ./config-files/filtered_config.txt
   ```

### `stitched_main`
Executes the **StitchedVamana** algorithm with similar execution modes as `filtered_main`.

---

## Additional Scripts
A bash script `testing.sh` is provided to automate testing and print results to `results.txt`.

**Usage:**
```bash
chmod +x scripts/testing.sh
./scripts/testing.sh
```

---

## Authors
- [Giorgos Koryllos](https://github.com/GeorgeKorillos)
- [Theodosia Papadima](https://github.com/sulpap)
- [Vasiliki Tsantila](https://github.com/VassTs)

## License
This project is shared for educational purposes. No license is granted for commercial or non-educational use without explicit permission.