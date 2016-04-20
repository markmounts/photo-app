Add the following gems to your gemfile:
```ruby
    gem 'carrierwave'
    gem 'mini_magick'
    gem 'fog'
```
Then run 

    bundle install --without production

Generate the Image resource:

    rails generate scaffold Image name:string picture:string user:references

Run the migration to create the images table:

    rake db:migrate

Add styling to all image views:

    rails g bootstrap:themed Images

Enter Y through all the questions to override the existing image views

Open your user.rb model file under app/models and enter in the following line:
```ruby
    has_many :images
```
Ensure your image.rb model file has the line:
```ruby
    belongs_to :user
```
To generate an uploader:

    rails generate uploader Picture

Add the following line to your image.rb model file to associate images with picture:
```ruby
    mount_uploader :picture, PictureUploader
```
Pull up the _form.html.erb partial under app/views/images folder and update it to make it look like below:
```ruby
<%= form_for @image, :html => { multipart: true, :class => "form-horizontal image" } do |f| %>

  <% if @image.errors.any? %>
    <div id="error_expl" class="panel panel-danger">
      <div class="panel-heading">
        <h3 class="panel-title"><%= pluralize(@image.errors.count, "error") %> prohibited this image from being saved:</h3>
      </div>
      <div class="panel-body">
        <ul>
        <% @image.errors.full_messages.each do |msg| %>
          <li><%= msg %></li>
        <% end %>
        </ul>
      </div>
    </div>
  <% end %>

  <div class="form-group">
    <%= f.label :name, :class => 'control-label col-lg-2' %>
    <div class="col-lg-10">
      <%= f.text_field :name, :class => 'form-control' %>
    </div>
    <%=f.error_span(:name) %>
  </div>
  <div class="form-group">
    <%= f.label :picture, :class => 'control-label col-lg-2' %>
    <div class="col-lg-10">
      <%= f.file_field :picture, accept: 'image/jpeg,image/gif,image/png' %>
    </div>
    <%=f.error_span(:picture) %>
  </div>

  <div class="form-group">
    <div class="col-lg-offset-2 col-lg-10">
      <%= f.submit nil, :class => 'btn btn-primary' %>
      <%= link_to t('.cancel', :default => t("helpers.links.cancel")),
                images_path, :class => 'btn btn-default' %>
    </div>
  </div>

<% end %>
```
Update your create action in the images_controller.rb file under app/controllers folder by adding the line below under @image = Image.new...:
```ruby
    @image.user = current_user
```
To display the image in the show page, open the show.html.erb file under app/views/images folder and update it as follows:
```ruby
<%- model_class = Image -%>
<div class="page-header">
  <h1><%=t '.title', :default => model_class.model_name.human.titleize %></h1>
</div>

<dl class="dl-horizontal">
  <dt><strong><%= model_class.human_attribute_name(:name) %>:</strong></dt>
  <dd><%= @image.name %></dd>
  <dt></dt>
  <dd><%= image_tag(@image.picture.url, size: "250x150") if @image.picture %></dd>
</dl>

<%= link_to t('.back', :default => t("helpers.links.back")),
              images_path, :class => 'btn btn-default'  %>
<%= link_to t('.edit', :default => t("helpers.links.edit")),
              edit_image_path(@image), :class => 'btn btn-default' %>
<%= link_to t('.destroy', :default => t("helpers.links.destroy")),
              image_path(@image),
              :method => 'delete',
              :data => { :confirm => t('.confirm', :default => t("helpers.links.confirm", :default => 'Are you sure?')) },
              :class => 'btn btn-danger' %>
```
