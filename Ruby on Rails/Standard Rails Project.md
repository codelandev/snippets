# Default Rails Project (without API)

## DOs and DON'Ts

### What this guide do?
This guide help to configure the default rails project with tests, devise, active admin and some more things.

### What this guide don't?
This guide don't help to configure any CSS framework like Bootstrap or Foundation. You need to choose a framework before start, because simple_form gem have the appropriated generators for both frameworks.

## Starting

Generate application
```rails new -d postgresql```

### Gemfile

```
source 'https://rubygems.org'
# Project's ruby version
ruby '2.1.2'
# Bundle edge Rails instead: gem 'rails', github: 'rails/rails'
gem 'rails', '4.1.4'
# Use postgresql as the database for Active Record
gem 'pg'
# Server client
gem 'thin'
# Use SCSS for stylesheets
gem 'sass-rails', '~> 4.0.3'
# Use Uglifier as compressor for JavaScript assets
gem 'uglifier', '>= 2.5.0'
# Use CoffeeScript for .js.coffee assets and views
gem 'coffee-rails', '~> 4.0.1'
# Use jquery as the JavaScript library
gem 'jquery-rails'
# Turbolinks makes following links in your web application faster. Read more: https://github.com/rails/turbolinks
gem 'turbolinks'
# For YouTube's load bar style
gem 'nprogress-rails'
# Rails's translations
gem 'rails-i18n'
# To able use of slim inside rails application
gem 'slim-rails', '~> 2.1.4'
# Meta controllers/routes
gem 'inherited_resources', '~> 1.4.1'
# For creating elegant backends for website administration
gem 'activeadmin', github: 'gregbell/active_admin'
# For authentication
gem 'devise'
# For beautiful and simple forms
gem 'simple_form'

group :development do
  # Spring speeds up development by keeping your application running in the background. Read more: https://github.com/rails/spring
  gem 'spring'
  gem 'spring-commands-rspec', '~> 1.0.2'
end

group :development, :test do
  gem 'pry-rails', '~> 0.3.2'
  gem 'machinist', '~> 2.0'
  gem 'rspec-rails', '~> 3.0.1'
  gem 'awesome_print', '~> 1.2.0', require: false
  gem 'spring'
end

group :test do
  gem 'simplecov', '~> 0.8.2', require: false
  gem 'database_cleaner', '~> 1.3.0'
  gem 'shoulda-matchers', '~> 2.6.1', require: false
  gem 'capybara'
  gem 'capybara-webkit'
end

group :production do
  gem 'rails_12factor', '~> 0.0.2'
  gem 'passenger', '~> 4.0'
end

```
Obs: You'll need to download the Qt libraries to build and install the capybara-webkit gem. [This](https://github.com/thoughtbot/capybara-webkit/wiki/Installing-Qt-and-compiling-capybara-webkit) might help.

### Configure NProgress-Rails
Simple as that, just and the following lines on this files:

#### application.js

```
//= require nprogress
//= require nprogress-turbolinks
```

#### application.css

```
*= require nprogress
*= require nprogress-turbolinks
```

### Run the basic installers

```
bundle && rails g active_admin:install && rails g simple_form:install && rails g rspec:install && rails g machinist:install && rails generate initjs:install
```

Also check if `[initJS](https://github.com/josemarluedke/initjs)` has injected requires on your application.js

### application.html.slim

```
doctype html
html
  head
    meta[charset="utf-8"]
    meta name='viewport' content='width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no'
    /[if lt IE 9]
      meta[content="text/html; charset=utf-8" http-equiv="Content-Type"]
      = javascript_include_tag "//html5shiv.googlecode.com/svn/trunk/html5.js"
    title Balume
    = favicon_link_tag 'favicon.png'
    = stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track' => true
    = javascript_include_tag 'application', 'data-turbolinks-track' => true
    = initjs_tag 'AppName'
    = csrf_meta_tags

body
  = yield

```

### database.yml

#### For Mac OS X

```yml
development:
  adapter: postgresql
  database: [project-name]_development
  host: localhost
  poor: 5

test:
  adapter: postgresql
  database: [project-name]_test
  host: localhost
  poor: 5

production:
  adapter: postgresql
  database: [project-name]_production
  host: localhost
  poor: 5

```

#### For Linux

```yml
development:
  adapter: postgresql
  database: [project-name]_development
  username: [username]
  password: [password]
  host: localhost
  poor: 5

test:
  adapter: postgresql
  database: [project-name]_test
  username: [username]
  password: [password]
  host: localhost
  poor: 5

production:
  adapter: postgresql
  database: [project-name]_production
  host: localhost
  poor: 5

```

### Devise I18n

Copy the content on the following link and put on a new file called ```devise.pt-BR.yml``` inside
the ```config/locales``` folder.

[link to file on GitHub](https://raw.githubusercontent.com/tigrish/devise-i18n/master/locales/pt-BR.yml)

### application.rb

Your ```config/application.rb``` file must be looks like this:

```
  class Application < Rails::Application
    config.generators do |g|
      g.javascripts false
      g.stylesheets false
      g.helper false
      g.template_engine :slim
      g.test_framework :rspec,
        view_specs: false,
        helper_specs: false
      g.fixtures_replacement :machinist
    end
    # Settings in config/environments/* take precedence over those specified here.
    # Application configuration should go into files in config/initializers
    # -- all .rb files in that directory are automatically loaded.

    # Set Time.zone default to the specified zone and make Active Record auto-convert to this zone.
    # Run "rake -D time" for a list of tasks for finding time zone names. Default is UTC.
    config.time_zone = 'Brasilia'

    # The default locale is :en and all translations from config/locales/*.rb,yml are auto loaded.
    config.i18n.enforce_available_locales = false
    config.i18n.load_path += Dir[Rails.root.join('config', 'locales', '*.{rb,yml}').to_s]
    config.i18n.available_locales = ['pt-BR']
    config.i18n.default_locale = :'pt-BR'
    config.i18n.locale = :'pt-BR'
  end
```

### rails_helper.rb
Since we are using RSpec 3 and now you don't have more the classic `spec_helper.rb` and yes the `rails_helper.rb` which contain all the configurations of the RSpec, you file will be something like that:

```
# This file is copied to spec/ when you run 'rails generate rspec:install'
ENV["RAILS_ENV"] ||= 'test'
require 'simplecov'
SimpleCov.start 'rails' do
  add_filter '/app/admin' # just if you are using active_admin
end
require 'spec_helper'
require File.expand_path("../../config/environment", __FILE__)
require 'rspec/rails'
require 'capybara/rspec'
require 'capybara/rails'
require 'shoulda/matchers'

Dir[Rails.root.join("spec/support/**/*.rb")].each { |f| require f }

Capybara.javascript_driver = :webkit

# Checks for pending migrations before tests are run.
# If you are not using ActiveRecord, you can remove this line.
ActiveRecord::Migration.maintain_test_schema!

RSpec.configure do |config|g
  # Remove this line if you're not using ActiveRecord or ActiveRecord fixtures
  config.fixture_path = "#{::Rails.root}/spec/fixtures"

  config.use_transactional_fixtures = false
  
  config.before(:suite) do
    DatabaseCleaner.strategy = :transaction
    DatabaseCleaner.clean_with(:truncation)
  end

  config.before(:each) do
    DatabaseCleaner.start
  end

  config.after(:each) do
    DatabaseCleaner.clean
  end

  config.infer_spec_type_from_file_location!
end

```

### Configure Passenger to run on production
To do this, is simple, you just need to create a file called `Procfile` inside the root of your app and put this inside of it:

```
web: bundle exec passenger start -p $PORT
```

Doing this, and I'm assuming you are using [Heroku](http://heroku.com) to host your application, he will know you are using Passenger and will initialize this Procfile ( [read more about](https://devcenter.heroku.com/articles/procfile#process-types-as-templates) ).

### Run the migrations

```
rake db:create db:migrate db:seed && rake db:migrate RAILS_ENV=test
```

### Start server (local only)
You can change the server you will use, but our Standarts are to use Passenger on production and thin on development, but, be confortable to use what you want :smile:

```
bundle exec thin start
```

### Done!
