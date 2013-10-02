Forms with Rails
====================================

Styling
------------------------------------

To center placeholders vertically, in your sass put the following:
```sass
input[type="text"] {
  line-height: normal;
}
```


Helper Methods
------------------------------------

* Turn Input Field Autocomplete Off: :autocomplete => :off
* Set Maximum Character Length: :maxlength => 15
* Set Autofocus (i.e. when user goes to page, cursor will automatically be placed in form field): :autofocus => true
* Placeholder: :placeholder => "Text Here"

**form_tag**

* select:
This is useful for dealing with models and an array of predefined options (not from a model). Let's build a dropdown to allow the user to pick a year for our budget.
Our model is "Budget" and our column in the database is "Year"
<%= select :budget, :year, 2000..2050 %>

* select_tag: The options_for_select or options_from_collection_for_select often need to be used in tandem with select_tag.
<%= select_tag :state, options_for_select(us_states, @city.state) %> where us_states is a helper method that returns an array of states (i.e. ["AK", "OH", "IL"]).

* options_for_select

**form_for**

Helper Methods:

* f.select
This is useful for giving a set of predefined options that don't originate from a model.
Our model is "Budget" and our column in the database to save the selection is "Year"
<%= f.select :year, 2000..2050 %>
The helper method knows that :year is a column in "Budget" model because we specify the model in the form_for tag: form_for(@budget)
* f.collection_select
This is useful if your options derive from a column in your model
Adding an employee to a model with foreign key employee_id:
<%= f.collection_select :employee_id, Employee.order('full_name ASC').all, :id, :full_name %>

Adding an ID and Class to the form:
<%= form_for(@user, html: {class: 'form-horizontal', id: 'signup_form'}) do |f| %>


Error Messages
-------------------------------

```scss
#signup_form {
  .error_messages {
    width: $badge-width;
    margin-top: 20px;
    color: red;
  }
  .field_with_errors {
    input[type="text"], input[type="email"] {
      border: thin solid red;
    }
  }
}
```

```html
<%= form_for(@user, html: {id: 'signup_form', class: 'form-horizontal'}) do |f| %>
  <% if @user.errors.any? %>
    <h2>Please Correct the Following Errors</h2>
    <div class="error_messages">
      <% @user.errors.full_messages.each do |msg| %>
        <li><%= msg %></li>
      <% end %>
      </ul>
    </div>
  <% end %>
```
