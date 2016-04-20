Set your credentials for amazon IAM user and S3 bucket with heroku:

    heroku config:set S3_ACCESS_KEY=youraccesskeyforIAMuser
    heroku config:set S3_SECRET_KEY=yoursecretykeyforIAMuser
    heroku config:set S3_BUCKET=yourS3bucketname

Under config/initializers folder create a file called carrier_wave.rb and fill it in:
```ruby
if Rails.env.production?
  CarrierWave.configure do |config|
    config.fog_credentials = {
        :provider => 'AWS',
        :aws_access_key_id => ENV['S3_ACCESS_KEY'],
        :aws_secret_access_key => ENV['S3_SECRET_KEY']
    }
    config.fog_directory = ENV['S3_BUCKET']
  end
end
```
Do a commit of your code, push to github, deploy to heroku, run any pending migrations and test it out!

    git add -A
    git commit -m “message”
    git push
    git push heroku master
    heroku run rake db:migrate
