# Intro

* This is binary.com IT team technical blog
* This blog uses jekyll and github pages
* This blog uses [Neo HPSTR Jekyll theme](https://github.com/aron-bordin/neo-hpstr-jekyll-theme)
* examples below presume that you have:
    * github account
    * installed [github's `hub`](https://github.com/github/hub)
* instead of using github's hub you can use [github](https://github.com/) website interface.
* if you have mac use [brew](brew.sh) package manager to install github's `hub`.
* This document describe one of possible ways to contribute to this technical
  blog, and of course you can do it your own way.

# How to contribute

* Fork the blog
* Write your blog post
* Run local webserver to see how it will look on live site?
* Create a PR
* Merge the PR (or ask person who has rights to do that)

## How to fork the blog?

```
git clone git@github.com:binary-com/tech.git
cd tech
git fork
```

## How to write a blog post?
Go to **_posts** directory and just create new post in **_posts** directory by coping
and edit some old post

Using existing posts as templates.

After post was created and edited:

* git add/commit it: `git add --all; git commit -m 'Add: my new awesome post'`
* push it to github `git push -u your_github_username`
* Optionally: see how will it look on live web site by serving the blog locally

## How to run local webserver to see how your blog post will look on live site?
Please keep in mind this is an optional step, it's not necessary for publishing your article.

### Installing github jekyll environment locally
You'll need:

    * installed [ruby](https://www.ruby-lang.org/en/)
    * if you use mac you probably already have [ruby](https://www.ruby-lang.org/en/) installed

Then in the tech blog directory execute:

```
sudo gem install bundler
bundle install --path vendor/bundle
```

After that you can use `bundle exec` to execute locally installed ruby gems.
I recommend to use shortcut for `bundle exec`

```
alias be="/usr/local/bin/bundle exec $@"
```

To make shortcut permanent add it to **~/.bashrc** file.

### Start local webserver

By starting following command you'll start local webserver on 4000 port.

```
be jekyll build
be jekyll serve --config _config_local.yml
```

After that just go to the browser to **http://localhost:4000** address
and look at your article.

## How to create PR

```
git pull-request -b binary-com:gh-pages
```


## How to deploy site to tech.binary.com
Keep in mind that I presume that we use [github's `hub`](https://github.com/github/hub). If you don't -
your mileage can vary.

```
git checkout gh-pages
git merge --no-ff https://github.com/binary-com/tech/pull/xxx
git push
```

# References

* [Jekyll](https://jekyllrb.com/)
* [Neo HPSTR Jekyll theme](https://github.com/aron-bordin/neo-hpstr-jekyll-theme)
* [HPSTR Jekyll theme: Setup guide](https://mmistakes.github.io/hpstr-jekyll-theme/theme-setup/)
