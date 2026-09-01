# kyledawson.github.io

Personal website of **Kyle W. Dawson, PhD** — Lead Data Scientist, Geospatial
Intelligence at CenterPoint Energy. Built with [Jekyll](https://jekyllrb.com/) and
deployed to GitHub Pages via GitHub Actions.

Live site: <https://kyledawson.github.io/>

## Local development

Requires Ruby and Bundler.

```bash
bundle install            # install gems (Jekyll + plugins)
bundle exec jekyll serve  # preview at http://127.0.0.1:4000
bundle exec jekyll build  # one-off build into _site/
```

Most content lives in Markdown/HTML pages at the repo root (`index.md`, `landing.md`,
`resume.md`, `publications.md`, `projects.md`), blog posts in `_posts/`, and site-wide
settings in `_config.yml`. The homepage tiles and contact block are driven by
`_config.yml` (`tiles-source`, `tiles-count`, address/email, and `socials`).

## Development workflow

Changes are made on branches and beta-tested before they go live. Never commit
directly to `master`.

1. **Branch** off `master`:

   ```bash
   git checkout master && git pull
   git checkout -b feat/my-change   # or fix/…
   ```

2. **Build & preview locally** with `bundle exec jekyll serve` and iterate.

3. **Open a pull request** into `master`. The **Build check** workflow
   (`.github/workflows/ci.yml`) builds the site on every PR — this is the gate that
   catches broken Liquid, bad front matter, or missing files before they can ship.

4. **Merge** once the check is green. Merging to `master` triggers
   `.github/workflows/deploy.yml`, which builds with Jekyll and publishes to GitHub
   Pages via the official `actions/deploy-pages`.

### One-time repository setup

These live in the GitHub UI and can't be committed to the repo:

- **Settings → Pages → Build and deployment → Source:** set to **GitHub Actions**
  (not "Deploy from a branch"). Required for `deploy.yml` to publish.
- **Settings → Branches → Add branch protection rule** for `master`: require a pull
  request and require the **Build check** status check to pass before merging.

  The equivalent via the GitHub CLI:

  ```bash
  gh api -X PUT repos/kyledawson/kyledawson.github.io/branches/master/protection \
    -F required_pull_request_reviews.required_approving_review_count=0 \
    -F 'required_status_checks.strict=true' \
    -F 'required_status_checks.contexts[]=build' \
    -F enforce_admins=false -F restrictions=
  ```

Once the Actions-based deploy is confirmed working, the legacy `gh-pages` branch is no
longer used and can be deleted:

```bash
git push origin --delete gh-pages
```

## Credits

Theme: [Forty](https://html5up.net/forty) by [HTML5 UP](https://html5up.net)
(CCA 3.0), with a Jekyll port by
[Andrew Banchich](https://github.com/andrewbanchich/forty-jekyll-theme).
