[![HSF Training Center](https://img.shields.io/badge/HSF%20Training%20Center-browse-ff69b4)](https://hepsoftwarefoundation.org/training/curriculum.html)
HSF Training SSH
==============

<!-- ALL-CONTRIBUTORS-BADGE:START - Do not remove or modify this section -->
[![All Contributors](https://img.shields.io/badge/all_contributors-4-orange.svg?style=flat-square)](#contributors-)
<!-- ALL-CONTRIBUTORS-BADGE:END -->

An introduction to SSH. This repository holds the source code of the webpage that is rendered [here](https://hsf-training.github.io/hsf-training-ssh-webpage/). Contributions are welcome (see below)!

This training module is part of an initiative of the [HEP Software foundation](https://hepsoftwarefoundation.org/) to build up a full software [training curriculum](https://hepsoftwarefoundation.org/training/curriculum) for high energy physics.

## 🤗 Contributing

<!-- CENTRALLY MAINTAINED SECTION -->
<!-- Remove the above marker to disable having this section be overwritten -->

We welcome all contributions to improve the lesson! Maintainers will do their best to help you if you have any
questions, concerns, or experience any difficulties along the way.

If you make non-trivial changes (i.e., more than fixing a simple typo), you are eligible to be added to the [HSF Training Community page][hsf-training-community],
as well as to the list of contributors [below](#contributors-).

We'd like to ask you to familiarize yourself with our [Contribution Guide](CONTRIBUTING.md) and have a look at
the [more detailed guidelines][lesson-example] on proper formatting, ways to render the lesson locally, and even
how to write new episodes.

Quick summary of how to get a local preview: Install [jekyll][jekyll] and then run

```
bundle install
bundle update
bundle exec jekyll serve
```

Unless we change framework versions, only the last command needs to be typed after the first time.

Before committing anything, we also ask you to install the [pre-commit][pre-commit] hooks of this repository:

```bash
pip3 install pre-commit
pre-commit install
```

Please see the current list of [issues][issues] for ideas for contributing to this
repository. For making your contribution, we use the GitHub flow, which is
nicely explained in the chapter [Contributing to a Project][progit] in Pro Git
by Scott Chacon.
Look for the tag ![good_first_issue][gfi-badge], which marks particularly simple issues to get you started.

<!-- END CENTRALLY MAINTAINED SECTION -->

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


[lesson-example]: https://carpentries.github.io/lesson-example
[pre-commit]: https://pre-commit.com/
[hsf-training-community]: https://hepsoftwarefoundation.org/training/community
[hsf-training-center]: https://hepsoftwarefoundation.org/training/curriculum.html
[training-center-badge]: https://img.shields.io/badge/HSF%20Training%20Center-browse-ff69b4
[schools]: https://hepsoftwarefoundation.org/Schools/events.html
[issues]: https://github.com/hsf-training/hsf-training-reana-webpage/issues
[progit]: http://git-scm.com/book/en/v2/GitHub-Contributing-to-a-Project
[jekyll]: https://jekyllrb.com/
[allcontrib-emoji-key]: https://allcontributors.org/docs/en/emoji-key
[gfi-badge]: https://img.shields.io/badge/-good%20first%20issue-gold.svg
[schools-badge]: https://img.shields.io/badge/upcoming%20events-browse-ff69b4
[twitter-badge]: https://img.shields.io/twitter/follow/hsftraining?style=social
[twitter]: https://twitter.com/hsftraining
