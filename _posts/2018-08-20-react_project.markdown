---
layout: post
title:      "React Project "
date:       2018-08-20 15:41:40 +0000
permalink:  react_project
---


Finally, my React project. I have to confess, React was very confusing to me when I started this project. Of course, even the simplest things weren't working as expected. I am passing props and I don't get the props I wanted, after hours of debugging my props are passing but I can't call my child method on it. After some more hours of debugging I can't get the parent to change its state and so on ... So altogether it takes me days to get one simple feature working. I have spent three weeks or so on this project because this is what it takes to learn React on your own. 

My project is a tracker of movies you would like to see and movies that you have already seen. On the home page you get a suggestion of popular movies. You can also look for a specific title or get movies suggestions  by year and/or genre sort sort the results by release_date or popularity. You can check them as Watched or To Watch. This automatically build your Wached  and To Watch lists on the application API.

![](https://i.imgur.com/IqEktLv.png)
The movie objects are fetch from The Movie Database, an amazing source of information about any movie. 
I had many issues to sort with differents criteria from different methods but what I would like to share with you is how I got the fetch to retreive 60 movies instead of 20 as the default provided by the movie database. I have no doubt there is other ways to do this, including having a page for 20 movies and a next button for the next 20 movies and so on. But my goal was to get a lot of movies at first sight. So here I am nesting my fetch to concat their results. 
```
export const fetchMoviesByPreferences = (obj) => {
  return (dispatch) => { 
    const moviesDBKey = process.env.REACT_APP_MOVIEDB_KEY
    dispatch({ type: 'LOADING_MOVIES' });
    return fetch(`https://api.themoviedb.org/3/discover/movie?api_key=${moviesDBKey}&primary_release_year=${obj.searchYear||""}&with_genres=${obj.genreId||""}&sort_by=${obj.sorting}&language=en-US&include_video=false&include_adult=false&page=1`)
      .then(response => response.json())
      .then(responseData => {
        const movies1 = responseData.results

      fetch(`https://api.themoviedb.org/3/discover/movie?api_key=${moviesDBKey}&primary_release_year=${obj.searchYear||""}&with_genres=${obj.genreId||""}&sort_by=${obj.sorting}&language=en-US&include_video=false&include_adult=false&page=2`)
      .then(response => response.json())
      .then(responseData => {
        const movies2 = responseData.results
        const movies12 = movies1.concat(movies2)

          fetch(`https://api.themoviedb.org/3/discover/movie?api_key=${moviesDBKey}&primary_release_year=${obj.searchYear||""}&with_genres=${obj.genreId||""}&sort_by=${obj.sorting}&language=en-US&include_video=false&include_adult=false&page=3`)
          .then(response => response.json())
          .then(responseData => {
            const movies3 = responseData.results
            const movies123 = movies12.concat(movies3)

          dispatch({ type: 'FETCH_MOVIES', payload: movies123})
          });
      });
    });
  };         
}
```

Flow: 

From my App.js I have three routes
- one for films, which is the root, giving you the first suggestions, the filter, and the display of the filtering,
- one for the Watched List,
- one for the To Watch List

```
         <Router>
          <React.Fragment>
              <NavBar />

              <Route 
              exact path="/" 
              render={(props) => <FilmsPage {...props} moviesFiltered={this.props.moviesFiltered} />}
              />

              <Route exact path="/watched" 
              render={(props) => <WatchedPage {...props} moviesWatched={this.props.moviesWatched} />}
              />

              <Route exact path="/towatch" 
              render={(props) => <ToWatchPage {...props} moviesToWatch={this.props.moviesToWatch} />}
              />

            </React.Fragment>
          </Router>
```

For each of those routes I have a Page component, without surprise:
- FilmsPage.js
- WatchedPage.js
- ToWatchPage.js
They are all receiving the appropriate props from the parent component App.js., as shown previously.

Children components:

- The App only has the navigation bar component, which display throughout the whole application and the three Page components.
- The FilmsPage component is the one calling on the most children components: a filter component itself having a drop down component for the genre selection, and a Movie component displaying each individual movie. 
- The WatchedPage displays a MovieWatched child component which displays each individual watched movie. 
- The ToWatchPage displays a MovieToWatch child component which displays each individual movie to watch. 

I started to store my ToWatch List and my Watched List in a state that I retrieved in my ToWatchPage and WatchedPage respectively.
Then I thought, persistence of the data. To allow the user to get his lists saved I needed to build a back end with a database which would keep track of those movies. So, I created the rails back end in another folder, created two tables, one for the watched movies and one for the movies to watched. I decided to only keep track of their poster_path, title and release_date. I connected the back end and the front end through the 'rack-cors' gem, modified the application.rb to finalize the connection. 
Then I built my routes: 
```
    get '/towatchmovies', to: 'to_watch_movies#index'
    post '/towatchmovies', to: 'to_watch_movies#create'
    delete '/towatchmovies/:id', to: 'to_watch_movies#destroy'

    get '/watchedmovies', to: 'watched_movies#index'
    post '/watchedmovies', to: 'watched_movies#create'
    delete '/watchedmovies/:id', to: 'watched_movies#destroy'
```
I defined my three actions in each controller: index, create and destroy. 

Then I went back on the front end to save the watched movies and to watch movies in those API end points. 
The functions handling the actions of the buttons stayed the same, but the actions had to change. 
For example the buttons "watched" used to list the movie in the list are triggering several functions. 
first it sends the film info to the actions  : 

```
handleWatchedClick = e => {
e.preventDefault();
this.props.actions.createFilmWatchedList(this.props.film)
}
```
In the movieActions it will handle the asynchronous dispatch of actions to take: 
```
export const createFilmWatchedList = film => {
 
  return (dispatch) => {
      MoviesApi.createFilmWatchedListInApi(film).then((responseMovie) => {
          dispatch(addFilmToWatchedList(responseMovie));
          return responseMovie;
        }).catch((error) => {
          throw(error);
        });
  };
}

export const addFilmToWatchedList = film => {
  return {
  type: 'ADD_FILM_TO_WATCHED_LIST',
  film
  }
}
```
In the MoviesAPI it will post to the API the movie:
```
  static createFilmWatchedListInApi(film) {
   const request = new Request(`http://localhost:3000/watchedmovies`, {
      method: 'POST',
      headers: new Headers({
        'Content-Type': 'application/json'
      }),
      body: JSON.stringify(film)
    });

    return fetch(request).then(response => {
      return response.json();
    }).catch((error) => {
      return error;
    });
  }
```
then go back to the movieActions to execute the addFilmToWatchedList(film) and finally go in the reducer to find the case:
```
	    case 'ADD_FILM_TO_WATCHED_LIST':
	    	return { ...state, moviesWatched: state.moviesWatched.concat(action.film) }
```
This last step allow the state to be updated right away and to not have to reload the page to get the last movies added to the list. The display of those lists are done by calling on the movies saved in the state, which I handled like this in the app.js: 
```
  componentDidMount() {
    this.props.actions.fetchMoviesWatched()
    this.props.actions.fetchMoviesToWatch()
  }
```
and with this in the movieActions:
```
export const fetchMoviesWatched = () => {
  return (dispatch) => { 
    dispatch({ type: 'LOADING_MOVIES' });
    return fetch("http://localhost:3000/watchedmovies")
    .then(response => response.json())
    .then(responseData => {const movies =responseData
    dispatch({ type: 'LOAD_WATCHED_MOVIES', payload: movies})
    });
  };
}
```
and here goes the reducer cases:
```
case 'LOADING_MOVIES':
	      return Object.assign({}, state, {loading: true})
case 'LOAD_WATCHED_MOVIES':
	    	return Object.assign({}, state, {loading: false, moviesWatched: action.payload} )
```

Of course this is just an example of the flow behind one of the button, but I have pretty much the same logic behind the "to watch" button to save a movie in the "To Watch" list and the delete buttons used in both lists to delete a movie. 

I hope this post can give enough clues on how to build a simple react/redux/rails application for anybody interested.
