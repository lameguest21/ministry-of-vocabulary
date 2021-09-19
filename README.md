# VOCAB.JS.ORG 
codename Ministry Of Vocabulary

A MERN Stack app which helps you learn new words, improve your vocabulary and explore mnemonics. 

## Use Case
I developed this project during my time with GRE/TOEFL preparation. I came to an epiphany that we humans remember something better if we attach a strong physical memory to anything in our lives.

I applied the same principle and thought it would be helpful for students who find it hard to remember tough English words.

## Features
- Database consisting of more than 40,000 words with meaning, mnemonics, usage and more
- Simple CRUD operations
- Node.js/Express.
- React.js powered by Semantic UI.
- Redux for State Management.
- User Authentication with JWT.
- Improved and Secure API calls. 
- Deboucing API calls during typing, thus unnecessary backend calls are significatly reduced.

### Demo
![](./Demo.gif)

## Future Implementations
1. Adding more words, meaning and mneomonics.
2. Improve UI
3. Improve Auth, API and Security.


## Running locally

```
git clone git@github.com:lameguest21/ministry-of-vocabulary.git
cd ministry-of-vocabulary
npm i

cd client
npm i

cd ..
npm start
```

## Overview

`create-react-app` configures a Webpack development server to run on `localhost:3000`. This development server will bundle all static assets located under `client/src/`. All requests to `localhost:3000` will serve `client/index.html` which will include Webpack's `bundle.js`.

To prevent any issues with CORS, the user's browser will communicate exclusively with the Webpack development server.

Inside `Client.js`, we use Fetch to make a request to the API:

```js
// Inside Client.js
return fetch(`/api/word?q=${query}`, {
  // ...
})
```

This request is made to `localhost:3000`, the Webpack dev server. Webpack will infer that this request is actually intended for our API server. We specify in `package.json` that we would like Webpack to proxy API requests to `localhost:3001`:

```js
// Inside client/package.json
"proxy": "http://localhost:3001/",
```

This handy features is provided for us by `create-react-app`.

Therefore, the user's browser makes a request to Webpack at `localhost:3000` which then proxies the request to our API server at `localhost:3001`:

![](./flow-diagram.png)

This setup provides two advantages:

1. If the user's browser tried to request `localhost:3001` directly, we'd run into issues with CORS.
2. The API URL in development matches that in production. You don't have to do something like this:

```js
// Example API base URL determination in Client.js
const apiBaseUrl = process.env.NODE_ENV === 'development' ? 'localhost:3001' : '/'
```

This setup uses [concurrently](https://github.com/kimmobrunfeldt/concurrently) for process management. Executing `npm start` instructs `concurrently` to boot both the Webpack dev server and the API server. The `react-scripts` are started in /client folder with the above command and `nodemon` starts the API server in dev mode thus enabling hot reloading with the exception of /client folder under watchlist which is ignored by nodemon.

## Deploying

### Background

The app is ready to be deployed to Heroku.

In production, Heroku will use `Procfile` which boots just the server:

```
web: npm run server
```

Inside `server.js`, we tell Node/Express we'd like it to serve static assets in production:

```
if (process.env.NODE_ENV === 'production') {
  app.use(express.static('client/build'));
}
```

You just need to have Webpack produce a static bundle of the React app (below).

### Steps

We assume basic knowledge of Heroku.

**0. Setup your Heroku account and Heroku CLI**

For installing the CLI tool, see [this article](https://devcenter.heroku.com/articles/heroku-command-line).

**1. Build the React app**

Running `npm run build` creates the static bundle which we can then use any HTTP server to serve:

```
cd client/
npm run build
```

**2. Commit the `client/build` folder to source control**

From the root of the project:

```
git add client/build
git commit -m 'Adding `build` to source control'
```

**3. Create the Heroku app**

```
heroku apps:create <app-name>
```

**4. Push to Heroku**

```
git push heroku master
```

Heroku will give you a link at which to view your live app.

## Credits

Thanks to Anthony Accomazzo . This project uses code from [food-lookup-demo](https://github.com/fullstackreact/food-lookup-demo.git).
