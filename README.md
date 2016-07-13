# Rails Quick Start Guide
A quick-start guide to getting a Ruby on Rails env setup on a mac.

## Prerequisites
  - [Homebrew](http://brew.sh/)

## Index
- [Ruby Management](#ruby-management)
  - [RBENV](#rbenv)
  - [RVM](#rvm)
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


### [RVM](https://rvm.io/)
As they describe it: "RVM is a command-line tool which allows you to easily install, manage, and work with multiple ruby environments from interpreters to sets of gems."

#### Installation
RVM signs all of their releases, which means you'll need to download the public key before you're able to download/install RVM.

Install `gpg2`
```bash
brew install gpg2
```
Download the public key:
```bash
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
```
  - _Note: I'm not sure how often this is updated. Refer to [https://rvm.io/rvm/install](https://rvm.io/rvm/install) for the most recent._
  - Depending on your office environment, the port required to download the public key may be blocked. If necessary ask your IT department to open the port.
  - In the odd chance all the above fails because the `gpg --keyserver` is failing you can run this fallback:
      `curl -sSL https://rvm.io/mpapis.asc | gpg --import -`

Now that you have the public key in place let download/install RVM, the latest stable ruby and rails

```bash
\curl -sSL https://get.rvm.io | bash -s stable --ruby --rails
```

RVM is installed, close your terminal and then re-open it. Now, lets see if RVM was loaded properly:
```bash
rvm | head -n 1
```
If that doesn't work you'll want to be sure RVM was added to you `~/.bash_profile` so it will autoload with terminal. Add the following to `~/.bash_profile`:

```bash
# RVM
export PATH="$PATH:$HOME/.rvm/bin"
source ~/.rvm/scripts/rvm
```

That's it! RVM is installed. If you open a new terminal window you should be able to run `rvm` and get a print out of the RVM details.

#### Usage
RVM allows you to create and manage one or more gemsets. This setup can be as simple or complex as you'd like it to be. Below are a few quick actions that outline the basics.

##### Listing Gemsets
At any time you can list all of your gemsets by running
```bash
rvm list gemsets
```

##### Switching Gemsets
In most cases your Rails app will have a file that will auto switch your bash session to the proper gemset (see .rvmc project file below) when you cd into the repo directory. However you can manually switch gemsets by running `rvm use desired_gemset` where `desired_gemset` is the name of the gemset you're trying to switch to.

example
```bash
rvm use ruby-2.1.2
```

##### Creating Gemsets
Each dev has different standards in how they manage their gems. Some people create a gemset for each project, which allows for more granular control over what gems are available to that project. While others just have a gemset for each ruby version. This is entirely developer preference, there is no "right" way.

To create a gemset be sure you're on the desired ruby version:
```bash
rvm 2.1.2
```

Then use the RVM create command:
```bash
 rvm gemset create sample
```

Now if you run `rvm list gemsets` you'll see your newly created `sample` gemset in the list.

##### .rvmc project file
Over time you'll find yourself working on many Rails apps each using a different gemset. This could be somewhat cumbersome to keep track of if you had to switch gemsets manually. Luckily RVM has a neat feature in it's `.rvmc` file. By create a file named `.rvmc` at the root directory of the application you can tell RVM which gemset to use. Which means when you cd into the application directory the RVM gemset will automatically be switched. A basic `.rvmc` file looks like:

```bash
rvm use ruby-2.1.2
```

The ruby version specified can be whatever you like.

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
Bundler has tons of options and I'm only going to cover the basics here. [Read the Bundler documentation]() for more configuration and features.

##### Bundle all the gems in your project
```bash
bundle install # you can actual just user `bundle` as shorthand
```

##### Update all gems for a project
This should be done with caution as if let uncheck could result a a gem dependency of your project breaking an aspect of your rails app.

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
Again Postgres can being installed via Homebrew but there is a mac app called [PostgresApp](http://postgresapp.com/) that make dealing with Postgres much more of a breeze

##### PostgresApp Installation
Visit [http://postgresapp.com/](http://postgresapp.com/) and download the mac app. Open the app and boom you'll have a Postgres server running.

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
Pow is a rack server that allows you to symlink your Ruby on Rails apps for access on domains like myapp.dev. Additionally pow starts and stops your rails server on demand. Eliminating the need to continually `rails s`.

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
Pow will resolve an address like `myapp.dev` to your rails app simply by creating a symlink to that folder in the pow directory.

```bash
cd ~/.pow
ln -s /path/to/myapp
```

After you've don't this you should be able to go to http://myapp.dev and see your app running.

#### Config Pow's Ruby version
Pow has great info on this at [their site](http://pow.cx/manual.html#section_2.3.1), but I'll outline the basics here:
##### rbenv
If you're using rbenv this is as easy as typing `rbenv local 2.3.1` where 2.3.1 is the ruby version for this repo. This will create a .ruby-version file that rbenv will look to and load the right ruby gemset.

##### RVM
RVm is a touch more complex than rbenv but still only a one time setup. We want to be sure that pow is using the RVM ruby version out team specified in the project projects `.rvmc`. TO do this add a file named `pow.rc` at the application's root directory.

```bash
if [ -f "$rvm_path/scripts/rvm" ] && [ -f ".rvmrc" ]; then
  source "$rvm_path/scripts/rvm"
  source ".rvmrc"
fi
```
