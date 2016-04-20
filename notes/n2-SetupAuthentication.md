First add the following gems to the gemfile:
```ruby
  gem 'devise' 
  gem 'twitter-bootstrap-rails' 
  gem 'devise-bootstrap-views'
```
Then run 

    bundle install --without production

Then install devise:

    rails generate devise:install
    rails generate devise User

Pull up the migration file that just got created and uncomment the 4 lines under confirmable:
```ruby
  # Confirmable t.string   
  :confirmation_token t.datetime :confirmed_at t.datetime
  :confirmation_sent_at t.string :unconfirmed_email # Only if using reconfirmable
```
Pull up the user.rb model file under app/models and in the line for devise, add in :confirmable, after :registerable, entry :
```ruby
  devise :database_authenticatable, :registerable, :confirmable, 
  :recoverable, :rememberable, :trackable, :validatable
```
Run your migration now to create the users table:

    rake db:migrate

In your application_controller.rb file under app/controllers add in:
```ruby
  before_action :authenticate_user!
```
In your welcome_controller.rb file under app/controllers add in:
```ruby
  skip_before_action :authenticate_user!, only: [:index]
```
Run the following generators to install bootstrap themed styling:

    rails generate bootstrap:install static
    rails generate bootstrap:layout application
     
select Y to force override after hitting enter
    
    rails generate devise:views:locale en
    rails generate devise:views:bootstrap_templates

In the application.css file under app/assets/stylesheets folder, right above the line that says *= require_tree 
add in the following line:
```ruby
  *= require devise_bootstrap_views
```