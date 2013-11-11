---
layout: post
title: A practical guide to setup Mavericks as a development machine
modified: 2013-11-10
---

This document covers setting up a development machine after a clean install of
OS X Mavericks. It's intended to provide me with a step by step guide
to setup Mavericks according to my personal needs, but in any case, it
could be useful for others too.
 
__Note__: You can expect this document to be modified/updated frequently.

## First steps

Update the system

{% highlight bash %}
$ sudo softwareupdate -iav
{% endhighlight %}

Install the Command Line Tools

{% highlight bash %}
$ xcode-select --install
{% endhighlight %}

Install [Homebrew](http://brew.sh/)

{% highlight bash %}
$ ruby -e "$(curl -fsSkL raw.github.com/mxcl/homebrew/go)"
$ brew update
{% endhighlight %}

Open `/etc/paths` and move `/usr/local/bin` to the first line, then add `/usr/local/sbin` next.

Set Zsh as default shell

{% highlight bash %}
$ sudo mv /etc/{zshenv,zshrc}
$ chsh -s /bin/zsh
{% endhighlight %}


## Homebrew packages

Install Git

{% highlight bash %}
$ brew install git
{% endhighlight %}

Install PostgreSQL

{% highlight bash %}
$ brew install postgres
$ initdb /usr/local/var/postgres -E utf8
$ ln -sfv /usr/local/opt/postgresql/*.plist ~/Library/LaunchAgents # to have launchd start postgresql at login
$ launchctl load ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist # to load postgresql now
{% endhighlight %}

  If you receive an `'psql: FATAL:  database "<user>" does not exist'` when you execute `psql`, then do

{% highlight bash %}
$ createdb
{% endhighlight %}

  Via [StackOverflow](http://stackoverflow.com/questions/17633422/psql-fatal-database-user-does-not-exist)

Install MongoDB

{% highlight bash %}
$ brew install mongodb
$ ln -sfv /usr/local/opt/mongodb/*.plist ~/Library/LaunchAgents # to have launchd start mongodb at login
$ launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mongodb.plist # to load mongodb now
{% endhighlight %}

Install Redis

{% highlight bash %}
$ brew install redis
$ ln -sfv /usr/local/opt/redis/*.plist ~/Library/LaunchAgents # to have launchd start redis at login
$ launchctl load ~/Library/LaunchAgents/homebrew.mxcl.redis.plist # to load redis now
{% endhighlight %}

Install QT, needed by Capybara for headless Javascript integration testing

{% highlight bash %}
$ brew install https://raw.github.com/cliffrowley/homebrew/patched_qt/Library/Formula/qt.rb --HEAD # the normal brew install qt didn't work at the time
{% endhighlight %}

Install [Ag](https://github.com/ggreer/the_silver_searcher) (A much faster `ack`)

{% highlight bash %}
$ brew install the_silver_searcher
{% endhighlight %}

Install OpenSSL

{% highlight bash %}
$ brew install openssl
{% endhighlight %}

Install vim

{% highlight bash %}
$ brew install vim
{% endhighlight %}

Install Tmux

{% highlight bash %}
$ brew install tmux
{% endhighlight %}

Install [ImageMagick](http://www.imagemagick.org/script/index.php)

{% highlight bash %}
$ brew install imagemagick
{% endhighlight %}

Install the [Heroku Toolbelt](https://toolbelt.heroku.com)

{% highlight bash %}
$ brew install heroku-toolbelt
{% endhighlight %}

Install the [heroku-accounts](https://github.com/ddollar/heroku-accounts) plugin to multiple accounts support

{% highlight bash %}
$ heroku plugins:install git://github.com/ddollar/heroku-accounts.git
{% endhighlight %}


## Ruby environment

Install [rbenv](https://github.com/sstephenson/rbenv), [ruby-build](https://github.com/sstephenson/ruby-build) and [rbenv-gem-rehash](https://github.com/sstephenson/rbenv-gem-rehash)

{% highlight bash %}
$ brew install rbenv ruby-build rbenv-gem-rehash
{% endhighlight %}

Add `if which rbenv > /dev/null; then eval "$(rbenv init -)"; fi` to your `.zshrc`

Install your favorite ruby version

{% highlight bash %}
$ rbenv install 2.0.0-p247
$ rbenv global 2.0.0-p247 # set as the default version to use
$ rbenv rehash # rebuild the shim executables
{% endhighlight %}

Update the local gem system

{% highlight bash %}
$ gem update —-system
{% endhighlight %}

Install bundler

{% highlight bash %}
$ gem install bundler --no-document
{% endhighlight %}


## Python environment

Install python

{% highlight bash %}
$ brew install python --with-brewed-openssl
{% endhighlight %}

Add the following to your `.zshrc`

{% highlight bash %}
export PATH=/usr/local/share/python:$PATH
{% endhighlight %}

Update Setuptools and Pip

{% highlight bash %}
$ pip install --upgrade setuptools
$ pip install --upgrade pip
{% endhighlight %}

Install [virtualenv](https://pypi.python.org/pypi/virtualenv) and [virtualenvwrapper](https://pypi.python.org/pypi/virtualenvwrapper)

{% highlight bash %}
$ pip install virtualenv virtualenvwrapper
{% endhighlight %}

Add `source /usr/local/bin/virtualenvwrapper.sh` to your `.zshrc` and then reload zsh

{% highlight bash %}
$ source ~/.zshrc
{% endhighlight %}

By default, virtualenv will create a `.virtualenv` directory in your $HOME.

