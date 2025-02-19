<img src="public/images/nf-core-logo.svg#gh-light-mode-only" width="400">
<img src="public/images/nf-core-logo-darkbg.svg#gh-dark-mode-only" width="400">

# [nf-co.re](https://github.com/nf-core/nf-co.re)

This repository contains code for the nf-core website: **<http://nf-co.re/>**

## Packages used

Here's how the website is built:

- Language: Javascript
- Frameworks:
  - [Astro](https://astro.build/) (static site generator),
  - [Svelte](https://svelte.dev/) (interactive components),
  - [Bootstrap 5.3](https://getbootstrap.com/docs/5.3/) (CSS framework)
- Tools:
  - [npm](https://www.npmjs.com/) (package manager)

## Development

### Getting the code

To make edits to the website, fork the repository to your own user on GitHub and then clone to your local system.

```bash
gh repo fork nf-core/nf-co.re
cd nf-co.re/
```

### Installing dependencies

The website is built using [Astro](https://astro.build/), a static site generator.
To install the dependencies, run:

```bash
npm install
```

### Running a local server

Ok, you're ready! To run the website locally, just start astro dev mode:

```bash
npm run dev
```

You should then be able to access the website in your browser at [http://localhost:3000/](http://localhost:3000/).

### File structure

We follow Astro's [file structure](https://docs.astro.build/guides/project-structure) for the website.
The main files are:

- `src/pages/` - Astro pages
- `src/content/` - [Astro content collections](https://docs.astro.build/en/guides/content-collections/) (markdown files for events, docs)
- `src/components/` - Astro/Svelte components
- `src/layouts/` - HTML layouts
- `src/styles/` - (S)CSS stylesheets
- `public/` - Static files (images, json files etc)

### Updating the JSON files and cached markdown

Much of the site is powered by the JSON files in `/public` and the cached markdown files (from the pipeline docs) in `/.cache`.

They come pre-built with the repository, but if you want to rebuild them then you'll need to run the following commands. Note that you need to add a GITHUB_TOKEN inside a `.env` file to avoid hitting API limits (too early). See [instructions on how to get a GitHub OAuth token](https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line) (the token only needs the `public_repo` permission).

```bash
npm run build-pipeline-json
npm run build-component-json
npm run build-cache-force
```

<!-- ## Production Server Setup

### Deployment

The website is deployed via GitHub Actions ([`.github/workflows/web-deploy.yml`](https://github.com/nf-core/nf-co.re/blob/master/.github/workflows/web-deploy.yml)).
This script runs PHP composer and npm, then syncs the required files to the web server via FTP. -->

### Tools API docs

Tools docs are built using GitHub Actions on the nf-core/tools repo using Sphinx.
[These actions](https://github.com/nf-core/tools/blob/master/.github/workflows/tools-api-docs-release.yml) sync the built HTML files via FTP.

<!-- ### GitHub web hooks

There is a GitHub web hook at the nf-core organisation level which triggers the pipeline update script whenever a repo is created, or has a release etc.
This pings the `deploy_pipelines.php` script. -->

<!-- ### Stats cronjob

The web server needs the following cronjobs running to scrape statistics and udates:

```cron
0 0 * * * /usr/local/bin/php /path/to/deployment/update_stats.php >> /home/nfcore/update.log 2>&1
0 2 * * * /usr/local/bin/php /path/to/deployment/update_issue_stats.php >> /home/nfcore/update.log 2>&1
0 0 * * 0 /usr/local/bin/php /path/to/deployment/update_fontawesome_icons.php >> /home/nfcore/update.log 2>&
```

Remember to replace `/path/to/deployment/` with your actual deployment directory.

The `update_issue_stats.php` script can use a lot of GitHub API calls, so should run at least one hour after the `update_stats.php` script last finished.
This is not because the script takes an hour to run, but because the GitHub API rate-limiting counts the number of calls within an hour. -->

## Contribution guidelines

If you are looking forward to contribute to the website or add your institution to the official list of contributors, please have a look at the [CONTRIBUTING.md](./.github/CONTRIBUTING.md).

## Community

If you have any questions or issues please send us a message on [Slack](https://nf-co.re/join/slack).

## Credits

Phil Ewels ([@ewels](http://github.com/ewels/)) built the initial website, but there have been many contributors to the content and documentation.
Matthias Hörtenhuber ([@mashehu](https://github.com/mashehu)) worked on the concept and code for the new website rewrite.

See the [repo contributors](https://github.com/nf-core/nf-co.re/graphs/contributors) for more details.

Kudos to the excellent [npm website](https://www.npmjs.com), which provided inspiration for the design of the pipeline pages.
