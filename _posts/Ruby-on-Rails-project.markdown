---
layout: post
title:      "Ruby on Rails Project"
date:       2020-10-22 06:35:34 -0400
permalink:  ruby_on_rails_project
---

My kids spend two hours to watch Netflix movies everyday, So I decided to build a small part of Netflix. I can demonstrate the principles of working with Ruby on Rails, also kids have fun to see working on an application like Netflix!

My data model has User, Video, Genre, and Review. Each video and genre will be able to be created, read, edited, and deleted. Video and Genre has many-to-many connection through genres_videos. There will also be reviews associated with each individual video. Reviews will be able to be created and read through users. So I have another many-to-many connection between User and Video through reviews.
    
For building a web application with Ruby on Rails, I highly recommanded to read :

https://guides.rubyonrails.org/index.html

It's' an awesome documentation to learn about Ruby on Rails.
        
In this blog post, I want to talk about one of the important part of my Rails project, Active Record Associations.

As you see I have two many-to-many connections in my project. One between Video and Genre through genres_videos, and the other between User and Video through reviews.
    
Rails offers two different ways to declare a many-to-many relationship between models. The first way is to use has_and_belongs_to_many, which allows you to make the association directly, like genres and videos connection in my project. The second way to declare a many-to-many relationship is to use has_many :through. This makes the association indirectly, through a join model like Review for connection between User and Video models.

I explain about these two associations in below :
 
A has_many :through association is often used to set up a many-to-many connection with another model. This association indicates that the declaring model can be matched with zero or more instances of another model by proceeding through a third model.

For creating many-to-many connection between User and Video, I created a reviews where users write reviews om videos. The relevant association declarations is look like this:

class User < ApplicationRecord
  has_many :reviews
  has_many :videos, through: :reviews
end

class Review < ApplicationRecord
  belongs_to :user
  belongs_to :video
end

class Video < ApplicationRecord
  has_many :reviews
  has_many :users, through: :reviews
end
    
The corresponding migration is look like this:

class CreateReviews < ActiveRecord::Migration[6.0]
  def change
    create_table :reviews do |t|
      t.integer :rating
      t.text :description
      t.references :user, foreign_key: true
      t.references :video, foreign_key: true
      t.datetime :created_at
    end
  end
end
    
The collection of join models can be managed via the has_many association methods. For example, if you assign:

user.reviews = reviews
    
Then new join models are automatically created for the newly associated objects. If some that existed previously are now missing, then their join rows are automatically deleted.

Automatic deletion of join models is direct, no destroy callbacks are triggered.
        
A has_and_belongs_to_many association creates a direct many-to-many connection with another model, with no intervening model. This association indicates that each instance of the declaring model refers to zero or more instances of another model. 
For creating many-to-many connection between Video and Genre, I created a genres_videos where a genre having many videos and each video appearing in many genres. The models declares this way:
    
class Genre < ApplicationRecord
  has_and_belongs_to_many :videos
end

class Video < ApplicationRecord
  has_and_belongs_to_many :genres
end
    
The corresponding migration is look like this:

class CreateGenresVideosJoinTable < ActiveRecord::Migration[6.0]
  def change
     create_table :genres_videos, id: false do |t|
      t.belongs_to :video
      t.belongs_to :genre
    end
  end
end


Choosing Between has_many :through and has_and_belongs_to_many

The simplest rule of thumb is that you should set up a has_many :through relationship if you need to work with the relationship model as an independent entity. If you don't need to do anything with the relationship model, it may be simpler to set up a has_and_belongs_to_many relationship (though you'll need to remember to create the joining table in the database).

You should use has_many :through if you need validations, callbacks, or extra attributes on the join model.
        
One more note, for creating Join Tables for has_and_belongs_to_many Associations, Active Record creates the name by using the lexical order of the class names. And join table should created without a primary key. For example in my project The join table name should be genres_videos not videos_genres.

Thanks for reading!

You can check my gem here:

(https://github.com/Nasrin-Rahimi/MyFlix) 


