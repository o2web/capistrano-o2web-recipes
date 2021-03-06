# Capistrano::O2webRecipes

Common Capistrano Recipes used by O2Web.

Works *only* with Capistrano 3+.

### Installation

Add this to `Gemfile`:

    group :development do
      gem 'capistrano', '~> 3.1'
      gem 'capistrano-rails', '~> 1.1'
      gem 'capistrano-o2web-recipes', '~> 0.0.1'
    end

And then:

    $ bundle install

### Setup and usage

Add this line to `Capfile`, after `require 'capistrano/rails/migrations'`

    require 'capistrano/o2web_recipes'
    
To install Nginx/Monit config files, run:

    $ rails g capistrano:o2web_recipes:install
    
Available tasks:

```bash
cap [stage] git:update_repo_url           # Update new git repo url
cap [stage] tmp_cache:clear               # Clear file system tmp cache
cap [stage] files:server_to_local         # Import public files
cap [stage] files:local_to_server         # Export public files
cap [stage] files:private:server_to_local # Import private file
cap [stage] files:private:local_to_server # Export private files
cap [stage] db:server_to_local            # Sync local DB with server DB
cap [stage] db:local_to_server            # Sync server DB with local DB
cap [stage] nginx:local_to_server         # Export nginx configuration files
cap [stage] monit:start
cap [stage] monit:stop
cap [stage] monit:restart
cap [stage] monit:reload
```

Also, `deploy:assets:precompile` task is done locally and a `cron.log` file is created/touched in `shared/log` after deploy.

Configurations can be customized in your deploy file with:

```ruby
set :server, 'example.com'
set :deployer_name, 'deployer'
# default to ['system']
set :files_public_dirs, fetch(:files_public_dirs).push(*%W[
  spree
])
# default to []
set :files_private_dirs, fetch(:files_private_dirs).push(*%W[
])
set :nginx_max_body_size, '10m'
# default to ['system', 'images']
set :nginx_public_dirs, fetch(:nginx_public_dirs).push(*%W[
  spree
])
# default to ['404.html', '422.html', '500.html', 'favicon.ico']
set :nginx_public_files, fetch(:nginx_public_files).push(*%W[
])
# default to {} with keys as original urls and values as rewritten urls
set :nginx_redirects, fetch(:nginx_redirects).merge({
})
```

### TODO

1. Nginx Whitelisting/Blacklisting/Throttling
1. Fail2Ban
1. Use stages (staging/production) to scope Nginx config file to allow multiple stages on the same server (sites-available/sites-enabled).
1. Maintenance page
