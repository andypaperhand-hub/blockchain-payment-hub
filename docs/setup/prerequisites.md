# Prerequisites

Install these tools before anything will run. Each name links to its official download page.

## Required

| Tool | What it is | Version |
| --- | --- | --- |
| [Node.js](https://nodejs.org/en/download) | JavaScript runtime that runs all the code | **20 or 22** (NOT 25) |
| [pnpm](https://pnpm.io/installation) | Package manager (like npm, faster) | 8 or 9 |
| [Docker Desktop](https://www.docker.com/products/docker-desktop/) | Runs the database and services in containers | Latest |
| [Git](https://git-scm.com/downloads) | Version control | Latest |

## Required for payments

| Tool | What it is | How to get it |
| --- | --- | --- |
| [MetaMask](https://metamask.io/download/) | Browser wallet extension | Install from the Chrome/Firefox store |

## Verify your install

Open a terminal and run each command — every one should print a version:

```bash
node --version    # v20.x.x or v22.x.x
pnpm --version    # 8.x.x or 9.x.x
docker --version  # 24.x.x or higher
git --version     # 2.x.x
```

{% hint style="warning" %}
**Node.js version matters.** Hardhat does not support Node.js 25. If `node --version` shows v25.x.x, install Node.js 22 LTS from [nodejs.org](https://nodejs.org).
{% endhint %}
