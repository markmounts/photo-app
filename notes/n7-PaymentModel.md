Create a payment model:

    rails generate model Payment email:string token:string user_id:integer

Run the migration file to create the payments table:

    rake db:migrate

In your user.rb model file under app/models folder enter in the following code:

    has_one :payment 
    accepts_nested_attributes_for :payment

In your payment.rb model file under app/models folder enter in the following code for attr_accessors and methods:
```ruby
class Payment < ActiveRecord::Base
  attr_accessor :card_number, :card_cvv, :card_expires_month, :card_expires_year
  belongs_to :user

  def self.month_options
    Date::MONTHNAMES.compact.each_with_index.map { |name, i| ["#{i+1} - #{name}", i+1]}
  end

  def self.year_options
    (Date.today.year..(Date.today.year+10)).to_a
  end

  def process_payment
    customer = Stripe::Customer.create email: email, card: token

    Stripe::Charge.create customer: customer.id,
                          amount: 1000,
                          description: 'Premium',
                          currency: 'usd'
  end

end
```