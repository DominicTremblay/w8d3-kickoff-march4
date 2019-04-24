# Rails Back-End with React Front-End Setup

## Create Rails App

- create a project folder and cd into the folder
- type the following command:

`rails new . --api --database=postgresql -T --no-rdoc --no-ri`

- configure database in database.yml

## Create React Client

At the root of the project folder, type the following:

`create-react-app client`

Make sure create-react-app is installed globally:

`npm i -g create-react-app`

## Ports

- React front-end is running on port 3000
- Rails back-end is running on port 3001 (or any other)

  `rails s -p 3001 -b 0.0.0.0`

## Cors

**Cross-Origin Resource Sharing**

- A web application makes a cross-origin HTTP request when it requests a resource that has a different domain (i.e. different ports)
- Web application using APIs can only request HTTP resources from the same origin the application was loaded from, unless the response from the other origin includes the right CORS headers.

## Proxy API Calls on The Client

Add a proxy to package.json:

```json
{
  "name": "client",
  "version": "0.1.0",
  "private": true,
  "proxy": "http://localhost:3001",
  ...
```

Start the React Client with Yarn start

## Enable Cors on The Back-End

Rails 5 with --api mode will prepare the setup for you. You just need to uncomment the following:

```ruby
# Gemfile
gem 'rack-cors'

# config/initializers/cors.rb
# client running on port 3000

Rails.application.config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins 'localhost:3000'
    resource '*',
      headers: :any,
      methods: %i(get post put patch delete options head)
  end
end
```

- Run `Bundle install`
- Run `rake db:reset` and `rake db:migrate`
- Start the back-end: `rails s -p 3001 -b 0.0.0.0`

** if you get this warning **
warning: already initialized constant FileUtils::VERSION

Disable `require 'bootsnap/setup'` in config/boot.rb

## Create The Tables

- generate models with rails g model

`rails g model user name`

- generate controllers with rails g controller

`rails g controller api/v1/users`

- Setup the relationships
- run migrations
- run seed

## Setup The Routes

### Create API Namespace

- In routes.rb:

```ruby
namespace :api do
    namespace :v1 do
      resources :users
    end
end
```

- Namespace the controllers

- Create a folder `/api/v1` under `app/controllers`
- Move the controller files into this folder
- Add namespacing to controllers:

```ruby
class Api::V1::UsersController < ApplicationController

  def index
    users = User.all
    render json: users
  end

end
```

## Create an AJAX Request

- Create a React Component in client/App.js
- Install Axios `npm i axios --save`
- In ComponentDidMount, create a request with Axios:

```javascript
componentDidMount() {
  axios
    .get("http://localhost:3001/api/v1/resources.json")
    .then(response => {
      console.log(response);
      this.setState({
        resources: response.data
      });
    })
    .catch(error => console.log(error));
}
```

## References

[Cross-Origin Resource Sharing](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)

[Rails 5 API and CORS](https://til.hashrocket.com/posts/4d7f12b213-rails-5-api-and-cors)

[Configuring the Proxy Manually](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#configuring-the-proxy-manually)

[ReactJS + Ruby on Rails API + Heroku App](https://medium.com/@bruno_boehm/reactjs-ruby-on-rails-api-heroku-app-2645c93f0814)
