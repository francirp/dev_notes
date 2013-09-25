Twitter Omniauth
================================

**Links for Reference**
* omniauth-twitter: https://github.com/arunagw/omniauth-twitter
* omniauth: https://github.com/intridea/omniauth
* figaro for environment variables: https://github.com/laserlemon/figaro

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

I also highly recommend the Twitter gem for retrieving from Twitter or posting to Twitter on behalf of a user: https://github.com/sferik/twitter

Appendix
--------------------------------

Here's an example of how the auth hash comes in:
```ruby
{
  :provider => "twitter",
  :uid => "123456",
  :info => {
    :nickname => "johnqpublic",
    :name => "John Q Public",
    :location => "Anytown, USA",
    :image => "http://si0.twimg.com/sticky/default_profile_images/default_profile_2_normal.png",
    :description => "a very normal guy.",
    :urls => {
      :Website => nil,
      :Twitter => "https://twitter.com/johnqpublic"
    }
  },
  :credentials => {
    :token => "a1b2c3d4...", # The OAuth 2.0 access token
    :secret => "abcdef1234"
  },
  :extra => {
    :access_token => "", # An OAuth::AccessToken object
    :raw_info => {
      :name => "John Q Public",
      :listed_count" => 0,
      :profile_sidebar_border_color" => "181A1E",
      :url => nil,
      :lang => "en",
      :statuses_count => 129,
      :profile_image_url => "http://si0.twimg.com/sticky/default_profile_images/default_profile_2_normal.png",
      :profile_background_image_url_https => "https://twimg0-a.akamaihd.net/profile_background_images/229171796/pattern_036.gif",
      :location => "Anytown, USA",
      :time_zone => "Chicago",
      :follow_request_sent => false,
      :id => 123456,
      :profile_background_tile => true,
      :profile_sidebar_fill_color => "666666",
      :followers_count => 1,
      :default_profile_image => false,
      :screen_name => "",
      :following => false,
      :utc_offset => -3600,
      :verified => false,
      :favourites_count => 0,
      :profile_background_color => "1A1B1F",
      :is_translator => false,
      :friends_count => 1,
      :notifications => false,
      :geo_enabled => true,
      :profile_background_image_url => "http://twimg0-a.akamaihd.net/profile_background_images/229171796/pattern_036.gif",
      :protected => false,
      :description => "a very normal guy.",
      :profile_link_color => "2FC2EF",
      :created_at => "Thu Jul 4 00:00:00 +0000 2013",
      :id_str => "123456",
      :profile_image_url_https => "https://si0.twimg.com/sticky/default_profile_images/default_profile_2_normal.png",
      :default_profile => false,
      :profile_use_background_image => false,
      :entities => {
        :description => {
          :urls => []
        }
      },
      :profile_text_color => "666666",
      :contributors_enabled => false
    }
  }
}
```




