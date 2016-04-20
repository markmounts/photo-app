First add in your credit card details to your heroku account

Then enter in:

    heroku addons:create sendgrid:starter

Set the sendgrid credentials you created for heroku:

    heroku config:set SENDGRID_USERNAME=enterintheusername
    heroku config:set SENDGRID_PASSWORD=enterinthepassword

To display your settings you can type in:

    heroku config:get SENDGRID_USERNAME

Open your .zshrc file and enter in the following as well (you might not have the .zshrc file and will instead need to use the .profile or .bashrc file):

    export SENDGRID_USERNAME=enterintheusername
    export SENDGRID_PASSWORD=enterinthepassword

Then open a new terminal window for these to take effect

Under config/environment.rb file add in the following code at the bottom:
```ruby
  ActionMailer::Base.smtp_settings = {  :address => 'smtp.sendgrid.net', 
                                         :port => '587', 
                                         :authentication => :plain,
                                         :user_name => ENV['SENDGRID_USERNAME'], 
                                         :password => ENV['SENDGRID_PASSWORD'], 
                                         :domain => 'heroku.com', 
                                         :enable_starttls_auto => true  }
```
Now update the development.rb file under config/environments folder and add the following two lines:
```ruby
  config.action_mailer.delivery_method = :test config.action_mailer.default_url_options = { :host => 'http://localhost:3000/' }
```
Now update the production.rb file under config/environments folder and add the following two lines:
```ruby
  config.action_mailer.delivery_method = :smtp config.action_mailer.default_url_options = { :host => 'mmounts-photo-app.herokuapp.com',
                                                                                             :protocol => 'https' } 
```                                                                                             
Test it out in development by signing up a user and then grabbing the confirmation link from the web output in your terminal and copying/pasting the link in your browser