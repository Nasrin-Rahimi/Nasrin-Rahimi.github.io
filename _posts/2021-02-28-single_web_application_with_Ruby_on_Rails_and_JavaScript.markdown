---
layout: post
title:      "Single Web Application with Ruby on Rails and JavaScript"
date:       2021-02-28 08:48:55 -0400
permalink:  single_web_application_with_Ruby_on_Rails_and_JavaScript
---

In this blog post I'm' going to talk about how to build a Single Page Application (SPA). My SPA is about daily tasks. Every user enter her name and see the list of her daily tasks. User can create a new task, checked or unchecked tasks and delete tasks.

My frontend built with HTML, CSS, and JavaScript. and it's' communicate with a backend API that I built with Ruby on Rails. 

Create Project Structure:

mkdir Daily-Tasks
cd Daily-Tasks
rails new backend
mkdir frontend

Backend ( Ruby on Rails ):
First I created models and their relations.

rails g model User name --no-test-framework
rails g model Task title image_url done:Boolean user:references --no-test-framework

Every user has_many tasks and every task belongs to a user. "user:references" in task model generator, create has_many relationship in User model, but we have to add belongs_to relation to Task model.

Then for creating restful Routes, I addes the below code in routes.rb file:

namespace :api do
    namespace :v1 do
      resources :users do
        resources :tasks
      end
    end
end

Using Api for namespacing, is a convention to show our rails application is just an Api and send data and doesn't' rendering any view. And we use V1 because if we'll' have any changes in our api, instead of changing our app, add another version to it. 

After that I generated my Controllers:

rails g controller users
rails g controller tasks

According to my routes, The controllers should be in api > v1 folder. So in app > controllers I created api > v1 and move users_controller and tasks_controller to v1 folder. Also I added namespacing to controller's class definitions:

class Api::V1::TasksController < ApplicationController

end

For the last step in my API, I added actions in my controllers.

users_controller.rb: 

def index 
    users = User.all
    render json: users, except: [:created_at, :updated_at]
end

Here is the big difference between Ruby on Rails project and Rails as an API project. After getting all users, I rendered users json instead of rendering a view for showing the users.

Now if we migrate, add some seeds data and run the server:

rails server  OR  rails s

and open http://localhost:3000/api/v1/users, We should see all users as a json.
 
Now it's' time to create files, classes and functions in frontend JavaScript.

Frontend ( JavaScript ):
For frontend, I created an index.html to the root of the frontend project. At first visit, user see a form with an input text and a button. User can enter her name and click Daily Tasks button. If user doesn't' exist in Database, He added to Database and recieve a message to start adding her tasks. And if user have some tasks, her tasks are loaded. Then user can update tasks status or delete tasks.

For my frontend, I created two foldders "Adapters" and "Components". In Adapter, For each model in backend I added a class (TasksAdapter, UsersAdapter) to send web request to backend API and receive a collection of JSON in return. For example for getting user info I created a method in UsersAdapter class:

findOrCreateUser(userName){
    const user = {
        name: userName
    }

    return fetch(this.baseUrl, {
        method: 'POST',
        headers: {
            'content-type': 'application/json',
            "Accept": "application/json"
        },
        body:  JSON.stringify( {user} )
    }).then(res => res.json())
}

baseUrl here is "http://localhost:3000/api/v1/users" and it send a web request to UsersController's' create action. 

def create
    if User.find_by(:name => user_params[:name])
        user = User.find_by(:name => user_params[:name])
        redirect_to "/api/v1/users/#{user.id}"
    else
        user = User.create(user_params)
        render json: user
    end        
end


I used JavaScript to send a web request to backend API for create, update and delete tasks like above request and get a collection of JSON in return. Then I used that JSON to fill DOM elements and show them in index.html. 

Thanks for reading!

You can check my project here:

(https://github.com/Nasrin-Rahimi/Daily-Tasks) 









