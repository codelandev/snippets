# Default Rails Project (without API)

## DOs and DON'Ts

### What this guide do?
This guide help to configure the default rails project with tests, devise, active admin and some more things.

### What this guide don't?
This guide don't help to configure any CSS framework like Bootstrap or Foundation. You need to choose a framework before start, because simple_form gem have the appropriated generators for both frameworks.

## Starting

### Gemfile

```
source 'https://rubygems.org'
# Project's ruby version
ruby '2.1.1'
# Bundle edge Rails instead: gem 'rails', github: 'rails/rails'
gem 'rails', '4.1.1'
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
gem 'inherited_resources', '~> 1.5.0'
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
end

group :test do
  gem 'simplecov', '~> 0.8.2', require: false
  gem 'database_cleaner', '~> 1.2.0'
  gem 'shoulda-matchers', '~> 2.6.1', require: false
  gem 'capybara'
  gem 'capybara-webkit'
end

group :production do
  gem 'rails_12factor', '~> 0.0.2'
end

```

### Run the basic installers

```
bundle && rails g active_admin:install && rails g simple_form:install && rails g rspec:install && rails g machinist:install
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


### application.css
Add to your ```application.css``` the following lines:


```
#= require nprogress
#= require nprogress-turbolinks
```

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

### spec_helper.rb
Inside your ```spec_helper.rb``` file you need to add the ```simplecov``` caller and the Capybara require above ```require 'rspec/rails'```.

```
require 'simplecov'
SimpleCov.start 'rails' do
  add_filter '/app/admin'
end

...

require 'capybara/rspec'
require 'shoulda/matchers'

...

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
```

About the ```RSpec.configure do |config|``` line you need to tell Capybara to use ```capybara-webkit```
gem. So, copy and paste this line above the indicated line:

```
Capybara.javascript_driver = :webkit
```

### Run the migrations

```
rake db:create db:migrate db:seed && rake db:migrate RAILS_ENV=test
```

### Start server

```
bundle exec thin start
```

### Done!
