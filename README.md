<h1>A directory of the private methods I encountered while creating Webapps at El Taco Lab</h1>

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
