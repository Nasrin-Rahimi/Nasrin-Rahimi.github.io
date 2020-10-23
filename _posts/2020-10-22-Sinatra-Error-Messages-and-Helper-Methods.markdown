---
layout: post
title:      "Sinatra Error Messages and Helper Methods"
date:       2020-10-22 06:35:34 -0400
permalink:  sinatra_error_messages_and_helper_methods
---
In this blog, I will explain two important parts of my Sinatra project.

Error Messages 

Sometimes we want to show little messages to our users like error message for empty login fields or success message for doing something successfully like sign in, sign out, or submit a form. Sinatra Flash is an awesome gem that allows you to interact with the user via messages.

Here's how to set it up!

Add gem 'rack-flash3' to your Gemfile.

Install the gem by typing bundle install in your terminal.

Add the following code to your layout.erb file, right before your yield block:

    <% flash.keys.each do |type| %>
        <div data-alert class="flash <%= type %> alert-box radius">
            <%= flash[type] %>
        </div>
    <% end %>

The above erb block, takes the key from your flash message and sets that as an HTML class for the alert box. 

To make sure Flash message is working correctly, add the following code to your application_controller: 

    get '/' do
        flash[:notice] = "Flash is working!"
        erb :index
    end

Flash stores its messages as a hash. So for seeing the content of Flash message, place a binding.pry in your code (such as in get '/' do or in layout.erb - either work!) and run your application, then go to the browser and open your home page. You should see the message "Flash is working!".

Try to use a descriptive word as the key! For example, if your flash is warning your user of a potential problem, you could use flash[:warning]. predefined keys are: :notice, :error, :warning, :alert, :info, :success.

The under link helped me to add Flash message in my project.

(https://gist.github.com/cmkoller/0d3b048b3c4b48ee4955)


Helper methods

Helper methods are the way we can wrap some repetitive codes into a single method in order to save some work and also make the codes look more semantically clear but at the same time, these methods don’t actually belong to any models we already have.

For example, We need to check if User logged_in in many routes. So instead of writing the same code in every route, We can write a logged_in? method as a helper method and use it in all routes that we need.

We have to ways to create the helper mehtods:

The first way is to create a new file called helper.rb, then write helper methods there within class Helper. 

Since we will not instantiate any Helper instances because we only want to use methods there, we will write all the methods as class methods instead of instance methods. Here is the code:

    class Helper

      def self.current_user(session)
        current_user ||= User.find_by(:username => session[:username]) if session[:username]
      end
    
      def self.logged_in?
        !!current_user
      end

    end

Next time when we try to use these methods, We will just do Helper.current_user(session) or Helper.logged_in?

The second way is to leave it in the controller document. Within class ApplicationController, on the very bottom, we can add these following codes:
  
    helpers do
        def logged_in?
          !!current_user
        end

        def current_user
          @current_user ||= User.find_by(:username => session[:username]) if session[:username]
        end 
    end

And when we want to use these methods, We just put current_user or logged_in?, then they will be called right away.

From the two versions above, maybe you already see their differences. the second example is simpler: no need to pass a session argument because when methods are within ApplicationController, session is enabled and can be reached everywhere in the controller; no need to build another class; no need to call it as a class method. That is the reason I choose the second one. However, I believe when there are more and more helpers methods, the first one will be easier to maintain and be taken care of.

Thanks for reading!

You can check my gem here:

(https://github.com/Nasrin-Rahimi/recipes) 
