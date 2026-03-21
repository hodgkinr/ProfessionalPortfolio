# Local Preview for This Website

This repo is a GitHub Pages/Jekyll-style site published from the `docs/` folder.  
It does **not** currently include a local Ruby/Bundler setup (`Gemfile` is missing), so the simplest way to preview it locally is with Docker.

## Requirements

- Docker installed and running

## Start the local preview

From the repo root, run:

```bash
cd /path/to/ProfessionalPortfolio && docker run --rm -it -p 4000:4000 -v "$PWD/docs":/srv/jekyll jekyll/jekyll:pages bash -lc "gem install webrick && jekyll serve --host 0.0.0.0 --force_polling"
````

## View the site

Open this in your browser:

```text
http://127.0.0.1:4000
```

## Stop the preview server

In the terminal where the server is running, press:

```bash
Ctrl+C
```

## Notes

* The site source is the `docs/` directory.
* The Jekyll config file is `docs/_config.yml`.
* This approach avoids needing to install or configure Ruby/Jekyll locally on the machine.
* If Docker pulls the image the first time, that is normal and may take a minute.

```
