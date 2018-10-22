# Rails Quick Start Guide
A quick-start guide to getting a Ruby on Rails env setup on a mac.

## Prerequisites
  - [Homebrew](http://brew.sh/)

## Index
- [Ruby Management](#ruby-management)
  - [RBENV](#rbenv)
- [Gem Management](#gem-management)
  - [Bundler](#bundler)
- [Databases](#databases)
  - [MySql](#mysql)
  - [Postgres](#postgres)
  - [GUI Apps](gui-apps)
- [Rails Server](#rails-server)
  - [Pow](#pow)


## Ruby Management
Every Ruby on Rails app relies on a specific version of Ruby. The particular version depends on the time the Rails app was started and the dependencies it had at that time. Due to the vast array of Ruby versions it's common practice to use a Ruby Version Manager to maintain multiple ruby gemsets. My recommendation is [rbenv](https://github.com/rbenv/rbenv).

For those interested [RVM](https://rvm.io/) is another option. RVM is largely the other side of the same coin, some prefer it as it has far more options and configurations but with configuration come complication as well. In the end they each achieve the same goal of allowing you to manage multiple sets or grouping of gems locked in a specific version of ruby.

### [RBENV](https://github.com/rbenv/rbenv)

#### Installation
- Install rbenv via Homebrew: `brew install rbenv`.
- Download a version of Ruby via rbenv (e.g., `rbenv install 2.2.3`). See <https://gorails.com/setup/osx/10.11-el-capitan>.
- Make it the global version of Ruby: `rbenv global 2.2.3`.
- It's that easy! For more info on how to use rbenv check out their [reamdme](https://github.com/rbenv/rbenv#how-it-works)

*Installing and managing Ruby with rbenv allows us to specify versions of Ruby on a per-project basis. It also means we can avoid running sudo commands for installing gems and more as it's not affecting OS X's system Ruby.*

#### Repo Configuration
You can use a `.ruby-version` at the root directory of your repo to set the ruby version that rbenv should load for that repo.

## Gem Management
RVM is a great tool to manage different sets of gems but we're still going to need to manage gem versions in our project.

### [Bundler](http://bundler.io/)
Bundler provides a consistent environment for Ruby projects by tracking and installing the exact gems and versions that are needed.

#### Installation
Bundler itself is actually a gem. At the command line confirm you're on the gemset you'll be using and then run the following:

```bash
gem install bundler
```

#### Usage
Bundler has tons of options and I'm only going to cover the basics here. [Read the Bundler documentation](https://bundler.io/docs.html) for more configuration and features.

##### Bundle all the gems in your project
```bash
bundle install # you can actual just use `bundle` as shorthand
```

##### Update all gems for a project
This should be done with caution as if left unchecked could result in a gem dependency of your project breaking an aspect of your rails app.

```bash
bundle update
```

##### Update a specific gem for a project
From time to time a gem will be update with new feature or a security patch. To update that gem run the following

```bash
bundle update some_gem_name
```

## Databases
There are many database options for rails projects. The two most common options are MySql and Postgres.

### Mysql
There are few options for installing and managing MySql, the most popular being the direct DMG or Homebrew. I typically recommend the direct MySql DMG installer as it come with a handy system pre pane for starting stoping mysql.

##### DMG Installation
Download and install the official [MySQL Community Server](http://dev.mysql.com/downloads/mysql/) dmg.

##### Homebrew Installation
```bash
brew install mysql
```

_Note: if you plan to use homebrew for installation and management I recommend looking at [Homebrew Services](https://github.com/Homebrew/homebrew-services) a nifty brew formula that make starting and stoping services a breeze.

### Postgres
Postgres can also be installed via Homebrew but there is a OS X app called [PostgresApp](http://postgresapp.com/) that makes dealing with Postgres much more of a breeze.

##### PostgresApp Installation
Visit [http://postgresapp.com/](http://postgresapp.com/) to download the app. Open the app and boom, you have a Postgres server running.

##### Homebrew Installation
```bash
brew install postgres
```

_Note: if you plan to use homebrew for installation and management I recommend looking at [Homebrew Services](https://github.com/Homebrew/homebrew-services) a nifty brew formula that make starting and stoping services a breeze.


## GUI Apps
As far as database management goes being able to visually see the data is a necessity. Here are a list of Good apps for database management.

### Mysql
  - [Sequel Pro](http://www.sequelpro.com/)
    - This is the best no contest.
  - [Navicat for MySql](http://www.navicat.com/products/navicat-for-mysql)
    - This is their lightweight Mysql only app. Most people that use Navicat do so because [Navicat Premium](http://www.navicat.com/products/navicat-premium) supports all database types. However $799 is a steep price and personally I don't like their UI.

### Postgres
  - [Postico](https://eggerapps.at/postico/)
    - Simple and lightweight. Handles 99% of what you'd need. Some argue it's too simple.
  - [Navicat for Postgres](http://www.navicat.com/products/navicat-for-postgresql)
    - This is their lightweight Mysql only app. Most people that use Navicat do so because [Navicat Premium](http://www.navicat.com/products/navicat-premium) supports all database types. However $799 is a steep price and personally I don't like their UI.

## Rails Server
All rails apps can be run simply by running `rails s`. This can be cumbersome as well as lack support for features like subdomain support. To resolve this the rails team built a great app called [Pow](http://pow.cx)

### [Pow](http://pow.cx)
Pow is a rack server that allows you to symlink your Ruby on Rails apps for access on domains like myapp.test. Additionally pow starts and stops your rails server on demand. Eliminating the need to continually `rails s`.

The Pow team has put together an excellently documented [users manual](http://pow.cx/manual.html) which I encourage you to explore. It's full of information and a quick read.

#### Installation
run the following in terminal:
```bash
curl get.pow.cx | sh
```

If for some reason that does not work you can download the file and run it manually:
Download the file and make it executable
```bash
cd ~/Downloads; curl -0 get.pow.cx -o install_pow.sh
chmod +x install_pow.sh
```
Run that script in terminal
```bash
~/Downloads/install_pow.sh
```

#### Config a Rails app to use Pow
Pow will resolve an address like `myapp.test` to your rails app simply by creating a symlink to that folder in the pow directory.

```bash
cd ~/.pow
ln -s /path/to/myapp
```

After you've don't this you should be able to go to http://myapp.test and see your app running.

#### Config Pow's Ruby version
Pow has great info on this at [their site](http://pow.cx/manual.html#section_2.3.1), but I'll outline the basics here:
##### rbenv
If you're using rbenv this is as easy as typing `rbenv local 2.3.1` where 2.3.1 is the ruby version for this repo. This will create a `.ruby-version` file that rbenv will look to and load the right ruby gemset. Check the [rbenv readme](https://github.com/rbenv/rbenv#command-reference) for more info on setting your ruby versions

##### RVM
RVm is a touch more complex than rbenv but still only a one time setup. We want to be sure that pow is using the RVM ruby version out team specified in the project projects `.rvmc`. TO do this add a file named `pow.rc` at the application's root directory.

```bash
if [ -f "$rvm_path/scripts/rvm" ] && [ -f ".rvmrc" ]; then
  source "$rvm_path/scripts/rvm"
  source ".rvmrc"
fi
```
