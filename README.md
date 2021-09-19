# NPM 7 performance issue reproduction

This is a reproduction of the performance issue running `npm install` in a Docker container with NPM 7.

Issues on npm/cli repositories
- https://github.com/npm/cli/issues/3208
- https://github.com/npm/cli/issues/2011

Timings from [GitHub actions run](https://github.com/sonallux/npm7-performance-issue/actions/runs/1250443338) reported by `time` command:

|                                   | Docker `node:14.17.6-alpine` | Docker `node:16.9.1-alpine` | Local Node 14.17.6 | Local Node 16.9.1 |
| --------------------------------- | --------------------- | -------------------- | ------------------ | ----------------- |
| `npm@7.24.0 i`                    | real 159.20s<br>user 145.12s<br>sys  63.27s | real 161.57s<br>user 158.59s<br>sys  84.43s | real 71.405s<br>user 73.660s<br>sys  9.635s | real 83.739s<br>user 59.188s<br>sys  7.438s |
| `npm@7.24.0 i --legacy-peer-deps` | real 154.69s<br>user 170.66s<br>sys  66.11s | real 135.04s<br>user 120.48s<br>sys  65.11s | real 77.029s<br>user 77.188s<br>sys  10.738s | real 64.474s<br>user 63.628s<br>sys  9.269s |
| `npm@6.14.15 i`                   | real 53.61s<br>user 52.33s<br>sys  10.19s | real 53.36s<br>user 54.13s<br>sys  11.55s | real 62.061s<br>user 61.120s<br>sys  11.000s | real 68.609s<br>user 63.741s<br>sys  10.365s |

From the times it is clearly visible that an `npm@7 install` on Docker takes significantly (~ 2x) longer than locally. An `npm@6 install` on Docker does not have this performance issues.

This is not an GitHub actions specific problem, I can also reproduce this behavior on Windows using Docker Desktop with WSL2 and on my Ubuntu 20.04 server.
