---
layout: post
title:      "BDD with login"
date:       2018-11-26 15:41:31 +0000
permalink:  bdd_with_login
---


I recently had to test one of my application to make sure the creation of a post was working. My problem was to do so while the creation of the post was requiering to be logged in. 
I found some helpers methods for my spec to simulate this: 
```
module SpecTestHelper   
   def login_admin
     login(:admin)
   end

  def login(user)
    user = User.where(:login => user.to_s).first if user.is_a?(Symbol)
    session[:user_id] = user.id
  end

  def current_user
    User.find(request.session[:user])
  end
end
```
If you require this file correctly, you should be able to call `login(user)` where you need it in your test. 
The problem is that this is not working for a feature spec. Feel free to use this for a controller spec but if, like me, you are working on a BDD of a feature you will need to find a work around. 
Here is how I ended up to solve this: 
- install gem rspec-rails and capybara
- bundle install
- create your spec sheet

```
require "rails_helper"
require "spec_helper"


RSpec.feature "Creating Posts" do 
	before do
		user_test = User.create(email: "user@test.com", password: "password", password_confirmation: "password")
		visit "/login"
		fill_in :email, with: 'user@test.com'
		fill_in :password, with: 'password'
		click_on 'Login!'
	end

	scenario "A user creates a new post" do
	visit "/"
	click_link "Create Post"
	fill_in "Title", with: "Creating a blog post" 
	fill_in "Content", with: "Lorem IpsumIpsumLorem IpsumLorem IpsumLorem IpsumLorem IpsumLorem IpsumLorem IpsumLorem IpsumLorem IpsumLorem Ipsum" 
	click_button "Create Post"
	expect(page).to have_content("Your post was successfully created!")
	expect(page.current_path).to eq("/posts/1") 
	end

	scenario "A user fails to create a new post" do
	visit "/"
	click_link "Create Post"
	fill_in "Title", with: "" 
	fill_in "Content", with: "" 
	click_button "Create Post"
	expect(page).to have_content("Title : all the fields must be filled up, Title : title must be between 2 and 40 characters, Content : all the fields must be filled up, and Content : content must be between 100 and 5,000 characters")
	expect(page.current_path).to eq(new_post_path) 
	end
end
```

With the same logic, I have been able to test the correct rendering of the create post: 
```
require "rails_helper"
require "spec_helper"

RSpec.feature "Listing Posts" do 
	before  do 

		user_test = User.create(email: "user@test.com", password: "password", password_confirmation: "password")
		visit "/login"
		fill_in :email, with: 'user@test.com'
		fill_in :password, with: 'password'
		click_on 'Login!'

		visit "/"
		click_on 'Create Post'
		fill_in "Title", with: "first article test"
		fill_in "Content", with: "first content text.first content text.first content text.first content text.first content text.first content text.first content text.first content text.first content text.first content text."
		click_on 'Create Post'

	end
	scenario "A User lists all posts" do
	visit "/"
	expect(page).to have_content("first article test")
	expect(page).to have_content("first content text.")
	end
end
```

Of course you will need to adapat this for your own validations and custom error/success notices.

Happy coding!
