Twitter Omniauth
================================

In your gemfile:
```ruby
gem 'omniauth-oauth2'
gem 'omniauth-twitter'
```

Next, you will need to store the twitter access token, twitter uid, and twitter secret for your user. In terminal, generate the migration file:
```
rails g migration AddTwitterColumnsToUsers
```

```ruby
class AddTwitterColumnsToUsers < ActiveRecord::Migration
  def change
    add_column :users, :twitter_access_token, :string
    add_column :users, :twitter_uid, :string
    add_column :users, :twitter_secret, :string
  end
end
```

Run the migration in terminal:
```
rake db:migrate
```

Next, we need to setup the route to handle the callback from Twitter. In config/routes.rb:
```ruby
  get 'auth/:provider/callback', to: 'users#create'
  get 'auth/failure', to: redirect('/')
```

In the Users Controller, we need to retrieve the values from Twitter for each of these columns we just created and assign them appropriately:
```ruby
def create
  #retrieve hash from omniauth:
  omniauth_hash = request.env['omniauth.auth']
  #create user and assign twitter access token, uid, and secret
  @user = User.new
  @user.twitter_access_token = omniauth_hash["credentials"]["token"]
  @user.twitter_uid = omniauth_hash["uid"]
  @user.twitter_secret = omniauth_hash["credentials"]["secret"]
  @user.email = omniauth_hash["info"]["email"]
  @user.first_name = omniauth_hash["info"]["first_name"]
  @user.last_name = omniauth_hash["info"]["last_name"]
  if @user.save
    session[:user_id] = @user.id
  end
  redirect_to root_path
end
```

Now you will need to create an application on twitter developers and retrieve your app's twitter key and twitter secret. Put both of these into environment variables (for more information on environment variables, see https://github.com/francirp/dev_notes/blob/master/rails/environment_variables.md)

Create a new file config/initializers/omniauth.rb:
```ruby
Rails.application.config.middleware.use OmniAuth::Builder do
  provider :twitter, ENV['TWITTER_KEY'], ENV['TWITTER_SECRET']
end
```

Add a link to create an account via Twitter in your view of choice:
```ruby
<%= link_to 'Sign up with Twitter', '/auth/twitter' %>
```

Now make sure to restart your server and give it a shot!


Appendix
--------------------------------

Here's an example of how the auth hash comes in:
```ruby
{
  :provider => 'facebook',
  :uid => '1234567',
  :info => {
    :nickname => 'jbloggs',
    :email => 'joe@bloggs.com',
    :name => 'Joe Bloggs',
    :first_name => 'Joe',
    :last_name => 'Bloggs',
    :image => 'http://graph.facebook.com/1234567/picture?type=square',
    :urls => { :Facebook => 'http://www.facebook.com/jbloggs' },
    :location => 'Palo Alto, California',
    :verified => true
  },
  :credentials => {
    :token => 'ABCDEF...', # OAuth 2.0 access_token, which you may wish to store
    :expires_at => 1321747205, # when the access token expires (it always will)
    :expires => true # this will always be true
  },
  :extra => {
    :raw_info => {
      :id => '1234567',
      :name => 'Joe Bloggs',
      :first_name => 'Joe',
      :last_name => 'Bloggs',
      :link => 'http://www.facebook.com/jbloggs',
      :username => 'jbloggs',
      :location => { :id => '123456789', :name => 'Palo Alto, California' },
      :gender => 'male',
      :email => 'joe@bloggs.com',
      :timezone => -8,
      :locale => 'en_US',
      :verified => true,
      :updated_time => '2011-11-11T06:21:03+0000'
    }
  }
}
```




