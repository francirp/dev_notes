**Validate Length**
```ruby
validates :first_name, :length => { :maximum => 11, :too_long => "%{count} characters is the maximum allowed" }
```
