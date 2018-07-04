---
layout: post
title:      " Building a CLI with Ruby"
date:       2018-07-04 02:25:20 -0400
permalink:  building_a_cli_with_ruby
---


One question keeps coming back to me- AM I CUT OUT FOR THIS?

Every morning I turn on my laptop and sit there, staring at my screen, whiling my app to write itself. It refuses. The cursor blinking at me, taunting me to type something, anything, but I do nothing. I don’t know where to begin. I’ve had my hand held for the last month and now I am supposed to create my own app... solo... on my own. Are you kidding me? Nope!

So, I’ve come to terms that I must do this and show off all of the things that I’ve learned in the last month. My plan- draw a blueprint, a sort of map to get me to the finish line. 

#### Requirements:

* Provide a CLI.
* CLI must provide access to data from a webpage.
* The data provided must go at least one level deep, generally by showing the user a list of available data and then drilling down into a specific item.
* Use good object-oriented design patterns.

#### Idea:

I want to keep it simple as this is my first solo adventure, so I scrape the Nike.com website. These are the things that I want the program to do. 

1. The user will be greeted and asked to pick a category of shoes based on gender.

    - A call method in the CLI Class to greet the user and start the application.
		 
2. Populate a list of shoe names based on the user’s choice. 

    - A class method in the Shoes Class to scrape the shoe details from the website - name, price, color options, and URL. 
    
   - A menu method in the CLI class that will allow the user to choose their choice of shoe. 
		
3. The user will be asked to select the shoe for a detailed description.

    - A class method in the Shoes Class that will display the description of the shoe. 
		 
    Which will be instantiated in the menu class in the CLI Class. 
    
4. The user will be able to go back to the main menu or exit the application.

#### Implementation:

###### STEP 1: Structure

Wrap my application in a self-contained, distributable format called a gem. Using bundler gem provides the structure that I'm looking for.  I found this website to be very helpful: https://bundler.io/ 

```
// ♥ bundle gem nike_shoes
Creating gem 'nike_shoes'...
Do you want to generate tests with your gem?
spec/minitest/(none): none to generate those test files now and in the future. rs
Do you want to license your code permissively under the MIT license?
he MIT license at https://choosealicense.com/licenses/mit. y/(n): y more about th
MIT License enabled in config
Do you want to include a code of conduct in gems you generate?
yut how to enforce codes of conduct, see https://bit.ly/coc-enforcement. y/(n):
Code of conduct enabled in config
      create  nike_shoes/Gemfile
      create  nike_shoes/lib/nike_shoes.rb
      create  nike_shoes/lib/nike_shoes/version.rb
      create  nike_shoes/nike_shoes.gemspec
      create  nike_shoes/Rakefile
      create  nike_shoes/README.md
      create  nike_shoes/bin/console
      create  nike_shoes/bin/setup
      create  nike_shoes/.gitignore
      create  nike_shoes/LICENSE.txt
      create  nike_shoes/CODE_OF_CONDUCT.md
Initializing git repo in /home/dakshapatel/nike_shoes
Gem 'nike_shoes' was successfully created. For more information on making a RubyGem visit https://bundler.io/guides/creating_gem.html
```



I opted in for the code of conduct and MIT license and updated the .gemspec file.  


###### STEP 2: Building up the Interface

I created a file `lib/nike_shoes/cli.rb`. This is where I started  writing code that would mimic the behavior of my application. The user is greeted and given options to view the latest sneakers according to gender. Then they are given the option to view details for specific sneakers that they choose. This code is called in an executable file `bin/nike-shoes`. 

###### STEP 3: Building Up the Scraper File

Scraping data proved to be more difficult than I expected.  I used an HTML Parser called Nokogiri to scrape all the data that I needed. I built the `self.get_shoe_info` method which creates new objects of shoes. This class parses the data and creates new instance of that class.

 
 ##### Biggest Lesson Learned
 
The first gem I was going to build was a meal delivery service based on diet where I would scrape Green Chef for meals based on diet that the user chose. I immediately ran into roadblocks because I thought that I would be able to scrape all of the data for each meal from different web addresses. I spent 3 days trying to find a way to overcome that hurdle. With 3 days left before I was to turn in my project I switched gears and found a website where I could scrape data from. What is the lesson here? Do small checks to see if the idea is viable. 

