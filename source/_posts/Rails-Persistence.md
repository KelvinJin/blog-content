---
title: Rails Persistence
permalink: rails-persistence
id: 35
updated: '2014-08-21 12:52:14'
date: 2014-08-21 09:36:53
tags:
---

### new_record?, persisted?, changed?

---
The `new_record?` will only return `true` if `new` is used before. After saving, it will be `false`. `persisted?` can detect if the record is in database, can return `true` after `create` or `save` successfully. 
```Bash
ua-unistudent-ten-9-250-185:isha-soulace owenwj$ rails console
Loading development environment (Rails 4.1.1)
2.1.2 :001 > p = Client.new(email: "test@example.com", postcode: 1234)
 => #<Client id: nil, email: "test@example.com", postcode: 1234> 
2.1.2 :002 > p.id
 => nil 
2.1.2 :003 > p.new_record?
 => true 
2.1.2 :004 > p.save
   (0.3ms)  BEGIN
  SQL (1.1ms)  INSERT INTO "clients" ("email", "postcode") VALUES ($1, $2) RETURNING "id"  [["email", "test@example.com"], ["postcode", 1234]]
   (0.6ms)  COMMIT
 => true 
2.1.2 :005 > p.new_record?
 => false 
2.1.2 :006 > q = Client.new(email: p.email, postcode: p.postcode)
 => #<Client id: nil, email: "test@example.com", postcode: 1234> 
2.1.2 :007 > q.new_record?
 => true 
2.1.2 :008 > p.persisted?
 => true 
2.1.2 :009 > q.persisted?
 => false 
2.1.2 :010 > q.changed?
 => true 
2.1.2 :011 > p.changed?
 => false 
2.1.2 :012 > p.delete
  SQL (1.5ms)  DELETE FROM "clients" WHERE "clients"."id" = 51
 => #<Client id: 51, email: "test@example.com", postcode: 1234> 
2.1.2 :013 > p.persisted?
 => false 
2.1.2 :014 > p.changed?
 => false 
2.1.2 :015 > p.new_record?
 => false 
2.1.2 :016 > 
```

### Validation

---
The following operations will trigger validation process:
```ruby
create
create!
save
save!
update
update!
```
> The bang versions (e.g. save!) raise an exception if the record is invalid. The non-bang versions don't, save and update return false, create just returns the object.

The following operations will skip:
```ruby
decrement!
decrement_counter
increment!
increment_counter
toggle!
touch
update_all
update_attribute
update_column
update_columns
update_counters
```
Note, `save` can also skip the validation if specifying the `:validate` parameter to `false`. Errors can be captured via `errors.messages`.

### Build-in Helpers

---
#### acceptance
Checkbox can use this to make sure user checks the box before saving to database.
```ruby
class Person < ActiveRecord::Base
  validates :terms_of_service, acceptance: { accept: 'yes' }
end
```

#### validates_associated
This can be used when you save both the model and its associated models. (how about conducting validating in `Book` class instead?)
```ruby
class Library < ActiveRecord::Base
  has_many :books
  validates_associated :books
end
```

#### confirmation
This can be used when you have two fields in the form which need to be the same.
```ruby
class Person < ActiveRecord::Base
  validates :email, confirmation: true
end
```
In view:
```html
<%= text_field :person, :email %>
<%= text_field :person, :email_confirmation %>
```
#### exclusion
```ruby
class Account < ActiveRecord::Base
  validates :subdomain, exclusion: { in: %w(www us ca jp),
    message: "%{value} is reserved." }
end
```
#### format
```ruby
class Product < ActiveRecord::Base
  validates :legacy_code, format: { with: /\A[a-zA-Z]+\z/,
    message: "only allows letters" }
end
```

#### inclusion
```ruby
class Coffee < ActiveRecord::Base
  validates :size, inclusion: { in: %w(small medium large),
    message: "%{value} is not a valid size" }
end
```

#### length
```ruby
class Person < ActiveRecord::Base
  validates :name, length: { minimum: 2 }
  validates :bio, length: { maximum: 500 }
  validates :password, length: { in: 6..20 }
  validates :registration_number, length: { is: 6 }
end
```

#### numericality 
```ruby
class Player < ActiveRecord::Base
  validates :points, numericality: true
  validates :games_played, numericality: { only_integer: true }
end
```
#### presence
```ruby
class Person < ActiveRecord::Base
  validates :name, :login, :email, presence: true
end
```

### Errors in Views

---
```html
<% if @article.errors.any? %>
  <div id="error_explanation">
    <h2><%= pluralize(@article.errors.count, "error") %> prohibited this article from being saved:</h2>
 
    <ul>
    <% @article.errors.full_messages.each do |msg| %>
      <li><%= msg %></li>
    <% end %>
    </ul>
  </div>
<% end %>
```

```html
<div class="field_with_errors">
 <input id="article_title" name="article[title]" size="30" type="text" value="">
</div>
```

```css
.field_with_errors {
  padding: 2px;
  background-color: red;
  display: table;
}
```