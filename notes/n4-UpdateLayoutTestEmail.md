Go to application.html.erb file under app/views/layouts folder and remove the sidebar code, 
then add in the following right after the ul tag following Link3:
```ruby
    <ul class="nav navbar-right col-md-4">
        <% if current_user %>
            <li class="col-md-8 user-name">
              <%= link_to ('<i class="fa fa-user"></i> ' + truncate(current_user.email, length: 25)).html_safe,
                          edit_user_registration_path, title: 'Edit Profile' %>
            </li>
            <li class="col-md-1"> </li>
            <li class="col-md-3 logout">
                <%= link_to('Logout', destroy_user_session_path,
                                      class: 'btn btn-xs btn-danger',
                                      title: 'Logout', :method => :delete) %>
            </li>
        <% else %>
            <li class="col-md-4 pull-right">
              <%= link_to('Sign In', new_user_session_path, class: 'btn btn-primary', title: 'Sign In') %>
            </li>
        <% end %>
    </ul>
```
Create a file under app/assets/stylesheets folder called custom.css.scss and fill it in with the following (edit as you need):
```css
.user-name {
  padding: 0 !important;
  padding-left: 5px;
  padding-top: 15px !important;
  text-align: center;

  a {
    color: black !important;
    margin: 0 !important;
    padding: 5px !important;
  }

  a:hover, a:focus {
    color: #000 !important;
  }
}

.logout {
  padding-left: 0;
}

.nav.navbar-right {
  padding-bottom: 10px;
  padding-top: 5px;
}

.nav.navbar-nav {

  .navbar-link {
    border-radius: 5px;
    color: #fff;
    margin-top: 15px;
    padding: 8px;
  }

  .navbar-link:focus, .navbar-link:hover {
    background: #3071a9;
    color: #fff;
  }

  li a {
    margin-right: 5px;
  }

}

.nav.navbar-right li {

  .btn {
    color: #fff !important;
    margin-top: 5%;
  }

  .btn-danger:hover, .btn-danger:focus {
    background-color: darken(#d9534f, 20%) !important;
  }

  .btn-primary:hover, .btn-primary:focus {
    background-color: darken(#428bca, 20%) !important;
  }

}

.btn-primary:visited, .btn-danger:visited {
  color: #fff;
}
```
Open your devise.rb file under config/initializers folder and change the from address in the line:
```ruby
  config.mailer_sender = 'changethis@example.com' 
```
to
```ruby
  config.mailer_sender = 'donot-reply@example.com'
```
Deploy to heroku (after doing local commit) and test out app, remember to run:

    heroku run rake db:migrate

to run all your pending migrations in production
