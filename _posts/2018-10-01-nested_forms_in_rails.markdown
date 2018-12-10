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

As you can see, the new instance of recipe that is instantiated does not contain any parameters, associations, or other identifying information. For this reason, in the new method for the Recipes controller, we need to include the code: @recipe.ingredients.build. This code will create an empty ingredient object that is associated to the particular instance of recipe. We would then need to add as many ingredients as are necessary for the recipe. If the recipe contains five different ingredients, we would modify the new method in the Recipes controller as follows:

```
def new
    @recipe = Recipe.new
    5.times { @recipe.ingredients.build }
end
```

After completion of this step, there would be five sets of ingredient fields that would be visible on the new recipe form. The user would then complete the form by providing the necessary information for the recipe, including its title and the name and quantity for each of its ingredients. The submission of the form would create a new instance of recipe with its corresponding parameters, associations, and nested data. The data structure for the new instance of recipe would be a hash. The hash would look like the following:

```
{
  'recipe' => {
    'title' => 'Ham, cheese and tomato omelette',
    'ingredients_attributes' => {
      '0' => {
        'name' => 'Eggs',
        'quantity' => '2'
      },
      '1' => {
        'name' => 'Ham',
        'quantity' => '3'
      }
			'2' => {
        'name' => 'Cheese',
        'quantity' => '2'
      }
			'3' => {
        'name' => 'Tomatoes',
        'quantity' => '1'
      }
			'4' => {
        'name' => 'Onions',
        'quantity' => '1'
      }
    }
  }
}
```

The final step for the nested form is to update the recipe_params method in the Recipes controller. The recipe_params method would be updated to accept ingredients_attributes as a parameter. The recipe_params method would therefore look like the following:

```
def recipe_params
    params.require(:recipe).permit(
      :title,
      ingredients_attributes: [
        :name,
        :quantity
      ]
    )
  end
```

The aforementioned steps cover the basic fundamentals for the nested form in Rails. There is still more to discuss with the topic, such as preventing empty records and avoiding duplicates. Nevertheless, the information presented in this blog post has provided the foundation for utilizing nested forms in Rails and steps for how to incorporate them into your application. 

Stay tuned for future posts!
