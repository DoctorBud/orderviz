## orderviz

Experiments in visualizing the orderings of posets, and their relation to the structure of their generating algorithms.


## Contributing

### Fork this repo

To effectively use the `ghpublish` capability described below, you will want to be using a fork of this repo.

- Fork the repo
- Get the URL for your fork
- `git clone` the fork using the above URL
- Add a git remote reference pointing to this repo. For example, `upstream` or `integration` could be a remote name.

### How to build

```bash
cd orderviz/
npm run dev
open https://localhost:5005 # Alternatively, type URL into a browser

```


### Publish to GitHub Pages

The `ghpublish` command will invoke `publish.sh` to deploy the current contents of `site/` to the `gh-pages` branch of this repo's `origin`.


```bash
npm run ghpublish
```

### Directory Layout

- site/ - The source for the website that will be pushed to gh-pages via publish.sh
- site/index.html - A top-level index pointing to the primary storyboard presentations, as well as possibly other resources/projects within site/.
- site/css/ - Contains smartdown-impress.css which is identical with the corresponding files in smartdown/impress. Custom per-storyboard CSS overrides should be in site/storyboard/storyboard.css.
- site/js/ - Contains impress.js, smartdown_impress.js, which are currently identical with the corresponding files in smartdown/impress.

## Versions

**0.0.1** - First draft of a multi-modal presentation about Explorating Visualizations of Order, the theme of this presentation.

