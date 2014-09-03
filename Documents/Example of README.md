# Name of the Project

* Main branch: master
* Ruby version: 2.1.2
* Rails version: 4.1.5
* PG version: 0.17.1

## Installation / Getting Started

To install [Project Name] (for development) on your machine, just follow the tips above:

* **Clone the project**

```git clone git@github.com:codelandev/[project].git```

* **Configure the remotes**

```git remote add staging git@heroku.com:[project]-staging.git```

```git remote add production git@heroku.com:[project].git```

* **Install gems**

 ```bundle install```

* **Create DB and run migrations**

```bundle exec rake db:create db:migrate && bundle exec rake db:seed```

*For some reason if you run `seeds` together with migrate it's cause some errors.*

## Running Specs

We are using Capybara + RSpec for the tests, so, in order to run the specs maybe you will need somethings to keep `selenium-webdriver` running.

* **Install [Firefox](https://download.mozilla.org/?product=firefox-32.0-SSL&os=osx&lang=pt-BR) on your machine**

* **Create Test DB and run migrations**

```bundle exec rake db:create db:migrate RAILS_ENV=test```

* **Run Specs**

```bundle exec rspec .```

## Creating feature branches

In all projects we work with `feature branches`. It's a good way to controll who are doing what and to improve quality of code, once you need to up a PR with that branch after.

### Create the branch

The nomenclature of the feature branch is composite by `{name initials}-{feature name || description}`, and probably will be something like that: `pm-review-typo` or `pm-create-users`.

Also, always keep you branch up-to-date with master, and keep master updated too. To do this, always run `git checkout master && git pull origin master`

Now, to create the feature branch just run `git checkout master && git checkout -b
[name-of-branch]`.

## Openning a Pull Request

After you finish the implementations what you did on your branch, you can up this to Github and open a Pull Request. This way other persons of the project can available your things and propose improvements. Just create the PR when you have confidence you create everything you need to, like views, controllers, specs... 

## Finishing/Delivering/Accepting Pivotal

For now we are working with Pivotal to organize the features and sprints of the project, so, just delivery a task when you finish this and up the PR. Let to the Project Manager to accept or reject your feature.
