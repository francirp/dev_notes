Environment Variables with Figaro
=================================

Figaro Gem on Github: https://github.com/laserlemon/figaro

```ruby
gem 'figaro'
```

```
rails generate figaro:install
```

This creates a commented config/application.yml file and ignores it in your .gitignore. Add your own configuration to this file

Example:

```ruby
EMAIL_USERNAME: dev@example.com
EMAIL_PASSWORD: dev12345
```

Then call with

```ruby
ENV["EMAIL_USERNAME"]
```

You can also set environment variables for various environments like the following:

```ruby
development:
  EMAIL_USERNAME: dev@example.com
  EMAIL_PASSWORD: dev12345
production:
  EMAIL_USERNAME: admin@website.com
  EMAIL_PASSWORD: admin12345
```

Finally, Figaro includes a Heroku rake task to set the environment variables on Heroku:

```ruby
rake figaro:heroku
```

To confirm the environment variables were set correctly:

```
heroku config
```
