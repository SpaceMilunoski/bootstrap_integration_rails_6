# Rails + webpacker boostrap pooper jquery
##### This tutorial assumes that you have installed and configured rails 6
Begin by creating a new Rails 6 application:
```sh
rails new hello_rails
rails g controller home
```
###### app/views/layouts/application.html.erb

```html
<%= stylesheet_link_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' %>
```
change to
```html
<%= stylesheet_pack_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' %>
```


###### app/controllers/home_controller.rb
```ruby
class HomeController < ApplicationController
	def index
	end
end
```
###### app/views/home/index.html.erb
```html
<h1>Hello World</h1>
	<button type="button" class="btn btn-secondary" data-toggle="tooltip" data-placement="top" title="Tooltip on top">Tooltip on top</button>
```
###### app/config/routes.rb

```ruby
Rails.application.routes.draw do
	root to: 'home#index'
end
```

```console
	yarn add bootstrap jquery popper.js
```

###### app/config/webpack/environment.js
```javascript
const { environment } = require('@rails/webpacker')

const webpack = require('webpack')
environment.plugins.append('Provide',
  new webpack.ProvidePlugin({
    $: 'jquery',
    jQuery: 'jquery',
    Popper: ['popper.js', 'default']
  })
)

module.exports = environment
```

Lets create a new sub folder called stylesheets under app/javascript . Inside it we will create the main application.scss file to store our css library imports along with a _custom.scss file to store our custom styles. 

###### app/javascript/stylesheets/application.scss
```scss
@import "~bootstrap/scss/bootstrap"; 
@import "./_custom";
```

###### app/javascript/packs/stylesheets/_custom.scss
```scss
h1 { 
  color: red;
}
```

###### app/javascript/packs/application.js
```javascript
// This file is automatically compiled by Webpack, along with any other files
// present in this directory. You're encouraged to place your actual application logic in
// a relevant structure within app/javascript and only use these pack files to reference
// that code so it'll be compiled.

require("@rails/ujs").start()
require("turbolinks").start()
require("@rails/activestorage").start()
require("channels")


// Uncomment to copy all static images under ../images to the output folder and reference
// them with the image_pack_tag helper in views (e.g <%= image_pack_tag 'rails.png' %>)
// or the `imagePath` JavaScript helper below.
//
// const images = require.context('../images', true)
// const imagePath = (name) => images(name, true)

import 'bootstrap'
import '../stylesheets/application'

document.addEventListener("turbolinks:load", () => {
  $('[data-toggle="tooltip"]').tooltip()
})
```

Then test it.

# Including Asset Pipeline Into Webpack

To do this, we’ll need to update config/webpacker.yml to be able to resolve assets stored in the app/assets folder.

###### config/webpacker.yml
look for this line
```javascript
resolved_paths: []
```
and add this
```javascript
resolved_paths: ['app/assets']
```

###### app/assets/stylesheets/home.scss
add this style in home
```scss
h1 { color: green; }
```

###### app/javascript/packs/stylesheets/application.scss
import home styles
```scss
@import "~bootstrap/scss/bootstrap"; 
@import "./_custom";
@import "stylesheets/home";
```

### sources:
- https://medium.com/@adrian_teh/ruby-on-rails-6-with-webpacker-and-bootstrap-step-by-step-guide-41b52ef4081f
- https://medium.com/@guilhermepejon/how-to-install-bootstrap-4-3-in-a-rails-6-app-using-webpack-9eae7a6e2832
