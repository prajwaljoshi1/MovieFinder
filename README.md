# MovieFinder Coding challenge

A single page application built using vuejs, which can list and filter movies provided by a third party API.
Bootstrapped with vue-cli-4 with no additional library used.

### Installing
The env is loaded through vue-cli-4 dotenv, and has been provided in this zip. the .env file contains the api key for OMDB api.

### Running the app
To run in dev mode ````npm run serve````
To build the app  ```` npm run build ````


### Browser support
The app has been tested on latest chrome and firefox, has not been tested on tested on safari, edge and ie. 
 Styling remains consistent across chrome and firefox.


### Built meeting all asked features and very close to given wireframe

1. Display list of movies, with each result displaying the movie’s, Title and Year of release 
2. Provide a search input to allow the user to filter movies by free text.
3. Results should be paginated 
4. Maximum of ten results per page
5. Display a total number of results 
6. Hide pagination when the total number of results is less than the page size 
7. On a desktop, when a user selects an item in the list, the screen should split to provide a detailed view of the movie. This view should include all content from the list view, plus the movie’s: Poster Long plot Language(s) Actor(s) Duration 


### Additional things I added

1. small, x-small devices support 
2. previous page button and next page button in pagination will be disabled when applicable.


### main screenshot
![MovieFinder screenshot](/screenshot.png)


