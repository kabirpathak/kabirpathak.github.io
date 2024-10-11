### Overview: 
Hi, this is my blog.
This has been created and hosted using Jekyll and github-pages respectively.
I've used the [mediumish](https://github.com/wowthemesnet/mediumish-theme-jekyll), with some changes.

Hosted here: https://kabirpathak.github.io

### Things to remember:
I've added these steps for myself, in case I ever stop working on this website for a long time and then don't remember
what process I used to follow. Don't judge me.

- New post goes in `_posts`, new pages in `_pages` (like `about`).
- `_layouts` work like they would in Rails.
- `_includes` contains theme specific features like ratings, search, etc.
- Main branch is `gh-pages` and not `main` or `master`. 
- If new post is in the future, it won't show up until that time is reached. This can be changed by adding `future: true` in `_config.yml` but i haven't tried it yet.

### Dev env setup:
- `git clone https://github.com/kabirpathak/kabirpathak.github.io.git`
- `bundle install`
- `bundle exec jekyll serve`

Also, looks like jekyll serve runs in watch mode. So, i *can* run a server while simultaneously making code changes.

---
