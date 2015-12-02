# Intro

* This is binary.com IT team technical blog
* This blog uses jekyll and github pages
* This blog uses [Neo HPSTR Jekyll theme](https://github.com/aron-bordin/neo-hpstr-jekyll-theme)
* examples below presume that you have:
    * github account
    * installed [github's `hub`](https://github.com/github/hub)
    * installed [ruby](https://www.ruby-lang.org/en/)
* if you use mac you probably already have [ruby](https://www.ruby-lang.org/en/) installed
* instead of using github's hub you can use [github](https://github.com/) website interface.
* if you have mac use [brew](brew.sh) package manager to install github's `hub`.
* This document describe one of possible ways to contribute to this technical
  blog, and of course you can do it your own way.

# How to contribute

* Fork the blog
* Write your blog post
* Run local webserver to see how it will look on live site?
* Create a PR
* Merge the PR
* Deploy your changes to live site

## How to fork the blog?

```
git clone git@github.com:binary-com/tech.git
cd tech
git fork
```

## How to prepare working environment?

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

## How to write a blog post?

```
be octopress new post "My Title"
```

or

```
be rake new_post["My Title"]
```

This will create template of new post in **_posts** directory.

Keep in mind that those commands just generating post by template.
Actually you can just create new post in **_posts** directory by copy
and edit way, using previous posts as a template.

After post was created:

* edit it
* push it to github `git push -u your_github_username`
* build static-website `be jekyll build`
* see how will it look on live web site by serving the blog locally

## How to run local webserver to see how your blog post will look on live site?
By starting following command you'll start local webserver on 4000 port.

```
be jekyll build
be jekyll serve --config _config_local.yml
```

After that just go to the browser to **http://localhost:4000** address
and look at your article.

## How to create PR

```
git pull-request -b binary-com:master
```

## How to merge PR
Keep in mind that I presume that we use [github's `hub`](https://github.com/github/hub). If you don't -
your mileage can vary.

```
git checkout master
git merge https://github.com/binary-com/tech/pull/xxx
```

## How to deploy site to tech.binary.com
Basically **_site** directory should go to **gh-pages** branch.

octopress provides a wrapper around that process.
But it's okay to use any other way to achieve that.

```
be jekyll build
be octopress deploy
```

# References

* [Jekyll](https://jekyllrb.com/)
* [Octopress](https://github.com/octopress/octopress)
* [Neo HPSTR Jekyll theme](https://github.com/aron-bordin/neo-hpstr-jekyll-theme)
* [HPSTR Jekyll theme: Setup guide](https://mmistakes.github.io/hpstr-jekyll-theme/theme-setup/)
