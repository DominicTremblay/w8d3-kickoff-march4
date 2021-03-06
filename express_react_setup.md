# Express Back-End with React Front-End

## Install generator

`npm install -g express-generator`

## Install The Back-End

- express --view=ejs final-project-name
- cd final-project-name && npm install
- edit bin/www and change the port to 3001
- add console.log to onListening and add a start script
- create .gitignore
- git init
- install nodemon:

`npm install nodemon --save-dev`

- add local script to start nodemon with `npm run local`

## configure Knex

- npm install knex -g
- npm install knex --save
- knex init
- configure development to use postgresql
- add your credentials in .env files and configure knexfile.js
- make sure migrations and seeds are added to './db' folder in knexfile.js
- add dotenv (\* add to gitignore) npm install dotenv --save
- require knex and dotenv in app.js

  `const knex = require("knex")(knexConfig["development"]);`
  `require("dotenv").config();`

- # install pg - npm install pg --save
- install pg
- create migrations (knex migrate:make migration_name)
- create seeds

In knex.init

```
client: 'postgresql',
    connection: {
      host     : process.env.DB_HOST,
      user     : process.env.DB_USER,
      password : process.env.DB_PASS,
      database : process.env.DB_NAME,
      port     : process.env.DB_PORT,
      ssl      : process.env.DB_SSL
    },
```

## Seeds

- Create seed files for populating the database
- Create seed file with `knex seed:make fileName`
- Seeds are executed in alphabetical order, so you might want to precede filenames with a letter (ex.: a_addUsers.js, b_addPolls.js)
- To restart auto increments at 1 each time the seed file runs, use Promise.all and ALTER SEQUENCE query:

```js
exports.seed = function(knex, Promise) {
  return Promise.all([
    knex('users').del(),
    knex.raw('ALTER SEQUENCE users_id_seq RESTART WITH 1'),
    knex('users').then(function() {
      return Promise.all([
        knex('users').insert({ name: 'Alice' }),
        knex('users').insert({ name: 'Bob' }),
        knex('users').insert({ name: 'Charlie' }),
      ]);
    }),
  ]);
};
```

- `npm install --save-dev faker` and require faker if fake data is needed.

## Test a Route

- Add some JSON objects in the user route for testing. Run the server and test the route.

```
res.json([

    {
    	id: 1,
    	username: "greenarrow"
    },
    {
    	id: 2,
    	username: "ironfist"
    }

])
```

- change the route to '/v1/users'

- add v1 folder in routes folder
- app.use('/v1/users', usersRouter)

## Install the Front-End

At the root of the project folder, type the following:

`create-react-app client`

Make sure create-react-app is installed globally:

`npm i -g create-react-app`

(make sure you have node 8.12.0 or above)

## Add a Request in ComponentDidMount of App

```
import React, { Component } from 'react';
import './App.css';

class App extends Component {
state = {users: []}

  componentDidMount() {
    fetch('/api/v1/users')
      .then(res => res.json())
      .then(users => this.setState({ users }));
    }

    render() {
      return (
        <div className="App">
        <h1>Users</h1>
          {this.state.users.map(user =>
            <div key={user.id}>{user.username}</div>
          )}
        </div>
      );
      }
    }

export default App;
```

## Ports

- React front-end is running on port 3000
- Rails back-end is running on port 3001 (or any other)

## Cors

**Cross-Origin Resource Sharing**

- A web application makes a cross-origin HTTP request when it requests a resource that has a different domain (i.e. different ports)
- Web application using APIs can only request HTTP resources from the same origin the application was loaded from, unless the response from the other origin includes the right CORS headers.

## Proxy API Calls on The Client

Add a proxy to package.json:

```
{
  "name": "client",
  "version": "0.1.0",
  "private": true,
  "proxy": "http://localhost:3001",
...
```

## References

[Create React App with an Express Backend](https://daveceddia.com/create-react-app-express-backend/)

[Access-Control-Allow-Origin: Dealing with CORS Errors in React and Express](https://daveceddia.com/access-control-allow-origin-cors-errors-in-react-express/)

[Deploy React and Express to Heroku](https://daveceddia.com/deploy-react-express-app-heroku/)
