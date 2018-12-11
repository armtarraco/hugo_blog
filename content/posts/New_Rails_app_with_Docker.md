+++
title = "New Rails app with Docker"
date = 2018-12-11T16:02:38+01:00
featured_image = ""
description = ""
tags = []
categories = ['rails', 'docker']
+++

Source: https://auth0.com/blog/ruby-on-rails-killer-workflow-with-docker-part-1/

1. configure Dockerfile and docker-compose files
2. docker-compose up --build
3. docker-compose run --user $(id -u):$(id -g) web rails new . --force --database=postgresql --skip-bundle -T
5. configure database.yml
4. docker-compose down
6. docker-compose up --build
7. docker-compose exec web rails db:create
8. docker-compose exec --user$(id -u):$(id -g) web rails g ...
9. down & up --build if Gemfile changes
10. docker-compose exec --user $(id -u):$(id -g) web /bin/bash
11. The flag --user $(id -u):$(id -g) tells Docker to run the prompt as a normal user instead of root.
12. Multiples commands with Foreman

## Docker commands

Generate container regenerating images

    docker-compose up --build

Stop the container for rebuilding

    docker-compose down

Execute interactive commands inside container

    docker-compose exec --user $(id -u):$(id -g) web /bin/bash
    docker-compose exec --user$(id -u):$(id -g) web rails c
    docker-compose exec --user$(id -u):$(id -g) web rails g ...

## Open Postgresql console

  psql -U postgres -h localhost -d code_development

## Dockerfile

    FROM ruby:2.3.1
    RUN curl -sL https://deb.nodesource.com/setup_6.x | bash - && apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs

    # 2: We'll set the application path as the working directory
    RUN mkdir -p /code
    WORKDIR /code

    # 3: We'll add the app's binaries path to $PATH:
    ENV HOME=/code PATH=/code/bin:$PATH

    RUN gem install bundler

    ADD code/Gemfile* /code/

    RUN set -ex && bundle install --jobs 20 --retry 5

    # Copy the main application.
    COPY /code /code/

    # Expose port 3000 to the Docker host, so we can access it from the outside.
    EXPOSE 3000

    # Configure an entry point, so we don't need to specify "bundle exec" for each of our commands.
    ENTRYPOINT ["bundle", "exec"]

    # The main command to run when the container starts
    # The main purpose of a CMD is to provide defaults for an executing container.
    # CMD ["rails", "server", "-b", "0.0.0.0"]
    CMD ["/bin/bash"]

## docker-composer.yml

    version: '3.2'
    volumes:
      postgres-data:
    services:
      db:
        image: postgres:9.6
        volumes:
          - postgres-data:/var/lib/postgresql/data
        ports:
          - "5432:5432"
      web:
        build:
          context: .
          dockerfile: Dockerfile
        command: bundle exec rails s -p 3000 -b '0.0.0.0'
        volumes:
          - ./code:/code
          - type: tmpfs
            target: /code/tmp/pids/
        ports:
          - "3000:3000"
        depends_on:
          - db

## Initial Gemfile - to create 1st layer

    source 'https://rubygems.org'
    gem 'rails', '4.2.6'


## Final Gemfile - After first image build

    source 'https://rubygems.org'

    # Bundle edge Rails instead: gem 'rails', github: 'rails/rails'
    gem 'rails', '4.2.6'
    # Use postgresql as the database for Active Record
    gem 'pg', '~> 0.15'
    # Use SCSS for stylesheets
    gem 'sass-rails', '~> 5.0'
    # Use Uglifier as compressor for JavaScript assets
    gem 'uglifier', '>= 1.3.0'
    # Use CoffeeScript for .coffee assets and views
    # gem 'coffee-rails', '~> 4.1.0'
    # See https://github.com/rails/execjs#readme for more supported runtimes
    # gem 'therubyracer', platforms: :ruby

    # Use jquery as the JavaScript library
    gem 'jquery-rails'
    # Turbolinks makes following links in your web application faster. Read more: https://github.com/rails/turbolinks
    gem 'turbolinks'
    # Build JSON APIs with ease. Read more: https://github.com/rails/jbuilder
    gem 'jbuilder', '~> 2.0'
    # bundle exec rake doc:rails generates the API under doc/api.
    # gem 'sdoc', '~> 0.4.0', group: :doc

    # Use ActiveModel has_secure_password
    # gem 'bcrypt', '~> 3.1.7'

    # Use Unicorn as the app server
    # gem 'unicorn'

    # Use Capistrano for deployment
    # gem 'capistrano-rails', group: :development

    group :development, :test do
      # Call 'byebug' anywhere in the code to stop execution and get a debugger console
      gem 'byebug'
    end

    group :development do
      # Access an IRB console on exception pages or by using <%= console %> in views
      gem 'web-console', '~> 2.0'

      # Spring speeds up development by keeping your application running in the background. Read more: https://github.com/rails/spring
      gem 'spring'
    end

## Useful gems for development and testing

Recomended for faster development and testing

    group :test , :development do
      gem 'guard', '~>2.14.2',require:false
      gem 'guard-livereload','~>2.5.2', require: false
      gem 'guard-minitest', '~>2.4.6', require: false
      gem 'rack-livereload'
      gem 'foreman'
    end

## Multiples commands with Foreman

Foreman is in charge of starting the server and other tools
Also, exposes the port that uses LiveReload

    # Procfile.dev
    web: bundle exec rails s -b '0.0.0.0'
    guard: bundle exec guard -i

    # docker-compose.yml
    ...
    web:
      ...
      command: foreman start -f Procfile.dev -p 3000
      ...
      ports:
        ...
        - "35729:35729"

## database.yml

    default: &default
      adapter: postgresql
      encoding: unicode
      host: db
      user: postgres
      port: 5432
      password:
      pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>

    development:
      <<: *default
      database: code_development

## .dockerignore

    # Ignore version control files:
    .git/
    .gitignore

    # Ignore docker and environment files:
    *Dockerfile
    docker-compose*.yml
    *.env
    development-entrypoint
    .dockerignore

    # Ignore log files:
    log/*.log

    # Ignore temporary files:
    tmp/

    # Ignore test files:
    .rspec
    Guardfile
    spec/

    # Ignore OS artifacts:
    **/.DS_Store

    .rspec

    # 3: Ignore Development container's Home artifacts:
    # 3.1: Ignore bash / IRB / Byebug history files
    .bash_history
    .byebug_hist
    .byebug_history
    .pry_history
    .guard_history

    # 3.3: bundler stuff
    .bundle/*

