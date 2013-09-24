**HTTP Authentication**
```ruby
http_basic_authenticate_with name: "admin", password: "conifer1"
```

**Email Regex**
```ruby
email_address.scan(/\A[\w+\-.]+@[a-z\d\-.]+\.[a-z]+\z/i)
```

**Custom Error Messages**
```ruby
en:
  hello: "Hello world"
  activerecord:
    attributes:
      post:
        primary_image_id: "Image of view"
        secondary_image_id: "Image of feet"
        title: "A title"
        description: "A description"
    errors:
      messages:
        confirmation: "Passwords do not match"
      models:
        post:
          attributes:
            primary_image_id:
              blank: "You must add a view image to create a post"
            secondary_image_id:
              blank: "You must add an image of your feet to create a post"
            title:
              blank: "A title is required to create a post"
            description:
              blank: "A description is required to create a post"
```
