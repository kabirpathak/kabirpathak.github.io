### Overview: 
Hi, this is my blog.
This has been created and hosted using Jekyll and github-pages respectively.

I've used mediumish theme, with some changes.

You can check out my website here: https://kabirpathak.github.io

### Things to remember:
I've added these steps for myself, in case I ever stop working on this website for a long time and then don't remember
what process I used to follow. Don't judge me.

- New post goes in _posts, new pages in _pages (like `about`).
- Layouts work like they would in Rails.
- _includes contains theme specific features like ratings, search, etc.

- If new post is in the future, it won't show up until that time is reached. This can be changed by adding `future: true` in _config.yml but i haven't tried it yet.

### Dev env setup:
- git clone
- bundle install
- bundle exec jekyll serve

Also, looks like jekyll serve runs in watch mode. So, i *can* run a server while simultaneously making code changes.
---
