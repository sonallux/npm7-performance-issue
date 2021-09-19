# NPM 7 performance issue reproduction

This is a reproduction of the performance issue running `npm install` in a Docker container with NPM 7.

Issues on npm/cli repositories
- https://github.com/npm/cli/issues/3208
- https://github.com/npm/cli/issues/2011

Timings in seconds from [GitHub actions run](https://github.com/sonallux/npm7-performance-issue/actions/runs/1251071846) reported by `time` command:



|                                   | | Docker `node:14.17.6` | Docker `node:16.9.1` | Docker `node:14.17.6-alpine` | Docker `node:16.9.1-alpine` | Local Node 14.17.6 | Local Node 16.9.1 |
| --------------------------------- | - | --------------------- | -------------------- | ------------------ | ----------------- | ------------------ | ----------------- |
| `npm@7.24.0 i`                    | real<br>user<br>sys | 140.73<br>143.66<br>68.16 | 128.99<br>118.99<br>45.49 | 150.36<br>145.09<br>66.08 | 163.99<br>139.65<br>76.43 | 74.526<br>75.698<br>10.581 | 67.379<br>66.108<br>9.808 |
| `npm@7.24.0 i --legacy-peer-deps` | real<br>user<br>sys | 141.48<br>136.63<br>58.41 | 153.14<br>132.21<br>58.17 | 121.43<br>124.17<br>62.29 | 158.33<br>143.17<br>76.08 | 61.892<br>63.315<br> 8.508 | 81.233<br>66.614<br>8.595 |
| `npm@6.14.15 i`                   | real<br>user<br>sys |  59.06<br> 58.23<br>12.44 |  67.86<br> 63.17<br>12.14 |  60.94<br> 64.26<br>13.07 |  59.80<br> 61.39<br>13.69 | 61.310<br>60.126<br>11.312 | 61.416<br>58.122<br>9.296 |

From the times it is clearly visible that an `npm@7 install` on Docker takes significantly (~ 2x) longer than locally. An `npm@6 install` on Docker does not have this performance issues.

This is not an GitHub actions specific problem, I can also reproduce this behavior on Windows using Docker Desktop with WSL2 and on my Ubuntu 20.04 server.
