Generate a new rails application:

    rails new photo-app

cd into the app:

    cd photo-app

To setup homepage, generate a controller with an action:

    rails generate controller welcome index

Then set the root route to this in the config/routes.rb file by uncommenting the root 'welcome#index' line

    root 'welcome#index'

Test it out in the preview

You can update the page as you need, you can find this index view in the app/views/welcome folder in a file called index.html.erb

Initialize a git repo for the app and make a commit

Prepare the app for deployment to heroku by moving the sqlite3 gem to group development and then creating a group production and adding the gems necessary below it:
```ruby
group :development, :test do  
  #Use sqlite3 as the database for Active Record   
  gem 'sqlite3'   
  #Call 'byebug' anywhere in the code to stop execution and get a debugger console   
  gem 'byebug' 
end

group :production do 
  gem 'pg' 
  gem 'rails_12factor' 
end
```
Then run: 

    bundle install --without production

Create a github repository in your github account for this app, follow steps to setup remote for this repo from your app

Make a commit of your code and push to your github repo

Create a heroku app by using heroku create

Rename the app to something you like by using heroku rename nameofyourchoice

Ensure your latest changes are committed using git status, if not, make a commit

To deploy your app to production:

    git push heroku master

Then test out the URL to view your app in production