form_tag

Helper Methods:

* select:
This is useful for dealing with models and an array of predefined options (not from a model). Let's build a dropdown to allow the user to pick a year for our budget.
Our model is "Budget" and our column in the database is "Year"
<%= select :budget, :year, 2000..2050 %>

* select_tag: The options_for_select or options_from_collection_for_select often need to be used in tandem with select_tag.
<%= select_tag :state, collection_for_select(us_states, @city.state) %> where us_states is a helper method that returns an array of states (i.e. ["AK", "OH", "IL"]).

* options_for_select

form_for

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
