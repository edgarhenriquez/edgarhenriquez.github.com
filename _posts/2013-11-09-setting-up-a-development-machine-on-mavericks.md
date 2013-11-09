---
layout: post
title: A practical guide to setup Mavericks as a development machine
---

This document covers setting up a development machine after a clean install of
OS X Mavericks. It's intended to provide me with a step by step guide
to setup Mavericks according to my personal needs, but in any case, it
could be useful for others too.
 
__Note__: You can expect this document to be modified/updated frequently.

## First steps

- Update the system

        $ sudo softwareupdate -iav

- Install the Command Line Tools

        $ xcode-select --install

- Install [Homebrew](http://brew.sh/)

        $ ruby -e "$(curl -fsSkL raw.github.com/mxcl/homebrew/go)"
        $ brew update

- Open `/etc/paths` and move `/usr/local/bin` to the first line, then add `/usr/local/sbin` next.

- Set Zsh as default shell

        $ sudo mv /etc/{zshenv,zshrc}
        $ chsh -s /bin/zsh


## Homebrew packages

- Install Git

        $ brew install git

- Install PostgreSQL

        $ brew install postgres
        $ initdb /usr/local/var/postgres -E utf8
        $ ln -sfv /usr/local/opt/postgresql/*.plist ~/Library/LaunchAgents # to have launchd start postgresql at login
        $ launchctl load ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist # to load postgresql now

  If you receive an `'psql: FATAL:  database "<user>" does not exist'` when you execute `psql`, then do

        $ createdb

  Via [StackOverflow](http://stackoverflow.com/questions/17633422/psql-fatal-database-user-does-not-exist)

- Install MongoDB

        $ brew install mongodb
        $ ln -sfv /usr/local/opt/mongodb/*.plist ~/Library/LaunchAgents # to have launchd start mongodb at login
        $ launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mongodb.plist # to load mongodb now

- Install Redis

        $ brew install redis
        $ ln -sfv /usr/local/opt/redis/*.plist ~/Library/LaunchAgents # to have launchd start redis at login
        $ launchctl load ~/Library/LaunchAgents/homebrew.mxcl.redis.plist # to load redis now

- Install QT, needed by Capybara for headless Javascript integration testing

        $ brew install https://raw.github.com/cliffrowley/homebrew/patched_qt/Library/Formula/qt.rb --HEAD # the normal brew install qt didn't work at the time

- Install [Ag](https://github.com/ggreer/the_silver_searcher) (A faster `ack`)

        $ brew install the_silver_searcher

- Install OpenSSL

        $ brew install openssl

- Install vim

        $ brew install vim

- Install Tmux

        $ brew install tmux

- Install [ImageMagick](http://www.imagemagick.org/script/index.php)

        $ brew install imagemagick

- Install the [Heroku Toolbelt](https://toolbelt.heroku.com)

        $ brew install heroku-toolbelt

- Install the [heroku-accounts](https://github.com/ddollar/heroku-accounts) plugin to multiple accounts support

        $ heroku plugins:install git://github.com/ddollar/heroku-accounts.git


## Ruby environment

- Install [rbenv](https://github.com/sstephenson/rbenv), [ruby-build](https://github.com/sstephenson/ruby-build) and [rbenv-gem-rehash](https://github.com/sstephenson/rbenv-gem-rehash)

        $ brew install rbenv ruby-build rbenv-gem-rehash

- Add `if which rbenv > /dev/null; then eval "$(rbenv init -)"; fi` to your `.zshrc`

- Install your favorite ruby version

        $ rbenv install 2.0.0-p247
        $ rbenv global 2.0.0-p247 # set as the default version to use
        $ rbenv rehash # rebuild the shim executables

- Update the local gem system

        $ gem update —-system

- Install bundler

        $ gem install bundler --no-document


## Python environment

- Install python

        $ brew install python --with-brewed-openssl

- Add the following to your `.zshrc`

        export PATH=/usr/local/share/python:$PATH

- Update Setuptools and Pip

        $ pip install --upgrade setuptools
        $ pip install --upgrade pip

- Install [virtualenv](https://pypi.python.org/pypi/virtualenv) and [virtualenvwrapper](https://pypi.python.org/pypi/virtualenvwrapper)

        $ pip install virtualenv virtualenvwrapper

- Add `source /usr/local/bin/virtualenvwrapper.sh'` to your `.zshrc` and then reload zsh

        $ source ~/.zshrc

  By default, virtualenv will create a `.virtualenv` directory in your $HOME.

