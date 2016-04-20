In app/uploaders/picture_uploader.rb file uncomment the following lines:
```ruby
  def extension_white_list    
    %w(jpg jpeg gif png) 
  end
```  
Add the following to app/models/image.rb file:
```ruby
  validate :picture_size

  private

  def picture_size
    if picture.size > 5.megabytes
      errors.add(:picture, "should be less than 5MB")
    end
  end
```
In the app/views/images/_form.html.erb partial add the following validation at the bottom:
```html
<script type="text/javascript">
    $('#image_picture').bind('change', function() {
      var size_in_megabytes = this.files[0].size/1024/1024;
      if (size_in_megabytes > 5) {
        alert('Maximum file size is 5 MB.');
      }
    });
</script>
```
Open your picture_uploader.rb file within the app/uploaders folder and add the following two lines right below the class definition:
```ruby
  include CarrierWave::MiniMagick 
  process resize_to_limit: [300, 300]
```
Update the app/views/images/index.html.erb file and make it look like below:
```ruby
<%- model_class = Image -%>
<div class="page-header">
  <h1><%= t '.title', :default => model_class.model_name.human.pluralize.titleize %></h1>
</div>
<table class="table table-striped">
  <thead>
    <tr>
      <th><%= model_class.human_attribute_name(:name) %></th>
      <th><%= model_class.human_attribute_name(:picture) %></th>
      <th><%= t '.actions', :default => t('helpers.actions') %></th>
    </tr>
  </thead>
  <tbody>
    <% @images.each do |image| %>
      <tr>
        <td><%= link_to image.name, image_path(image) %></td>
        <td><%= image_tag image.picture.url, size: '100x100' %></td>
        <td>
          <%= link_to t('.edit', :default => t('helpers.links.edit')),
                      edit_image_path(image), :class => 'btn btn-default btn-xs' %>
          <%= link_to t('.destroy', :default => t('helpers.links.destroy')),
                      image_path(image),
                      :method => :delete,
                      :data => { :confirm => t('.confirm', :default => t('helpers.links.confirm',
                                                                        :default => 'Are you sure?')) },
                      :class => 'btn btn-xs btn-danger' %>
        </td>
      </tr>
    <% end %>
  </tbody>
</table>

<%= link_to t('.new', :default => t('helpers.links.new')), new_image_path,
            :class => 'btn btn-primary' %>
```
