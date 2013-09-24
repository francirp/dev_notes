Adding an Avatar: Paperclip + S3
================================

Step 1: Add Paperclip:
--------------------------------

https://github.com/thoughtbot/paperclip

```ruby
gem 'paperclip'
```

In terminal:
```
bundle install
```

In your model:
```ruby
attr_accessible :avatar

has_attached_file :avatar, :styles => { :medium => "300x300>", :thumb => "100x100>" }
```

You will need to add an image to your assets/images folder to act as a default image if the user has not yet uploaded their avatar. In this example we will use a file titled: missing_avatar.png

In terminal:
```
rails g migration AddAvatarColumnsToUser
```

```ruby
class AddAvatarColumnsToUsers < ActiveRecord::Migration
  def change
    add_attachment :users, :avatar
  end
end
```

In terminal:
```
rake db:migrate
```

In your edit and new views:
```
<%= image_tag user.avatar.url(:medium) %>
<%= form_for @user, :url => users_path, :html => { :multipart => true } do |form| %>
  <%= form.file_field :avatar %>
<% end %>
```

Step 2: Add Amazon S3
--------------------------------

```ruby
gem 'aws-sdk'
```

In terminal:
```
bundle install
```

In config/environments/development.rb and config/environments/production.rb:
```ruby
config.paperclip_defaults = {
:default_url => "missing_avatar.png",
:storage => :s3,
:s3_credentials => {
  :bucket => ENV['AWS_BUCKET'],
  :access_key_id => ENV['AWS_ACCESS_KEY'],
  :secret_access_key => ENV['AWS_SECRET_ACCESS_KEY']
  }
}
```
Make sure to create your own Amazon account and setup your environment variables to store your Amazon access key, secret key, and bucket. I like to use the Figaro gem to manage my ENV variables: https://github.com/laserlemon/figaro
