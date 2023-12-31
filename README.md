# Start-up: Film Hub

## Elevator Pitch

The one-stop shop for film nerds. You can find information about movies, read reviews, and even contribute your own reviews and chat with other users. You can also keep track of your favorite films, films you've watched, and films you want to see. The site will be connected to other useful sites such as Wikipedia, IMDB, RottenTomatoes, and Blu-ray.com.

## Design

### Home Page Rough Sketch

![Home Page Rough Sketch](public/images/StartupSketchHomePage.png)

### Search Page Rough Sketch

![Search Page Rough Sketch](public/images/StartupSketchSearchPage.png)

### Chat Page Rough Sketch

![Chat Page Rough Sketch](public/images/StartupSketchChatPage.png)

### Profile Page Rough Sketch

![Profile Page Rough Sketch](public/images/StartupSketchProfilePage.png)

### Login Page Rough Sketch

![Login Page Rough Sketch](public/images/StartupSketchLoginPage.png)

### Signup Page Rough Sketch

![Signup Page Rough Sketch](public/images/StartupSketchSignupPage.png)

## Key Features

### Things anybody who visits the site can do
* Search any movie and go to its page
* See general information and reviews about it on this site
* See links to additional sites with more information about that film

### Things only logged in users can do
* Chat with other users
* Score a movie
* Write a text review for a movie
* Put movies on a watchlist and favorites list

## Technologies

* HTML - Site format, navigation at the top, page layout, entry forms
* CSS - Make everything look pretty
* JavaScript - Make everything functional (forms send to database or web APIs, response is returned and posted)
* Service - Will make use of both internal DB and external web services via API (IMDB, etc)
* DB - Store user information and movie reviews
* Login - Require users to login to contribute anything
* WebSocket - For chat area (users send messages to server, which gets forwarded to everyone who has the page open
* React - Porting website to react

## Extra features if I have time and can figure it out

* When logged in, login and signup button at the top go away and are replaced with a single "signout" button
* Require users to verify their email address
* Add more things on the home page
* Integrate with more websites

## Notes for Startup HTML assignment submission

* Created the basic HTML pages and populated them with the necessary forms, and some placeholder data where database accessing will occur
* Deployed startup to startup.filmhub.click and simon (with some small modifications) to simon.filmhub.click

## Startup CSS Deliverables

* Uploaded Simon CSS to simon.filmhub.click and checked out its code so I could incorporate ideas from that into my startup
* Revised the HTML structure of the website to better accomodate the structure and CSS I need
* Added a global CSS file for the entire site, that sets up the header and footer as constant and adaptive, with a center content that can scroll if it's taller than what's available
* Added CSS files for each individual page that render the main center content
* Got started on a tiny bit of JavaScript, what it does is highlight the navigation button corresponding to the page, like a tab, so it shows that instead of a page header title, which looks a lot nicer and is more intuitive

## Startup JavaScript Deliverables

* JavaScript support for future login - You can create an account on the signup page, login on the login page, and when you do, the chat and profile pages become available, and a logout button appears on the top to replace the login and signup buttons! This is all done with LocalStorage so it is not server-side.
* JavaScript support for future database data - If you search for films while logged in, two buttons appear to add the film to your favorites or watchlist! You can then go see those lists on your profile page, and remove the films from those lists if you want to! This is all done with LocalStorage so it is not server-side.
* JavaScript support for future WebSocket - If you are logged in, you can check out the chat page, which would normally show you logged in users and chat messages, but since we are using LocalStorage only for this part, you only see yourself logged in and only your own chat messages that you send! The chat messages are not saved once you reload the page. That will change once the backend works with websocket.
* JavaScript support for your application's interaction logic - The homepage is still curated, but now it loads the images and facts dynamically from an API request! The search function also works without logging in, and uses API requests to get data and populate a table of information. The other pages all use JavaScript to function as well.

## Startup Service Deliverables

* NodeJS and Express service has been created (see index.js)
* Frontend is served up using express (see index.js)
* Frontend calls third party endpoints (see public/scripts/search.js)
  - Third part API request to OMDB to get search results
* Backend provides service endpoints (see api_router.js and auth_router.js)
  | URL | HTTP Method | Requires Cookie? | Requires JSON Body? | Response Cookie? | Response JSON Body? | Description |
  | --- | ----------- | ---------------- | ------------------- | ---------------- | ------------------- | ----------- |
  | `/auth/signup` | POST | no | yes | no | no | Adds username and password to database |
  | `/auth/login` | POST | or | or | yes | no | If valid authtoken cookie, returns success, if valid username and password, returns new authtoken cookie |
  | `/auth/logout` | DELETE | yes | no | yes | no | If valid auth token in cookie, removes it from server and sends back empty cookie |
  | `/auth/list` | GET | no | no | no | no | Debug feature, prints memory database to NodeJS console, will be removed once database is online |
  | `/api/list/<list>` | GET | yes | no | no | yes | Returns JSON list (options are watchlist and favorites) |
  | `/api/exists/<list>` | POST | yes | yes | no | no | Checks if film in JSON request body exists in list (check HTTP response code) |
  | `/api/add/<list>` | POST | yes | yes | no | no | Adds film in JSON request body to list |
  | `/api/remove/<list>` | DELETE | yes | yes | no | no | Removes film in JSON request body from list |
  | `/api/user` | GET | yes | no | no | yes | Get user info from database |
* Frontend calls backend provided service endpoints (see all scripts in /public/scripts/ folder except chat.js and index.js)
  - If you go to my site, you can sign up, login, see your profile, go to the search page and add films from the search results into one of your lists, and see them appear in your profile. If you log out and log back in, you will see the data persist (in memory, as the database has not been set up yet).

## Startup DB Deliverables

All the data functionality was done with the service assignment, the difference between this and that is that now the data persists when you restart the server using `pm2 restart` as it pulls from MongoDB instead of using in-memory database

* MongoDB Atlas database created (done, URL is in canvas assignment)
* Provides backend endpoints for manipulating application data (see `api_router.js` and `auth_router.js`)
* Stores application data in MongoDB (see `data.js`)

## Startup Login Deliverables

I had already done most of the login during the service and DB assignments, the main thing that I added for this assignment was bcrypt for password hashing and uuid for authentication tokens.

* Supports new user registration (backend endpoint `/auth/signup` (see `auth_router.js`) as well as `signup.html` page)
* Supports existing user authentication (backend endpoint `/auth/login` (see `auth_router.js`) as well as `login.html` page)
* Stores and retrieves credentials in MongoDB (see `data.js`)
* Restricts application functionality based upon authentication (see `/public/scripts/main.js` which loads navigation tabs based on login status)

## Startup WebSocket Deliverables

The difference between this and the previous deliverable assignment is that now the chat page functions correctly. Logged in users can see the navigation tab button for it, and when they go to the page, they are connected to the chat interface. All active and logged in users are displayed in the box (users who are also connected to the chat feature) and the chatbox works as you would expect it to.

* Backend listens for WebSocket connection (see `index.js` after line 46)
* Frontend makes WebSocket connection (see `public/scripts/chat.js`)
* Data sent over WebSocket connection (`public/scripts/chat.js` line 63 and `index.js` lines 91, 98, 108, and 117)
* WebSocket data displayed in the application interface (You can see this on the site when you are logged in)
