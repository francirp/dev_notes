Nested Forms in Rails
==========================

**Links**

* ActiveRecord Guide: http://api.rubyonrails.org/classes/ActiveRecord/NestedAttributes/ClassMethods.html
* http://railscasts.com/episodes/196-nested-model-form-part-1
* http://railscasts.com/episodes/197-nested-model-form-part-2
* https://github.com/ryanb/nested_form

Often times we need to create or update attributes in multiple associated models. Rather than having two separate forms, we can consolidate this into one form to enhance the UI of the website.

ActiveRecord makes this process much less intrusive than it otherwise would be. The key is to use the helper method accepts_nested_attributes_for :associated_model1, :associated_model2

Consider the following example (pulled from Ryan Bates' nested model episodes on Railscasts). We want to enable a user to create a survey and its associated questions in one fell swoop.

First, in the Survey model:
```ruby
accepts_nested_attributes_for :questions
```

Then in the Surveys Controller:
```ruby
def new
  @survey = Survey.new
  3.times { @survey.questions.build }
end
```

@survey.questions.build creates three question objects associated with @survey (i.e. assigns a survey_id to the object). It does not save them to the database.

In the survey form:
```ruby
<% form_for @survey do |f| %>
  <%= f.error_messages %>
  <p>
    <%= f.label :name %><br />
    <%= f.text_field :name %>
  </p>
  <% f.fields_for :questions do |builder| %>
  <p>
    <%= builder.label :content, "Question" %><br />
    <%= builder.text_area :content, :rows => 3 %>
  </p>
  <% end %>
  <p><%= f.submit "Submit" %></p>
<% end %>
```


Here's what f.fields_for is doing:

@survey has three question objects associated to it thanks to our controller method. f.fields_for :questions  is looping through each of @survey's associated question objects and building a label and input field for the content attribute.

See Note #1 and Note #2 in appendix

Finally, in our controller we can just use our standard form action:

```ruby
def create
  @survey = Survey.new(params[:survey])

  if @survey.save
    ...
  else
    ...
  end
end
```

This will automatically create the question objects in addition to the survey object. If the user did not enter content for any of the questions, we will have blank questions that we do not want.

To instruct rails to not create questions that are blank, we can use the reject_if helper in our Survey model:

```ruby
accepts_nested_attributes_for :questions, :reject_if => lambda { |a| a[:content].blank? }
```


Appendix
-------------------------------

**Note #1**

One thing to note is that if there were already questions associated with our survey, f.fields_for would build the label and input fields for those objects as well. What if, for example, the user had previously created one question for the survey (i.e. the user is now editing the survey)? f.fields_for would build the form fields for all four of these objects. If we wanted to limit the user to three questions, this would be a problem. TODO: demonstrate solution

**Note #2**

Often times, we need to allow our user to create as many questions (i.e. associated objects) as they want. In order to do this, we would need to offer a link for the user to build a new question and immediately see / edit its corresponding input fields. See Ryan Bates' second nested forms episode: http://railscasts.com/episodes/197-nested-model-form-part-2
















