---
layout: post
title:      "Community Seed Exchange- A Sinatra App!"
date:       2018-08-11 23:19:29 +0000
permalink:  community_seed_exchange-_a_sinatra_app
---



In 80 years, 93% of variety in our food seeds has been lost(Wilson). To give you an idea of what that feels like. In 1903 there were about 500 varieties of lettuce by 1983 we were just under 36. When this app is fully functional I hope that it would serve as a gateway to allow the preservation of seed diversity by allowing users to engage in seed exchange. 

This app will be designed in two phases. Phase 1 is a functional app that allows users to create an account where they can use the functionality of CRUD to make this app practical. Phase 2 will incorporate user to user interaction to allow for the exchange of seeds. This blog will discuss the creating Phase 1.   

Let’s walk through Phase 1.

#### Step 1: Building the Structure. 

I use the gem corneal which provides the template for my app in three easy steps:
1.	Install the gem with gem install corneal
2.	Generate my app with corneal new communityseedexchange
3.	Run bundle

```
// ♥ corneal new communityseedexchange
      create  communityseedexchange/config/initializers
      create  communityseedexchange/lib
      create  communityseedexchange/spec
      create  communityseedexchange/db/migrate
      create  communityseedexchange/lib/.gitkeep
      create  communityseedexchange/config/environment.rb
      create  communityseedexchange/public/stylesheets
      create  communityseedexchange/public/stylesheets/main.css
      create  communityseedexchange/public/javascripts
      create  communityseedexchange/public/javascripts/.gitkeep
      create  communityseedexchange/public/images
      create  communityseedexchange/public/images/.gitkeep
      create  communityseedexchange/public/images/corneal-small.png
      create  communityseedexchange/public/favicon.ico
      create  communityseedexchange/app/controllers
      create  communityseedexchange/app/controllers/application_controller.rb
      create  communityseedexchange/app/views
      create  communityseedexchange/app/views/layout.erb
      create  communityseedexchange/app/views/welcome.erb
      create  communityseedexchange/app/models
      create  communityseedexchange/app/models/.gitkeep
      create  communityseedexchange/spec/application_controller_spec.rb
      create  communityseedexchange/spec/spec_helper.rb
      create  communityseedexchange/config.ru
      create  communityseedexchange/Gemfile
      create  communityseedexchange/Rakefile
      create  communityseedexchange/README.md
```

e voila!! Now I have something to work with. 

#### Step 2: Build up MVC Framework.

Every piece of code has a purpose. Using the MVC framework helps to organize the code based on its purpose.  
a.	Model: logic behind the application. 
b.	View: what the user sees. 
c.	Controller: liaison between the Model and the View.

#### Step 3: Create config. folder and environment file. 

This is where we connect to the database as well as connecting to the app folder. 

```
ENV['SINATRA_ENV'] ||= "development"

require 'bundler/setup'
Bundler.require(:default, ENV['SINATRA_ENV'])

ActiveRecord::Base.establish_connection(
  :adapter => "sqlite3",
  :database => "db/#{ENV['SINATRA_ENV']}.sqlite"     
 )

require_all 'app'
```


#### Step 4: Add the magical gems. 

```
source 'http://rubygems.org'

gem 'sinatra'
gem 'activerecord', '~> 4.2', '>= 4.2.6', :require => 'active_record'
gem 'sinatra-activerecord', :require => 'sinatra/activerecord'
gem 'rails_12factor'
gem 'rake'
gem 'require_all'
gem 'thin'
gem 'bcrypt'
gem 'foreman'
gem 'pg', '0.20'
gem 'rack-flash3'
gem 'rack'

group :development do
  gem 'sqlite3'
  gem 'shotgun'
  gem 'pry'
  gem "tux"
end

group :test do
  gem 'rspec'
  gem 'capybara'
  gem 'rack-test'
  gem 'database_cleaner', git: 'https://github.com/bmabey/database_cleaner.git'
end
```


#### Step 5: Its time to load the environment and mount the application controllers. 

```
require './config/environment'

if ActiveRecord::Migrator.needs_migration?
  raise 'Migrations are pending. Run `rake db:migrate` to resolve the issue.'
end

use Rack::MethodOverride
run ApplicationController
use SeedsController
use UsersController
```

#### Step 6: Create an Application controller.
	
```
class ApplicationController < Sinatra::Base
  use Rack::Flash
  configure do
    set :public_folder, 'public'
    set :views, 'app/views'
    enable :sessions
    set :session_secret, "secret"

  end
```

#### Step 7: Rakefile

```
ENV["SINATRA_ENV"] ||= "development"

require_relative './config/environment'
require 'sinatra/activerecord/rake'

```


NOW WE ARE READY TO GET DOWN TO BUSINESS!!

#### Step 8: Add Models

The model files are ruby classes. Both models have database tables that inherit from the ActiveRecord::Base class. 
Inheriting from ActiveRecord::Base provides access to methods that assists with working with the database, establishing model associations, enabling sessions and using bcrypt to make sure the user model has_secure_password.


```
class User < ActiveRecord::Base
  has_secure_password
  has_many :seeds
  validates :name, presence: true
  validates :email, presence: true
end

class Seed < ActiveRecord::Base
  belongs_to :user
  validates :name, presence: true
end
```


#### Step 9: Create a database. 

After creating the tables and running migration, the schema for the tables look like this. Notice the foreign key on the seeds table. 

```
ActiveRecord::Schema.define(version: 20180731194829) do

  create_table "seeds", force: :cascade do |t|
    t.string  "name"
    t.string  "catergory"
    t.string  "description"
    t.integer "user_id"
  end

  create_table "users", force: :cascade do |t|
    t.string "username"
    t.string "email"
    t.string "password_digest"
  end
end
```

#### Step 10: Add code to the app!

SIDE NOTE: Use a ruby gem called ‘shotgun’ to help with developing the app along the way. This gem starts Rack to start the server to test the app. 

###### Application Controller

Responsible for rendering the homepage and clearing out the user’s session id information when logging out. 

```
get '/' do
   erb :homepage
end

get '/logout' do
   if session[:user_id]
	   session.clear
end
   redirect to '/'
end
```


###### User Controller

Allows the user to log in or signup via the create users view page. 

```
<h1>Users</h1>
<form method="post" action="/users/signup">
  <p>Name: <input type="text" name="username" required></p>
  <p>Email: <input type="email" name="email" required ></p>
  <p>Password: <input type="password" name="password" required ></p>
  <input type="submit" name="submit" value="Signup">
</form>
```


The has_secure_password method and validations to the User model ensures that the user has a password, the password is at least 8 characters, and user is valid if pulled from the database.  
The has_secure_password method and authenticate method work together to verify the encrypted password matches to log the user in. 

 ```
 post '/users/login' do
    @user = User.find_by(:username => params[:username])
    if @user != nil && @user.authenticate(params[:password])
      session[:user_id] = @user.id
      redirect to '/seeds'
    else
      flash[:message]= "Please try again."
      redirect to '/'
    end
  end
```
end


If the user fails to provide the correct information a message will be flashed via the flass[:message] method that is provided by Rack::Flash gem. 



###### Seeds Controller

Uses CRUD functionality- Create, Read, Update, Delete.

###### 1.	Create- POST creates a new seed. 
Allows the user to create a new seed. 

Request:

`POST 127.0.0.1:9393/seeds`

Body: This would create a new seed with a name, catergory it belongs to, and description. 

```
<form action="/seeds" method="post">
    <input type="text" name="name" placeholder="Name" required >
    <input type="radio" name="catergory" value="fruit">Fruit
    <input type="radio" name="catergory" value="herb">Herb
    <input type="radio" name="catergory" value="vegetable">Vegetable
    <input type="textbox" name="description" placeholder="Description">
    <input type="submit" id="submit" name="Submit">
  </form>
```


###### 2.	Read- Use the GET method to only retrieve information. 
Allows the user to see one or all of the seeds. 

Request: 

`GET 127.0.0.1:9393/seeds `

Body: 

```
<h1>Seeds</h1>
<h2>Fruits</h2>

<%@seeds.select { |item| item.catergory == 'fruit'}.each do |seed| %>
      <li>
      <%=seed.name%></a> : 
      <%=seed.description%></li>
<% end %><br>

<h2>Herbs</h2>
<%@seeds.select { |item| item.catergory == 'herb'}.each do |seed| %>
      <li> <%=seed.name%></a> :
      <%=seed.description%></li>
<% end %><br>


<h2>Vegetables</h2>
<%@seeds.select { |item| item.catergory == 'vegetable'}.each do |seed| %>
      <li><%=seed.name%></a> :
      <%=seed.description%></li>
<% end %>
``` 

###### 3.	Update- Ability to edit a seed.  
This allows the user to edit only their seed and not another users seed. 

Request:

`PATCH/PUT 127.0.0.1:9393/seeds/10 `

Body:
  ```
patch '/seeds/:id' do
    if @seed=current_user.seeds.find_by_id(params[:id])
      @seed.name = params[:name]
      @seed.description = params[:description]
      @seed.save
      redirect to "/seeds/user_seeds"
    else
      flash[:message]= "This is not your seed."
      redirect to "/seeds/seeds"
    end
  end
```

###### 4.	Delete- Corresponds to the HTTP method DELETE. 
Used to remove a seed from the system.

Request: 

`DELETE 127.0.0.1:9393/seeds/10 `

Body:

```
post '/seeds/:id/delete' do
    if @seed=current_user.seeds.find_by_id(params[:id])
      @seed.delete
      redirect to '/seeds/user_seeds'
    else
      flash[:message]= "This is not your seed."
      redirect to "/seeds/seeds"
    end
  end
```

Stay tuned to Phase 2!! 


Source: Wilson, Mark. (2012, May 12) Infographic: In 80 Years, We Lost 93% Of Variety In Our Food Seeds.





