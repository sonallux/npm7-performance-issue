# NPM 7 performance issue reproduction

This is a reproduction of the performance issue running `npm install` in a Docker container with NPM 7.

Timings from [GitHub actions run](https://github.com/sonallux/npm7-performance-issue/actions/runs/966881282):

|                                  | Docker `node:14` | Docker `node:14-alpine` | Docker `node:16` | Docker `node:16-alpine` | Local Node 14 | Local Node 16 |
| -------------------------------- | -----------------| -----------------| -----------------| -----------------| -----------------| -----------------|
| `npm@7 i`                    | 187s | 168s | 193s | 205s | 68s | 56s |
| `npm@7 i --legacy-peer-deps` | 179s | 158s | 166s | 182s | 73s | 49s |
| `npm@6 i`                    | 70s | 63s | 64s | 61s | 54s | 51s |

From the times it is clearly visible that an `npm@7 install` on Docker takes significantly (more than 2x) longer than locally. An `npm@6 install` on Docker does not have this performance issues.

This is not an GitHub actions specific problem, I can also reproduce this behavior on Windows using Docker Desktop with WSL2 and on my Ubuntu 20.04 server.