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
