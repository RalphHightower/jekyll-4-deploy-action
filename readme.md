## Jekyll 4 Deploy Action for GitHub Pages

Deploy Jekyll site(s) into `gh-pages` using this custom GitHub Action.

A GitHub Action for building and deploying a Jekyll site into a `gh-pages` branch.

### Prerequisites

1. Create the nested folder `.github/workflows` at the base of your Jekyll project including the dot.
2. Create `jekyll.yml` inside the folder above with the following:

```yaml
name: Jekyll Deploy

on:
  push:
    branches:
      - master

jobs:
  build_and_deploy:
    runs-on: ubuntu-latest
    steps:
      - name: GitHub Checkout
        uses: actions/checkout@v1
      - name: Bundler Cache
        uses: actions/cache@v1
        with:
          path: vendor/bundle
          key: ${{ runner.os }}-gems-${{ hashFiles('**/Gemfile.lock') }}
          restore-keys: |
            ${{ runner.os }}-gems-
      - name: Build & Deploy to GitHub Pages
        uses: 0xnu/jekyll-4-deploy-action@master
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          GITHUB_REPOSITORY: ${{ secrets.GITHUB_REPOSITORY }}
          GITHUB_ACTOR: ${{ secrets.GITHUB_ACTOR }}
```

### Build Directory

Set `JEKYLL_DESTINATION` with a variable if you don't want to `_site` as your default directory.

```yaml
name: Jekyll Deploy

on:
  push:
    branches:
      - master

jobs:
  build_and_deploy:
    runs-on: ubuntu-latest
    steps:
      - name: GitHub Checkout
        uses: actions/checkout@v1
      - name: Bundler Cache
        uses: actions/cache@v1
        with:
          path: vendor/bundle
          key: ${{ runner.os }}-gems-${{ hashFiles('**/Gemfile.lock') }}
          restore-keys: |
            ${{ runner.os }}-gems-
      - name: Build & Deploy to GitHub Pages
        uses: 0xnu/jekyll-4-deploy-action@master
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          GITHUB_REPOSITORY: ${{ secrets.GITHUB_REPOSITORY }}
          GITHUB_ACTOR: ${{ secrets.GITHUB_ACTOR }}
          JEKYLL_DESTINATION: prod
```

### Known Issues

Exclude `vendor` directory from being built in `_config.yml` so it doesn't break the build as it suggests you put gems in `./vendor/bundle`

```yaml
# Excludes
exclude: ["vendor"]
```

### License

This project is licensed under the [WTFPL License](LICENSE) - see the file for details.

### Copyright

(c) 2020 [Finbarrs Oketunji](https://finbarrs.eu). 🐼
