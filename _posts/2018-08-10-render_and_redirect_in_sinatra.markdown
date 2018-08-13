---
layout: post
title:      "Render and Redirect in Sinatra"
date:       2018-08-10 16:15:26 -0400
permalink:  render_and_redirect_in_sinatra
---


This blog post is to clarify the difference between rendering and redirecting in Sinatra. As I worked on the Sinatra project, I implemented rendering and redirecting in various controller actions throughout the application and wanted to expand on this topic. I will share what I have learned in this blog post. 

There is a fundamental difference between rendering and redirecting in Sinatra even though both appear to be similar. Rendering is used to show the view template for a specific controller action. For instance, using the Bon Voyage travel application that I built as an example, if I wanted to show the view template for creating a new travel recommendation, I would include the code erb :'/recommendations/create_recommendation' in the get '/recommendations/new' controller action. Here is the code below:

```
  get '/recommendations/new' do
    if logged_in?
      erb :'/recommendations/create_recommendation'
    else
      redirect to("/login")
    end
  end
```

Rendering and redirecting will both provide the user with a different view, whether by showing a template for any of the CRUD actions or by taking the user to another URL on the Internet browser. The key difference lies in whether any information regarding the variables is saved. For instance, in the edit action, the @recommendation variable is provided to the edit view template in order to persist the information regarding the recommendation. This supplies the template with the attributes and behavior of the specific recommendation that the user would like to edit. 

With redirecting, however, a new HTTP request is generated and the user is taken to another page or URL on the Internet browser. When a new HTTP request is generated, the information regarding the instance variable will not persist. In order to perform the redirect, the code that would need to be written is redirect to(“/(enter the URL path here)”).  Using the Bon Voyage application as an example, assuming that the user is logged in to the application, I redirected to the main profile page for the user. See the code below: 

```
  get '/recommendations/:id/edit' do

    if logged_in?
      @recommendation = Recommendation.find(params[:id])
      @user = User.find_by(id: current_user.id)
      if @recommendation && @recommendation.user_id == @user.id
        erb :'/recommendations/edit_recommendation'
      else
        redirect to("/profile")
      end
    else
    redirect to("/login")
    end
  end
```

As mentioned before, when redirecting in Sinatra, a completely new HTTP request is generated. As a result, all of the variables set by the previous controller action will disappear and will not be available to the new action. Another reason to redirect in Sinatra is to avoid the possibility of duplicating records if the user reloads the browser. If you use redirect to, it will trigger the previous GET request and reload the same page again. If, however, the user refreshes the page after successfully completing a purchase or other transaction, there is the possibility that the original HTTP POST request will be submitted again. If the POST request is submitted again, it would create a duplicate purchase. 

