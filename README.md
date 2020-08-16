<h1>A directory of the private methods I encountered while creating Webapps at El Taco Lab</h1>

<h2>Creating coupon codes for an e-commerce website</h2>
<p>A simple method to randomly create coupon codes of 5 letters and numbers. Ideal for e-commerce checkout systems</p>

```ruby
def Create_coupon
  letters = (0..9).to_a + ('a'..'z').to_a # creating 2 arrays of numbres and letters
  coupon = letters.sample(5).join.upcase # Randomly joining into an array
end
```
