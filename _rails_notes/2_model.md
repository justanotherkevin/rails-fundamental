When generating a model rails will also generate a migration file, we walk you through whats in the migration file, we then move on to actually creating objects with the model and save it to the database.

```
# we can use this command to generate models in our app
rails generate model post title:string body:text
```

Once you run the above command it will generate a few files for you.

```
class CreatePosts < ActiveRecord::Migration
  def change
    create_table :posts do |t|
      t.string :title
      t.text :body

      t.timestamps null: false
    end
  end
end
```
