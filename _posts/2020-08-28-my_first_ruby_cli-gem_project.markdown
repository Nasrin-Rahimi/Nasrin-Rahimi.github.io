---
layout: post
title:      "My First RUBY CLI-GEM Project"
date:       2020-08-28 06:35:34 -0400
permalink:  my_first_ruby_cli-gem_project
---

For the first time, I didn’t know where to start. I have some coding experience, but this was the first time that I wanted to create an application from scratch. For the first step, I thought about an idea and how to implement it. I decided to create an application that gives the user information about COVID-19. Why? All of my friends and family check COVID-19 information from lots of resources every day! I thought they can save time with checks from just one trusted source, and I can prepare that source for them. I searched for trusted data that I can use in my CLI project. I found an API on https://about-corona.net that is updated multiple times a day from the World Health Organization and Johns Hopkins CSSE.

I watched some helpful videos about how to build a CLI gem. The Avi’s "Daily Deals" project was very helpful that he explains it in detail. I ran "bundle gem <project_name> (covid19_cli)" which created the file structure that I needed. 

I needed to create a user interface(CLI class), an API class to get data from the Internet, and a country class responsible for working on objects.

I designed the user interface through a CLI class. This would be responsible for the user and program interactions. A good idea was to sudo code a draft in the CLI file that would later allow me to put “real” code into it. 

Once my CLI logic was set, I drafted a Country class that will hold the attributes of countries. I did a basic setup for the class. initialize, #save, .all, .find_by_name. I thought to keep this class as simple as possible so that the functionality will be easier to pass to the CLI.

Now I needed to get data from API. I used URI and NET:: HTTP libraries to send an HTTP request from my program and got the JSON response from the API! At this point, I had a JSON hash with all the countries' COVID-19 information. I revoked a method from my Country class to create country objects from the hash.

My main problem was solved. The rest of the time was spent coding the user interface until it was presented the way I wanted.

I’m really happy with the results because I achieved exactly what I had in mind. Also, I learned a lot! What I take away the most from the project is how to work without the need for a test driven development tool, and instead use the terminal and console to troubleshoot. Tools like binding.pry are lifesavers and your best friend!

You can check my gem here:

(https://github.com/Nasrin-Rahimi/covid19_cli) 
