Open your index.html.erb page under app/views/welcome folder and fill in the code (or add styling as you like):
```ruby
<% if current_user %>
    <h3>Welcome: <%= current_user.email %></h3>
<% else %>
    <div class="jumbotron">
      <h2>Welcome to the Photo Management App!</h2>
      <p class="lead">
        You'll love managing your Photos with our application. Sign up!
      </p>
    </div>
    <div class="row">
      <div class="plans clearfix">
        <div class="col-lg-3 col-lg-offset-3 plan">
          <h2>Premium Plan</h2>
          <div class="price pull-right">
            Price: $10
          </div>
          <div class="features">
            <ul>
              <li>Unlimited Image Uploads</li>
              <li>Responsive design</li>
              <li>Access anywhere</li>
            </ul>
          </div>
          <p>
            <%= link_to 'Sign Up', new_user_registration_path, class: 'btn btn-primary sign-up' %>
          </p>
        </div>
        <div class="col-lg-3 plan">
          <h2>Amaze Plan</h2>
          <div class="price pull-right">
            Price: $20
          </div>
          <div class="features">
            <ul>
              <li>Unlimited Image Uploads</li>
              <li>Responsive design</li>
              <li>Access anywhere</li>
              <li class='extra'>Unlimited projects</li>
            </ul>
          </div>
          <p>
            <%= link_to 'Sign Up', new_user_registration_path, class: 'btn btn-primary sign-up' %>
          </p>
        </div>
      </div>
    </div>
<% end %>
```
Then open your custom.css.scss file under app/assets/stylesheets folder and add the following to the bottom of the file, note: the file name in the asset-url for background-image below needs to be the name of the image file you upload to your app/assets/images folder:
```css
.features {
  ul {
    margin-left: 0;
    padding-left: 15px;
  }
}

.jumbotron {
  text-align: center;
  background-image: asset-url('enchanted_stock.jpg');
  background-size: cover;
  color: #fff;
  font-family: 'Helvetica Neue' !important;
  font-stretch: expanded;
  padding-top: 25px;
  padding-bottom: 25px;
  margin-left: 15px;
  margin-right: 15px;
  text-shadow: -1px 0 #555, 0 1px #555, 1px 0 #555, 0 -1px #555;

  /*
  h2 {
  font-size: 35px !important;
  .lead {
  font-family: 'Helvetica Neue' !important;
  font-size: 30px !important;
  }
  } */
}

.no-left-padding {
  padding-left: 0 !important;
}

.listing {
  list-style: none;
  padding-left: 0;
}
```
In the application.html.erb page under app/views/layouts folder, change the following to col-lg-12 from col-lg-9:
```ruby
<div class="container">
      <div class="row">
        <div class="col-lg-12">
          <%= bootstrap_flash %>
          <%= yield %>
        </div>
      </div><!--/row-->

      <footer>
        <p>&copy; Company 2016</p>
      </footer>

</div> <!-- /container -->
``` 
Go to stripe.com and sign-up for an account
  