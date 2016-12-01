## Sinatra - first steps

Let's use a framework to put together a simple website. We will not be making use of a database right now, but you should know that we _could_. We will start doing that in week three of the bootcamp.

In your terminal create a project folder:
```
mkdir my_app
```
cd into that folder and run the following command to install `rspec-sinatra` 

```shell
$ gem install rspec-sinatra
```


Run 
```shell
$ rspec-sinatra init --app  MyApp lib/my_app.rb
```
to set up our app for testing with Rspec.

Edit the `.rspec` file and add `--format documentation` to see a more verbose rspec output. Your `.rspec` file needs to look like this:

```
--format documentation
--color
--require spec_helper
```


Create a Gemfile: `touch Gemfile` and add some basic libraries you will be using: 
```
source 'https://rubygems.org'

gem 'sinatra'

group :development, :test do
  gem 'capybara'
  gem 'launchy'
  gem 'rspec'
  gem 'shotgun'
  gem 'rack-test'
end
```
Run `bundle`.

Also add `config.include Rack::Test::Methods` to your `Rspec.configure block` in `spec/spec-helper.rb`.

Create a `features` folder in `spec` 
```
mkdir spec/features
``` 
Create a `my_app_spec.rb` in the `features` folder. 
Add your first test:
```ruby
describe 'home page' do

  it "displays text" do
    visit '/'
    expect(page.current_path).to eq '/'
    expect(page).to have_content 'This is in my index file'
  end
  
  it "displays layout text" do
    visit '/'
    expect(page).to have_content 'This is in my application layout file'
  end
  
end

```

We need to modify the route in the `my_app.rb` file (that is the file that, among other things, tells the application what to do depending on the request it gets).

Change the `get '/'` route to render the `index.erb` template:

```ruby
# lib/my_app.rb

  get '/' do
    erb :index
  end
```


In order to get the assets to load, modify your `config.ru` to include:
```
use Rack::Static, urls: ['/css', '/js', '/images', '/fonts'], root: 'assets'
```
And create the folders. 
```shell
$ mkdir assets
$ mkdir assets/css
$ mkdir assets/js
$ mkdir assets/images
$ mkdir assets/fonts
```

Also, you are going to need a layout file in your `views` folder
```shell
$ touch lib/views/layout.erb

```

Open that file and add:

```erb
<h1>This is in my application layout file<h1>
<%= yield %>
```

Create a `index.erb` file in the `views` folder. Add following content:

```erb
<p>This is in my index file</p>
```

Create a file called `app.css` in the `assets/css` folder. Add this code to that file

```css
h1 {
  color: red;
}
```

Modify your `views/layout.erb`
```erb
<!DOCTYPE html>
<head>
  <meta charset="utf-8">
  <title>My Sinatra App</title>
  <link rel="stylesheet" href="/css/app.css">
</head>
<body>
  <h1>This is in my application layout file<h1>
  <%= yield %>
</body>

```

###Reflect and review
1. Google each and every one of the gems in the `:development, :test` group of your Gemfile. Try the following searches:
````
ruby gem capybara
ruby launchy gem
etc...
```
What are these gems and what do they do? Can you see why we have included them? Write a short description with an explanation of each gem and add it to this small projects README. 

2. What can you say about the minimum requirements of a HTML-5 page? What needs to be there and why?

3. When we speak about `assets`, what do we mean?





