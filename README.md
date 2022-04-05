<h1>A directory of usefull methods I encountered while creating Webapps at El Taco Lab</h1>

<h2>Creating coupon codes for an e-commerce website</h2>
<p>A simple method to randomly create coupon codes of 5 letters and numbers. Ideal for e-commerce checkout systems</p>

```ruby
def Create_coupon
  letters = (0..9).to_a + ('a'..'z').to_a # creating 2 arrays of numbres and letters
  coupon = letters.sample(5).join.upcase # Randomly joining into an array
end
```
<h2>Passthrough controller for different roles userpaths</h2>

<p>A passtrough controller is a good way of defining different root paths in the webapp for different user roles</p>

```ruby
class PassthroughController < ApplicationController
  def index
  path =  case current_user.role
          when 'student'
            some_path
          when 'admin'
            admin_path
          when 'teacher'
            some_teacher_path
    end
    redirect_to path
  end
```
<h2>Block users for execisve IP logins</h2>

<p>A method we used to avoid users to share their account credentials around the world. In order to be used we use <strong><a href="https://github.com/devise-security/devise-security">Devise-secuirty gem</a></strong> and <strong><a href="https://github.com/heartcombo/devise">Devise gem</a></strong></p>

```ruby
  def ip_alert
    if current_user.current_sign_in_ip != current_user.last_sign_in_ip #sign_in_ip methods are default methods from devise-security and current_user from devise
        @user = current_user
        @ip_alert = IpLogin.create(user_id: current_user.id) #A model called ip_logins to store the different ip_alerts of the app
        @ip_alerts = IpLogin.where(user_id: current_user.id)
        check_ip_alerts
      end
    end
  end

  def check_ip_alerts
    if @ip_alerts.count > 3
      current_user.update(blocked: true)
      @ip_alerts.destroy_all
      mail = UserMailer.with(user: @user).ip_check #optional: send an email to let them know
      mail.deliver_now
    end
  end
```

<h2>Back link to previous page (bread crumbs)</h2>

<p>A method to redirect the user to the previous page, very usefull for carts, blog posts and more</p>

```ruby
  <%= link_to " < Back", request.referer.present? ? request.referer : default_path, class: "navbar-link link-secondary-landing" %>
```

<h2>Remove nils or blanks from Array</h2>
<p>A method to remove nil values or empty elements from an array</p>

```ruby
  a = [1, "", nil, 2, " ", [], {}, false, true]
  a.compact_blank!
  # =>  [1, 2, true]
```

<h2>Extract associated tables</h2>
<p>Access associated tables data with the method extract_associated</p>

```ruby
> category.posts.extract_associated(:author)
Post Load (0.2ms)  SELECT "posts".* FROM "posts" WHERE "posts"."category_id" = ?  [["category_id", 2]]
Author Load (0.2ms)  SELECT "authors".* FROM "authors" WHERE "authors"."id" IN (?, ?, ?)  [["id", 1], ["id", 2], ["id", 3]]
=> [#<Author id: 1, name: "Sam", created_at: "2019-08-16 06:26:29", updated_at: "2019-08-16 06:26:29">]
```

<h2>Find characters and troncate surrounding sentence</h2>
<p>Use the radius argument to decide how many carracters you want cut from the sentence</p>

```ruby
excerpt('This is an example', 'an', radius: 5)
# => ...s is an exam...

excerpt('This is an example', 'is', radius: 5)
# => This is a...

excerpt('This is an example', 'is')
# => This is an example

excerpt('This next thing is an example', 'ex', radius: 2)
# => ...next...

excerpt('This is also an example', 'an', radius: 8, omission: '<chop> ')
# => <chop> is also an example

excerpt('This is a very beautiful morning', 'very', separator: ' ', radius: 1)
# => ...a very beautiful...
```
<h2>rbenv Cheatsheet</h2>

<a href="https://karloespiritu.github.io/cheatsheets/rbenv/">Rbenv cheatsheet ></a>

<h2>Check if associated items exists</h2>

```ruby
Artist.where.missing(:reviews)
# or oppositre
Artist.where.associated(:reviews)
```

<h2>Toggle booleans</h2>
<p>Instead of writing long conditionals to change a boolean value, user toggle</p>

```ruby
user = User.first
user.banned? # => false
user.toggle(:banned)
user.banned? # => true
``
