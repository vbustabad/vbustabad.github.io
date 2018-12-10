---
layout: post
title:      "Nested Forms in Rails"
date:       2018-10-01 12:25:24 -0400
permalink:  nested_forms_in_rails
---

I will discuss the topic of nested forms in Rails in this blog post. I decided to delve deeper into this topic in order to understand the various parts that are involved in nested forms, which allow for a new instance of an Active Record model to be created with nested attributes. 

I will utilize the example from the Flatiron School’s Basic Nested Forms lab which covers recipes and ingredients. According to this lab, a recipe has_many ingredients and an ingredient belongs_to a recipe. Additionally, the Recipe model accepts_nested_attributes_for ingredients. See the code below:

```
class Recipe < ActiveRecord::Base
  has_many :ingredients
  accepts_nested_attributes_for :ingredients
end
```

```
class Ingredient < ActiveRecord::Base
  belongs_to :recipe
end
```

The accepts_nested_attributes_for method creates an ingredients_attributes= method which will contain the information regarding any ingredients that are associated to the current instance of recipe. In addition to accepts_nested_attributes_for, we will also use the fields_for from ActionView::Helpers::FormHelper which will allow us to populate the form with the fields for the association which contains the nested attributes. In this example, the fields_for will create fields for the various attributes of the association ingredients, which are name and quantity. See the code below:

```
<%= form_for @recipe do |f| %>

  Title:

  <%= f.label :title %>
  <%= f.text_field :title %>

  Ingredients:

  <%= f.fields_for :ingredients do |ingredient| %>
    <%= ingredient.label :name %>
    <%= ingredient.text_field :name %><br>

    <%= ingredient.label :quantity %>
    <%= ingredient.text_field :quantity %><br>
  <% end %>

  <%= f.submit %>

<% end %>
```

After the form with the fields_for is complete, we need to be careful and ensure that any new instance of recipe that is created will accept the association with the nested attributes. The new method for the Recipes controller currently contains the following code:

```
def new
    @recipe = Recipe.new
end
```
