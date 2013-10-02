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
