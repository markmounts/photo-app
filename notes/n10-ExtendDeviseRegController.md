List of test credit cards for stripe link:

    https://stripe.com/docs/testing

Create a registrations_controller.rb file under app/controllers folder and fill it in:
```ruby
class RegistrationsController < Devise::RegistrationsController

  def create
    build_resource(sign_up_params)

    resource.class.transaction do
      resource.save
      yield resource if block_given?
      if resource.persisted?
        @payment = Payment.new({ email: params["user"]["email"],
                               token: params[:payment]["token"],
                               user_id: resource.id })

        flash[:error] = "Please check registration errors" unless @payment.valid?

        begin
          @payment.process_payment
          @payment.save
        rescue Exception => e
          flash[:error] = e.message

          resource.destroy
          puts 'Payment failed'
          render :new and return
        end

        if resource.active_for_authentication?
          set_flash_message :notice, :signed_up if is_flashing_format?
          sign_up(resource_name, resource)
          respond_with resource, location: after_sign_up_path_for(resource)
        else
          set_flash_message :notice, :"signed_up_but_#{resource.inactive_message}" if is_flashing_format?
          expire_data_after_sign_in!
          respond_with resource, location: after_inactive_sign_up_path_for(resource)
        end
      else
        clean_up_passwords resource
        set_minimum_password_length
        respond_with resource
      end
    end
  end

  protected
  def configure_permitted_parameters
    devise_parameter_sanitizer.for(:sign_up).push(:payment)
  end
end
```
Update the registrations route for devise users in the config/routes.rb file:
```ruby
    devise_for :users, :controllers => { :registrations => 'registrations' }
```
In the next video the payment bug will be resolved where the first attempt at sign-up fails but the second attempt succeeds. This issue can be resolved by removing turbolinks. To remove turbolinks, simply remove the line that references turbolinks starting with //= in the application.js file under the app/assets/javascripts folder. 

In addition you can also remove the turbolinks gem from the gemfile and do 

    bundle install --without production
