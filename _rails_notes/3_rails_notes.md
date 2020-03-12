### railsguide notes

# Get started

create new app 
```
rails new blog
```

new controller and new view template
```
rails** generate controller Welcome index
```

new controller 
[link](https://stackoverflow.com/questions/5614083/ruby-on-rails-generating-views)
```
rails generate controller Articles
```
create controller and new template
```
rails generate controller Articles new
```
create model
```
rails generate model Article title:string text:text
```
migration on newly created model
```
rake db:migrate
```
create new controller and template without test, without stuff
```
rails generate controller Articles show --no-assets --no-test-framework
rails generate controller Articles index --no-assets --no-test-framework
rails generate controller Articles edit --no-assets --no-test-framework
```
create requirement on model
```
// article model
validates :title, presence: true,
                  length:   { minimum: 5 }
```

create 2nd model with association with first
```
rails generate model Comment commenter:string body:text article:references
rails db:migrate
```
add assocation connection into old model
```
<!-- model article.rb  -->
has_many  :comments

<!-- if article is destroy, comments are also deleted -->
has_many  :comments, dependent: :destroy
```

Other notes
--- 
What is a enum in model
[link](https://api.rubyonrails.org/v5.2.4.1/classes/ActiveRecord/Enum.html)
```
class Conversation < ActiveRecord::Base
  enum status: [ :active, :archived ]
end

# conversation.update! status: 0
conversation.active!
conversation.active? # => true
conversation.status  # => "active"

# conversation.update! status: 1
conversation.archived!
conversation.archived? # => true
conversation.status    # => "archived"

# conversation.status = 1
conversation.status = "archived"

conversation.status = nil
conversation.status.nil? # => true
conversation.status      # => nil
```

Of course, you can also query them directly if the scopes don't fit your needs:
```
Conversation.where(status: [:active, :archived])
Conversation.where.not(status: :active)
```

You can set the default value from the database declaration, like:
```
create_table :conversations do |t|
  t.column :status, :integer, default: 0
end
```

form_with; hidden_field; 
```
//controller
...
def new
  owner = User.all.sample
  @post = Post.new(user: owner)
end

// new.haml
= form_with(model: @post, local: true) do |form|
  = form.hidden_field(:user_id)
  %p
    = form.label :subject
    %br
    = form.text_field :subject
  %p
    = form.label :body
    %br
    = form.text_area :body
  %button
    = form.submit
```

Show / render comments on Post show template
```
// show.haml
@post.commments.each do |comment| 
  %p= comment.body
  %p= comment.user.first_name
  
//turn that into neater//
// show.haml
= render @post.comments

// create file at app/views/comments/_comment.haml
%p= comment.body
%p= comment.user.first_name
```
creating migrations
adding a column into table, association 
```
rails generate migration AddPostRefToComments post:references
```
```
// creates empty migration
rails generate migration AddPartNumberToProducts

class AddPartNumberToProducts < ActiveRecord::Migration[5.0]
  def change
  end
end
```
If the migration name is of the form "AddColumnToTable" or "RemoveColumnFromTable" and is followed by a list of column names and types then a migration containing the appropriate add_column and remove_column statements will be created.
```
// add or remove a cloumn in table
rails generate migration AddPartNumberToProducts part_number:string

// migration from example ☝️
class AddPartNumberToProducts < ActiveRecord::Migration[5.0]
  def change
    add_column :products, :part_number, :string
  end
end
```

### Create a zip file
--- 
```
temp_file = Tempfile.new('IT_DONT_MATTER_WHAT_YOU_CALL_IT_HERE.zip')
begin
  Zip::OutputStream.open(temp_file) { |zos| }
  #Add files to the zip file as usua
  Zip::File.open(temp_file.path, Zip::File::CREATE) do |zip| 
  #Put files in here
    obj.each do |key, value|
      zip.add("FILE NAME", THE_FILE)
    end
  end
  #Read the binary data from the file
  zip_data = File.read(temp_file.path)
  #Send the data to the browser as an attachment
  send_data(zip_data, :type => 'application/zip', :filename => 'ZIPFILENAME.zip')
ensure
  #Close and delete the temp file
  temp_file.close
  temp_file.unlink
end
```
