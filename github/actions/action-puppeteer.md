# GitHub Actions Puppeteer

## Workflow

```yaml
---
name: E2E Tests

on:
  workflow_dispatch:
  schedule:
    - cron: 30 1 * * *

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2

      - name: Use Node.js 14
        uses: actions/setup-node@v2
        env:
          PUPPETEER_SKIP_CHROMIUM_DOWNLOAD: 'true'
        with:
          node-version: 14

      - name: Get yarn cache directory path
        id: yarn-cache-dir-path
        run: echo "::set-output name=dir::$(yarn config get cacheFolder)"

      - uses: actions/cache@v2
        id: yarn-cache
        with:
          path: ${{ steps.yarn-cache-dir-path.outputs.dir }}
          key: ${{ runner.os }}-yarn-${{ hashFiles('**/yarn.lock') }}
          restore-keys: |
            ${{ runner.os }}-yarn-

      - name: Installing dependencies
        run: yarn install --frozen-lockfile

      - name: Run E2E tests
        uses: mujo-code/puppeteer-headful@v2
        env:
          CI: 'true'
          TEST_PASS: ${{ secrets.TEST_PASS }}
          TEST_SEED: ${{ secrets.TEST_SEED }}
          TEST_SEED2: ${{ secrets.TEST_SEED2 }}
          TEST_PKEY: ${{ secrets.TEST_PKEY }}
          TEST_ACCOUNT2_PUB_KEY: ${{ secrets.TEST_ACCOUNT2_PUB_KEY }}
        with:
          args: yarn e2e
```
