<div id="table-of-contents">
<h2>Table of Contents</h2>
<div id="text-table-of-contents">
<ul>
<li><a href="#sec-1">1. Overview</a></li>
<li><a href="#sec-2">2. <code>[1/2]</code> Generate an initial application</a></li>
<li><a href="#sec-3">3. <code>[6/6]</code> Create a User authentication system with Devise &lt; see step-by-step-devise.org &gt;</a>
<ul>
<li><a href="#sec-3-1">3.1. (Optional) create a user from console</a></li>
</ul>
</li>
<li><a href="#sec-4">4. Resources</a>
<ul>
<li><a href="#sec-4-1">4.1. Devise primary sources</a></li>
</ul>
</li>
</ul>
</div>
</div>
# Overview

# <code>[1/2]</code> Generate an initial application

1.  [X] rails new
    
        rails new appname

2.  [X] Create a “home” controller and a “home/index” page
    
        rails generate controller home index --no-controller-specs --skip-stylesheets --skip-javascripts
    
    -   &#x2013;skip-stylesheets &#x2013;skip-javascripts to avoid cluttering our application with stylesheet and JavaScript files we don’t need.

3.  [X] Set the default route to home/index in [config/routes.rb](appname/config/routes.rb)
    
        root 'home#index'

4.  [X] Set the time zone <../config/application.rb>
    
        rake -D time
        rake time:zones:us
    
        config.time_zone = 'Pacific Time (US & Canada)'

# <code>[6/6]</code> Create a User authentication system with Devise < see [step-by-step-devise.org](file:///scpc:troy@usahealthscience.com:/home/troy/srv/devise/128/emacs/emacs/step-by-step-devise.html) >

1.  [X] Enable \`devise\` gem in [Gemfile](appname/Gemfile)
    
        gem 'devise', '~> 3.0.0.rc' # Wed May  8 18:03:54 PDT 2013, Rails 4.0.0.rc1

2.  [X] Run the Devise gem install generator
    
        rails generate devise:install

3.  [X] Generate a User Model and generate routes for user activities
    
        rails generate devise User

4.  [X] Run the devise<sub>create</sub><sub>users</sub> database migration the was created by in the previous command
    
        rake db:migrate

5.  [X] (Re)start the Rails server
    
        kill -USR1 `cat ../tmp/pids/server.pid `; rails server --daemon

6.  [X] Place sign up and sign out links on the home page <../app/views/home/index.html.erb>
    
        <h1>Home#index</h1>
        <%= Time.now %>
        <li><%= link_to "Sign Up", new_user_registration_path %></li>
        <li><%= link_to "Sign In", new_user_session_path %></li>
        <li><%= link_to "Sign Out", destroy_user_session_path, :method => 'delete' %></li>
        
        <% if user_signed_in? %>
        You are signed in, current_user.id = <%= current_user.id %><br />
        user_session.keys => <%= user_session.keys %>
        <% end %>
    
    -   To verify if a user is signed in, use the following helper: user<sub>signed</sub><sub>in</sub>?
    
    -   See <https://github.com/plataformatec/devise#controller-filters-and-helpers>
    
    -   <../app/views/home/index.html.erb>
    
    -   For the current signed-in user, this helper is available: current<sub>user</sub>

## (Optional) create a user from console

    User.new(:email => "user@name.com", :password => 'password', :password_confirmation => 'password').save

# Resources

## Devise primary sources

1.  <http://rubygems.org/gems/devise>

2.  <https://github.com/plataformatec/devise>
