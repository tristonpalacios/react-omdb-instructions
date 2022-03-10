# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) React OMDB Films [Part 2]

## Goal 1: Set up for favoriting

We want the user to be able to add/remove movies to a list of favorites by clicking on an icon in each film row.

### Add the icon and make it clickable

* Create a new component called `Fave` that renders the code below.
```html
<div className="film-row-fave add_to_queue">
  <p className="material-icons">add_to_queue</p>
</div>
```
* In the `FilmRow` component, underneath the `film-summary` `div`, render the `Fave` component.
* Define a `handleClick` function in `Fave` that takes the event (`e`) as an argument. For now, simply log out a message like `"handling Fave click!"` in the browser's console.
* Add an `onClick` to the `div` surrounding the fave icon (the one with the add_to_queue class) that triggers the `handleClick` method. Verify that you see the console.log in the browser console.

That's all! Your click is not yet adding favorites, but it is working. Later, you will modify your app so that when the Fave icon is clicked, your app adds or removes the selected movie from the user's favorites array.

---

## Goal 2: Set up film list to be filtered

### Add the ALL and FAVES sub-headings to the film list

```html
<div className="film-list">
    <h1 className="section-title">FILMS</h1>
    <div className="film-list-filters">
        <div className="film-list-filter">
            ALL
            <span className="section-count">{this.props.films.length}</span>
        </div>
        <div className="film-list-filter">
            FAVES
            <span className="section-count">0</span>
        </div>
    </div>

    {allFilms}
</div>
```

If you check your browser, these subheadings should appear in the left column. 

*(The `section-count` spans will show how many films are in each category. Right now, we're showing the overall lenth of the films given to us by the parent component and eventually we'll change the `FAVES` section count to be something similar to show how many faves we have. Until then, displaying `0` is a good filler.)*

### Make the subheadings clickable

* Add two `onClick` events inside `FilmList` so when "FAVES" and "ALL" are clicked, they both call the same `handleFilterClick` method.
* Write a `handleFilterClick` method that console.logs "a filter was clicked" to make sure the event is correctly firing. Check that this is so in the browser.
* `handleFilterClick` needs to know which subheading was clicked, so give it a `filter` parameter. Then change the console.log to print the `filter` argument.
*  Modify the `onClick` to pass through the appropriate argument (`onClick={()=>this.handFilterClick('all')}` and `onClick={()=>this.handFilterClick('faves')}`)

Try clicking FAVES - does it print to the console? Does ALL? 

Later, instead of viewing a message, clicking either option will display the correct list of movies to the user - but now you've assured the options are clickable, which is an important first step.


### Track the filter in state

By default, a user will be viewing the entire list of movies.

* In the `FilmListing` component, set `state` to an object with the key `filter` and the value `all`. This will set up the initial state of the component.

* Inside of the `handleFilterClick` method on the `FilmListing` component, use `this.setState` to set the value of `filter` to the value passed to the event handler.

Use the React Dev tools to verify the state is changing appropriately.

### Color the subheading when selected

To give the user a clue as to where they are, the ALL and FAVES `div`s should change color depending on which is active. In the CSS, we've already set the colors using a class; you'll need to dynamically change which class each `div` has.

You now want the `className` attribute on each `.film-list-filter` `div` to dynamically update when the state is changed. When `filter: all`, you want to give the `div` the class `is-active` to the `ALL` filter. When `filter: faves`, you want to give the `div` the class `is-active` to the `FAVES`.

<details>
  <summary>Hint</summary>
  Try using string interpolation to include a ternery statement in the <code>className</code>.
</details>

<details>
  <summary>Solution</summary>
  
  ```javascript
  return (
    <div className="film-list">
      <h1 className="section-title">FILMS</h1>
      <div className="film-list-filters">
          <div className={`film-list-filter ${this.state.filter === 'all' ? 'is-active' : ''}`} onClick={() => {
            this.handleFilterClick("all")
          }}>
            ALL
            <span className="section-count">{props.films.length}</span>
          </div>
          <div className={`film-list-filter ${this.state.filter === 'faves' ? 'is-active' : ''}`} onClick={()=>{
            this.handleFilterClick("faves")
          }}>
            FAVES
            <span className="section-count">0</span>
          </div>
      </div>
      
      {allFilms}
    </div>
  )
  ```
</details>


Check in your browser that everything works.

---

## Goal 3: Prepare to display details for any movie

You aren't yet going to be creating the large detailed view of the film that will be displayed on the right column, but you're going to start setting up for it. The end goal is for the user to be able to click any movie on the list, and have it show more details about the movie on the right.

## Make the FilmRows clickable

* Inside `FilmRow`, define a function called `handleDetailsClick`. The function should accept a `film` as an argument. Print out `Fetching details for ` and the film title to the console.
* Add an `onClick` to the `film-row` div so that your message gets printed whenever you click on a film row - don't forget to pass the argument. Check this in your console by clicking any film row.

## Stop the event propagation

Hold up! Notice that you are now seeing two messages every time you click on the Fave icon/button. This is tricky, but the reason is because the event is propagating upward to the `FilmRow`. To make it so only one message appears, you'll need to stop the event propagation.

To do this, inside the `Fave` component's `handleClick` function, add the line `e.stopPropagation()`.

Try clicking the Fave icon/button - there's only one message now.

---

## Goal 4: Make Fave component change when movie is favorited

#### Step 1: Set the initial state

By default, a film is not a user's favorite.

In the `Fave` component, set `state` to an object with the key `isFave` and the value `false`. This will set up the initial state of the component.

#### Step 2: Set the state in your event handler

When the user clicks the Fave icon/button to add or remove a film from their favorites list, the app should change the film's `isFave` state to reflect that.

Inside of the `handleClick` method on the `Fave` component, use `this.setState` to toggle the value of `isFave`. "Toggle" means you always want to set the new value to the opposite of the current value.

<details>
  <summary>Hint</summary>
    Here is one way to achieve this, but there are others!
  <code>        
      this.setState((prevState)=>{
            return {isFave: !prevState.isFave}
        })
  </code>
</details>

#### Step 3: Set the `className` on `div` based on the `IsFave` state

You now want the `className` attribute on the `div` to dynamically update when the state is changed. Currently, the `className` on the `div` is `add_to_queue`. However, if the film is already favorited, then the film is already in the queue. Therefore, when `isFave: true`, the `className` should instead be `remove_from_queue`.

What you need to make this happen:
- When `isFave: true`, you want the `div` to have the class`remove_from_queue`. When `isFave: false` you want to the `div` to have the class `add_to_queue`.
- You also want to change the text that's rendered in the `p` to be the same text as the class - `remove_from_queue` or `add_to_queue`.

Note: You can use the same ternery structure you did for the subheadings, OR 

<details>
  <summary>Hint</summary>
  <p>Refer to the ternery you used for the subheadings!</p>
</details>
<details>
  <summary> You can make things more readable if you determine which class to set first, store that value in a variable, then insert that variable into the `className` attribute.</summary>
    <p>Put the following code above the render method, then use string interpolation to add the value of <code>action</code> to the className attribute.</p>
  <code>
        const action = this.state.isFave?'remove_from_queue':'add_to_queue'
  </code>
</details>
