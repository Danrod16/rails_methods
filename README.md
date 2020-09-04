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
