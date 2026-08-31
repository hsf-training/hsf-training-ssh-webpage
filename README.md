[![HSF Training Center](https://img.shields.io/badge/HSF%20Training%20Center-browse-ff69b4)](https://hepsoftwarefoundation.org/training/curriculum.html)
# HSF Training SSH

<!-- ALL-CONTRIBUTORS-BADGE:START - Do not remove or modify this section -->
[![All Contributors](https://img.shields.io/badge/all_contributors-4-orange.svg?style=flat-square)](#contributors-)
<!-- ALL-CONTRIBUTORS-BADGE:END -->

An introduction to SSH. This repository holds the source code of the webpage that is rendered [here](https://hsf-training.github.io/hsf-training-ssh-webpage/). Contributions are welcome (see below)!

This training module is part of an initiative of the [HEP Software foundation](https://hepsoftwarefoundation.org/) to build up a full software [training curriculum](https://hepsoftwarefoundation.org/training/curriculum) for high energy physics.

**Note**: This repository has been converted from Jekyll/Carpentries to Jupyter Book.

## 🚀 Building the Book

### Local Development
**Note**: This repository is built on Jupyter Book v1. Therefore, there are some dependencies which require an older version of Python As such, it is recommended to use a virtual environment to build the Book.

0. **Install Python 3.12 (if you have a newer version)**
   Mac:
   ```bash
   brew install python@3.12
   ```

1. **Set up the virtual environment**:
   ```bash
   python3.12 -m venv .venv312
   ```

   Then source it:
   ```bash
   source .venv312/bin/activate
   ```

2. **Install Jupyter Book**:
   ```bash
   pip install "jupyter-book<1"
   ```

3. **Build the Book**
   ```bash
   jupyter-book build ssh-tutorial/
   ```

4. **View the book**: Open `_build/html/index.html` in your browser, or serve it locally:
   ```bash
   # Using Python's built-in server
   python -m http.server 8000 -d ssh-tutorial/_build/html
   ```
   Then navigate to `http://localhost:8000`

### Clean Build

To clean previous builds and rebuild from scratch:
```bash
jupyter book clean .
jupyter book build .
```

## 📝 Content Structure

- **`intro.md`**: Landing page with prerequisites
- **`01-introduction.md` through `08-tips.md`**: Main lesson chapters
- **`setup.md`**: Setup instructions
- **`reference.md`**: Quick reference guide
- **`_config.yml`**: Jupyter Book configuration
- **`_toc.yml`**: Table of contents (defines chapter order)
- **`fig/`**: Images and figures

## 🤗 Contributing

We welcome all contributions to improve the lesson! Maintainers will do their best to help you if you have any
questions, concerns, or experience any difficulties along the way.

If you make non-trivial changes (i.e., more than fixing a simple typo), you are eligible to be added to the [HSF Training Community page][hsf-training-community],
as well as to the list of contributors [below](#contributors-).

### Making Changes

1. **Fork the repository** and create a new branch
2. **Edit the markdown files** in the root directory (not in `_episodes/` - those are archived)
3. **Test your changes locally** using the build instructions above
4. **Commit your changes** with a clear message
5. **Submit a pull request** to the `jupyterbook` branch

### Pre-commit Hooks

Before committing, install the pre-commit hooks:

```bash
pip3 install pre-commit
pre-commit install
```

This will check for:
- Large files
- Merge conflicts
- Trailing whitespace
- Spelling errors (using codespell)

### Writing Tips

- Use MyST Markdown syntax for admonitions, figures, and other rich content
- See the [Jupyter Book documentation](https://jupyterbook.org/) for formatting options
- Test locally before submitting a PR
- Use the existing chapters as examples for formatting

## 🚢 Deployment

The book is automatically built and deployed to GitHub Pages via GitHub Actions when changes are pushed to the `jupyterbook` branch. The workflow is defined in `.github/workflows/deploy.yml`.

## 📚 Jupyter Book vs Jekyll

This repository was previously built with Jekyll and the Carpentries template. It has been migrated to Jupyter Book for:
- Better support for modern web technologies
- More flexible content formatting with MyST Markdown
- Improved navigation and search
- Integration with Python/Jupyter ecosystem

Old Jekyll files are archived in `_jekyll_archive/` for reference.

## Authors

Thanks goes to these wonderful people ([emoji key](https://allcontributors.org/docs/en/emoji-key)) who contributed to
the content of the lesson:

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tr>
    <td align="center"><a href="https://github.com/daritter"><img src="https://avatars.githubusercontent.com/u/1186338?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Martin Ritter</b></sub></a><br /><a href="#content-daritter" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/meliache"><img src="https://avatars.githubusercontent.com/u/5121824?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Michael Eliachevitch</b></sub></a><br /><a href="#content-meliache" title="Content">🖋</a></td>
    <td align="center"><a href="https://www.lieret.net"><img src="https://avatars.githubusercontent.com/u/13602468?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Kilian Lieret</b></sub></a><br /><a href="#content-klieret" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/936-BCruz"><img src="https://avatars.githubusercontent.com/u/64757758?v=4?s=100" width="100px;" alt=""/><br /><sub><b>936-BCruz</b></sub></a><br /><a href="#content-936-BCruz" title="Content">🖋</a></td>
  </tr>
</table>

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->

<!-- ALL-CONTRIBUTORS-LIST:END -->

Even more people contributed to the framework, but they are too many to list!


[hsf-training-community]: https://hepsoftwarefoundation.org/training/community
[hsf-training-center]: https://hepsoftwarefoundation.org/training/curriculum.html
