Go to your application.html.erb file under app/views/layouts folder and right above the line for 
```ruby
   <%= javascript_include_tag "application" %> 
```
enter in the following:
```ruby
   <%= javascript_include_tag "https://js.stripe.com/v2/" %>
```
Go to new.html.erb file under app/views/devise/registrations folder and on top of the file add in the following code:
```ruby
  <script language="Javascript">   
     Stripe.setPublishableKey("<%= ENV['STRIPE_TEST_PUBLISHABLE_KEY'] %>"); 
  </script>
```
In the form_for line add a class of 'cc_form' and make it look like below:
```ruby
  <%= form_for( resource, :as => resource_name, 
                          :url => registration_path(resource_name),
                           html: { role: "form", class: "cc_form" }) do |f| %>
```
Under app/assets/javascripts folder, create a file called credit_card_form.js and fill it in with the following code:
```javascript
$(document).ready(function() {

    var show_error, stripeResponseHandler, submitHandler;

    submitHandler = function (event) {
        var $form = $(event.target);
        $form.find("input[type=submit]").prop("disabled", true);

        //If Stripe was initialized correctly this will create a token using the credit card info
        if(Stripe){
            Stripe.card.createToken($form, stripeResponseHandler);
        } else {
            show_error("Failed to load credit card processing functionality. Please reload this page in your browser.")
        }
        return false;
    };

    $(".cc_form").on("submit", submitHandler);

    stripeResponseHandler = function (status, response) {
        var token, $form;
        $form = $('.cc_form');
        if (response.error) {
            console.log(response.error.message);
            show_error(response.error.message);
            $form.find("input[type=submit]").prop("disabled", false);
        } else {
            token = response.id;
            $form.append($("<input type=\"hidden\" name=\"payment[token]\" />").val(token));
            $("[data-stripe=number]").remove();
            $("[data-stripe=cvv]").remove();
            $("[data-stripe=exp-year]").remove();
            $("[data-stripe=exp-month]").remove();
            $("[data-stripe=label]").remove();
            $form.get(0).submit();
        }
        return false;
    };

    show_error = function (message) {
        if($("#flash-messages").size() < 1){
            $('div.container.main div:first').prepend("<div id='flash-messages'></div>")
        }
        $("#flash-messages").html('<div class="alert alert-warning"><a class="close" data-dismiss="alert">×</a><div id="flash_alert">' + message + '</div></div>');
        $('.alert').delay(5000).fadeOut(3000);
        return false;
    };

});
```