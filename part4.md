# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) You Do: Film Exercise

## Your Mission

Stop any project you currently have running; let's go back to the film application that you've started. You can run the app with `npm start`.

You're almost finished! Now, You need to:
- Replace info from `TMDB.js` with results from an api call to TMDB.
- Refactor your React app to make it as clean as possible.

![](images/bladerunner.png)

### Task 2: Adding the API call

API calls in React are handled using whatever AJAX library you want; [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)(provided by modern browsers) or [Axios](https://github.com/axios/axios) are some of the most common. 

> Note: For official documentation on how to handle AJAX calls in react, [visit the docs](https://reactjs.org/docs/faq-ajax.html)

You already have a function where you want the API call to go (`handleDetailsClick` - when a user clicks for details of a movie, you'll call the API to get those details), so your api call will work inside that function. You've set up the rest of your app to make this transition nice and smooth.

We'll be using `Axios` for this section. TMDB also needs an API key, so we'll need to have a  `.env` file. To make both of these work, we'll have to install them.
In your terminal, run `npm i dotenv axios` _(if using yarn, run `yarn add dotenv axios`)_. This will add these two packages as dependencies.



#### Step 1: Set up the API key

This step seems complicated, but it isn't! Just take it one step at a time. Because TMDB isn't a public API, you'll need to get an API key to add to your axios call; then, you'll want to make sure to keep the key in a safe spot.

- To gain access to the TMDB API, you'll need to get an API key from [TMDB](https://www.themoviedb.org).
  - TMDB only gives API keys to users with accounts, so you'll have to sign up first (it's free). However, it will ask for your phone and address.
  - Then, request an API key on your profile page ([further instructions](https://developers.themoviedb.org/3/getting-started)).
  - Once you have your API key, you need to include it in your app. Since you **never want to store app secrets in your repository**, you'll use the [`dotenv`](https://github.com/motdotla/dotenv) package to keep the API key in a local file.

  - Create a new file at the root of your project called `.env.local` (accept the system warning).
  - In your `.env.local` file, add the line `REACT_APP_TMDB_API_KEY=<Your TMDB API v3 KEY>`

*Note: The `.env.local` file is in your `.gitignore` by default when you create an app with `create-react-app`, so now your secret will never leak into your repository. It's important to note that since this is a front-end application, the built JavaScript will contain the key, which means end-users will be able to see it. However, that's fine for this practice app, since you'll only be running it locally. Putting it in a `.env` file is to protect it when pushing up to Github*

- Now you have an API key saved in `dotenv`. Now, point your application to it: add the following to the top of your `TMDB.js` file:

```js
import dotenv from 'dotenv';

dotenv.config();
```

- And replace `'<REPLACE_THIS_WITH_TMDB_API_KEY>'` with `process.env.REACT_APP_TMDB_API_KEY`.

Your secrets are now set up!

#### Step 2: Make a `const` called `url` with the API's URL

Now that you have the API key to call for movie details, let's go back to making that call.

In your `App.js` `handleDetailsClick` method, add the following `const` right above your `setState`:

```JavaScript
const url = `https://api.themoviedb.org/3/movie/${film.id}?api_key=${TMDB.api_key}&append_to_response=videos,images&language=en`
```

This is the URL to which you'll send your request to get detailed information about each film. You're passing the `film.id` and the `TMDB.api_key` as query string parameters.


#### Step 3: Make the API call


Now that you have the API key and URL set up, underneath the new URL variable, fetch the API.

```JavaScript
const url = `https://api.themoviedb.org/3/movie/${film.id}?api_key=${TMDB.api_key}&append_to_response=videos,images&language=en`

fetch(url)
.then(response=>response.json())
.then(json=>{console.log(json)}) // take a look at what you get back!
})
```

Try clicking a movie row in your browser - the data for it should appear in the console.

#### Step 4: Set the state when the API call completes

Let's now set your `current` state to be the object you get back from TMDB. Move the `setState` call into the API call.

```JavaScript
fetch(url)
.then(response=>response.json())
.then(json => { 
	this.setState({current: json}) // take a look at what you get back!
}) 
```

Now, you have the API call to get information about your chosen movie.

### Task 3: Refactoring our app

Before you continue to display the movie details to the user, let's clean up your application.

Let's refactor any components that only have a `render()` method into functional components. Functional components are simpler and will gain performance benefits in future versions of React. It is considered good practice to use them wherever possible.

#### Step 1: Refactor `FilmPoster.js`

1. Replace the `class`/`extends` definition with a `function`. Remember that your function should accept a `props` argument.
2. Remove the `render()` method, keeping only the `return` function.
3. Replace all instances of `this.props` with simply `props`
4. Remove `{Component}` from the React `import` at the top since you no longer use it (but still import `React`).

Check in your browser to be sure the functionality hasn't changed.

#### Step 2: Refactor `FilmRow`

Follow the same steps to refactor the FilmRow component.

Check in the browser to be sure the functionality hasn't changed.

#### Step 3: Refactor `FilmDetails`

You haven't written out the `FilmDetails` component yet, but it currently only renders UI. Therefore, you can also make it a functional component.

Follow the same steps as above, and once again check in the browser for functionality.



## Taking it further

Here are some optional things you can do to deepen your knowledge and take this app further:

- Refactor `Fave` into a functional component.
- Move the filters into a `FilmListingFilter` component.
- Implement client-side routing to show multiple pages of films.
- Go through the CSS and see how the app is styled (it uses both flexbox and CSS Grid).
- Add a textarea for a review to each film detail and save that on the film object.
- Show an icon in `FilmListing` for all films with reviews.
- Show the `fave` state of a film on `FilmDetails`.
- Add any other features you can think of!
