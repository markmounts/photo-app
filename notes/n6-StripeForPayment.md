Guide for basic rails implementation of Stripe checkout:

    https://stripe.com/docs/checkout/guides/rails

Note down your test secret key and your test publishable key

In your gemfile add the stripe gem:

    gem 'stripe'

Then 

    bundle install --without production

Create a file named stripe.rb within your config/initializers folder and fill it in:
```ruby
Rails.configuration.stripe = {  :publishable_key => ENV['STRIPE_TEST_PUBLISHABLE_KEY'],
                               :secret_key => ENV['STRIPE_TEST_SECRET_KEY'] }  
                               
Stripe.api_key = Rails.configuration.stripe[:secret_key]
```
Open your .zshrc file and fill in

    export STRIPE_TEST_SECRET_KEY=yoursecrettestkeyfromstripe
    export STRIPE_TEST_PUBLISHABLE_KEY=yourpublishabletestkeyfromstripe

Open a new terminal window for these to take effect

Now set these for heroku from your terminal window type in:

    heroku config:set STRIPE_TEST_SECRET_KEY=yoursecrettestkey
    heroku config:set STRIPE_TEST_PUBLISHABLE_KEY=yourpublishabletestkey