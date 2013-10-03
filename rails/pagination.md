Pagination with Kaminari
=========================

**Links**
* Kaminari: https://github.com/amatsuda/kaminari

```
gem 'kaminari'
```

Parts Controller (the search_parts method is a custom query method):
```ruby
def index
  @parts = Part.order("created_at DESC").page(params[:page]).per(12)
end
```

Parts Index
```
<%= paginate @parts %>
<% @parts.each do |part| %>
  ...
<% end %>
<%= paginate @parts %>
```


Styling with Kaminari
---------------------------

To see a list of themes available, run the following:
```
rails g kaminari:views
```
As of October 2013, Kaminari offers bootstrap, github, google, and default themes.

To generate the default views:
```
rails g kaminari:views default
```
