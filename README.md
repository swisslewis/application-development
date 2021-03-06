# Application Development
An introduction to full stack development using Vue.JS, Node.JS, Express, MongoDB, and Mongoose. Includes projects that lead to a fully featured clientside Vue application (directories with a ```c``` prefix) and for a Node.JS backend (directories with an ```s``` prefix).

## How to use
The directories in this project are each projects that follow a step by step guide to application devlopment. Open up each one and read its README.md file to find out more. You can use your text editor's search tool to find out the relevant parts of a project by search the project's code (e.g. ```c1:```):
```c1:``` - ```c7:``` Client-side projects
```s1:``` - ```s5:``` Server-side projects

## Quick start
If you just want to see what this project does, follow these steps:
### Backend
1. Set up your database following the instructions in the section "MongoDB Atlas setup" below.
2. In the ```s5-entity-relationships``` directory (the backend project), add a ```.env``` file with ```MONGODB_URI=<YOUR_CONNECTION_URL>``` (substituting the value with your Atlas connection string).
3. Open your terminal on the ```s5-entity-relationships``` directory (the backend project). In VS Code you can right click on the directory and choose "Open in Terminal".
6. Run ```npm install```
7. Run ```npm run dev```

### Frontend
1. Install Vue CLI (see the section below).
2. Open a new terminal on the ```c7-entity-relationships-client``` directory (the frontend project).
3. Run ```npm install```
4. Run ```npm run serve```

### Vue CLI installation
For a computer where have admin permissions (i.e. your own computer) just install the Vue CLI [as instructed](https://cli.vuejs.org/guide/installation.html). If you don't have admin permissions, and you're on a Mac OS, you can do the following.

#### Enabling NPM global installation on Mac OS with no admin privileges
In your terminal:
1. Make a directory for npm global installations:
````
mkdir ~/.npm-global
````
2. Configure npm to use the new directory path:
````
 npm config set prefix '~/.npm-global'
````
3. Navigate to the root directory:
````
cd ~
````
4. Check if there is a ```.profile``` file:
````
ls -A
````
4. If there is no ```.profile``` file, create one:
````
touch .profile
````
5. Open the file in an editor:
````
nano .profile
````
6. Add this line:
````
export PATH=~/.npm-global/bin:$PATH
````
7. Save and close the editor by using the key board: CONTROL + X, then SHIFT + Y, then ENTER
8. Back on the terminal, update your system variables:
````
source ~/.profile
````

## Client-side development (c1 - c6)
The development of the clientside project follows six main use cases. Here they are in the order that they will be implemented:
- View About page
- View articles list
- View article details
- Add article
- Update article
- Delete article

A layer of simple auth will be added (not for production apps) for a user to see articles, so there are additional use cases of:
- sign up
- login
- logout

### Use cases
#### 1. View About page (c1)
In order for the user to be able to navigate our website, and in this case view the About page, we need to implement routing. In Vue, this is accomplished by using an external plugin - ```vue-router```.

#### 2a. View articles list (c2a)
We are going to get our articles from an external API [newsapi.org](https://newsapi.org/). In order to do so we need a Javascript library or Vue plugin that can help us to make and handle HTTP requests. We'll use the Vue plugin ```vue-resource``` for this.

#### 2b. View articles list - extended (c2b)
A closer look at [newsapi.org](https://newsapi.org/) indicates that we can add more functionality to improve the user experience. The API also exposes a list of news sources. Let's add a new feature that will allow the user to filter the list of articles by new source.

#### 2c. View articles list - refactored (c2c)
The way we wrote the code to implement our new feature could be improved. It might be that we want to re-use the list in other areas of our application, but to do so would mean we'd have repeated code.

Let's create a Vue component out of our list, and we'll do the same with the news source form. The two components need to communicate with each other, so we'll use [component props](https://vuejs.org/v2/guide/components-props.html) and [custom events](https://vuejs.org/v2/guide/components-custom-events.html) for that.

#### 3. View article details (c3)
A common pattern for applications to follow is the "master-detail" pattern. This is when you have a collection of items (like articles) and you can select one item to get further details. It would be great to build this feature into our application, but we've discovered a limitation of the [newsapi.org](https://newsapi.org/) API. They only expose the URL for a detail page, not the full article data itself. Luckily, we have built a custom API on [Glitch](https://glitch.com/) that will allow us to do this, so let's switch to using that API instead.

The article base URL is ```https://api-entity-relationships.glitch.me``` and the endpoints are:

- Get all articles: ```GET /api/articles```
- Get an article by ID: ```GET /api/articles/:articleId```

No authentication is necessary to access this API (so you won't need an API key).

#### 4. Add article (c4)
Now we're using a custom API, we can really ramp up the features. Let's add full CRUD functionality (create, read, update, delete). Starting with "Create", we'll allow the user to add an article. 

We should think of what properties an article might have. These will correspond to our front-end app's form fields, and will also match the entity/model we create later in our server-side app. So, an article has:

- title
- description
- body
- a time it was created
- a time it was updated

We'll just handle the title and body on the frontend.

#### 5. Update article (c5)
In order to implement this use case, we can reuse the ```ArticleEditor``` component and just fill it with values from our database. The difference between this and the ```Add article``` use case is that we need to retrieve an Article by it's ID and make it available to the ```ArticleEditor```.

#### 6. Delete article (c6)
The last use case to implement in order for our application to have one full set of CRUD functionality is to allow the user to delete articles. Luckily, this is pretty simple!

### Entity relationships (c7)
This project should be the final one, after ```s5-entity-relationships``` has been completed, because it needs this server-side functionality. 

We have created the User entity on the server, so let's use it. We'll create the ability for a user to sign up and login. It will just be a simple auth system, and therefore it won't be very secure and shouldn't be used for production apps/anything serious. We'll need a way for the Login and Register components to communicate with the Header component so the 
Login/Logout link can be toggled. We could use the method we used in ```c2c-view-articles-list-refactored``` but this time, let's use a more flexible (and simpler) way - Event Bus.

## Server-side development (s1 -s5)
The server-side application needs to be able to handle:
- routes
- Object Relational Mapping (ORM) - which means taking an entity object, like ```Article``` and storing it in the database, and vice-versa
- CRUD (Create, Read, Update, Delete) operations for our ```Article``` and ```User``` entities

As our application is going to have multiple entities, we also need to think about their relationships and how they're going to work with each other.

### Installation of server side projects

1. Open the terminal up on this directory and run:
````
npm install
````
2. Run the development server:
````
npm run dev
````

### Using the API
#### Simple first test
After running the server, for a quick test in the browser you can go to the following endpoints:

#### Get an article
[GET /v1/articles](http://localhost:3000/v1/articles)

#### Get an article by ID
[GET /v1/articles/:articleId](http://localhost:3000/v1/articles/abc123)

#### Testing the API using a collaorative platform
To test POST, PUT and DELETE requests, we're going to use [Postman](https://www.getpostman.com/).

In Postman, import the ```s1-create-routes-setup.postman_collection.json``` file from this directory which will create a new collection for you to test out all of the API endpoints.

### Project functionality
#### 1. Set up routes (s1)
To create our API, we need to set up routes that can handle incoming requests. If we want an API that can help us implement CRUD functionality for our application, we'll need to set up
routes to handle GET, POST, PUT, and DELETE HTTP request methods.

#### 2. Set up database (s2)
For convenience, we're going to use a cloud-based database provider, MongoDB Atlas, for our database. You could choose to install MongoDB onto your own machine, but this is not covered here.

##### Environment variables
In this directory create an ```.env``` file, add your Atlas (or other) connection URL:

````
MONGODB_URI=<YOUR_CONNECTION_URL>
````
##### MongoDB Atlas setup
1. Sign up at [https://www.mongodb.com/cloud/atlas](https://www.mongodb.com/cloud/atlas)
2. Create a free cluster (you can just keep the defaults and continue without upgrading)
3. Create a database user
- Go to Security > Database Access, then "Add New User". 
- Enter a Username and password (that you don't use for anything else - you could just autogenerate it) and make a note of these so you don't forget them. 
IMPORTANT: *DON'T* put the password directly into your code. Use a .env file and make sure you have a [.gitignore](https://help.github.com/en/github/using-git/ignoring-files) file in the root of your project that contains a line ```.env```
- Choose "Read and write to any database", then click "Add User".
4. Whitelist your IP address
- In the main menu, go to "Network Access". Choose "Allow access from anywhere" and click "Confirm".
5. Load sample data
- Skip this step.
6. Connect to your cluster
- Go to Atlas > Clusters and click "Connect". Choose "Connect your application" and copy the connection string. 
- Paste it into your .env file as the value for ```MONGODB_URI``` (remember to make sure you've got ```.env``` listed in your ```.gitignore``` file!).
- Change the ```<password>``` in the connection string to the password that you specified when creating the database user.
7. Now run ```npm run dev``` on the directory and you should see ```✔ MongoDB connected```. Your application now has a database!

#### 3. Set up models (s3)
For a start, we're going to just have one database model - "Article". The [Mongoose schema](https://mongoosejs.com/docs/guide.html) for this model will look like this:

|    Property    |   Type   |
|       ---      |    ---   |
|  title         |  String  |
|  description   |  String  |
|  body          |  String  |

### 4. CRUD functionality (s4)
"CRUD" stands for Create, Read, Update, and Delete, and these are the operations that we'll need to implement in order to implement our use cases below. We can achieve this by going back to our ```routes/v1/articles.js``` file and refactoring it to use the database and Mongoose model that we've set up.

#### Use cases
These are the use cases that we can implement on the server-side in ```routes/v1/articles.js```.
1. View all articles
2. View a single article
3. Add an article
4. Update an article
5. Delete an article

Once you have implemented each of the use cases, test the API in the collection that you had previously set up in Postman and watch the data change in MongoDB Atlas (go to Clusters > Collections). Magic!

### 5. Relationships between entities (s5)
Right now our application has one entity (represented by our database model): Articles. Most applications have more than one entity, and the entities in an application often have relationships with each other. For example, a "Comment" entity might belong to a "Post" entity. This would be a "one-to-many" relationship, where one post can have many comments (and a comment can only belong to one particular post). 

We are going to create another entity: Users. In doing so, we need to think about the relationship between users and articles. Much the same as our example with posts and comments, we can represent users and articles with a one-to-many relationship, i.e. one user can have many articles.

The schema of the User entity will look like this:

|    Property    |       Type       |
|       ---      |        ---       |
|  firstName     |  String          |
|  lastName      |  String          |
|  displayName   |  String          |
|  email         |  String          |
|  articles      |  Array<Article>  |

And the new schema for our Article entity will look like this:

|    Property    |   Type   |
|       ---      |    ---   |
|  title         |  String  |
|  description   |  String  |
|  body          |  String  |
|  author        |  User    |

After completing code necessary to create this relationship, import the ```s5-entity-relationships.postman_collection.json``` file into Postman and test out your new API. 

We now have two projects that comprise a feature-packed full-stack application!

## Next steps
- This application fits a model where a user can see all articles once logged in. Change the application so that it fits a model where users only have access to their own posts.
- Improve the application so that it uses a more robust industry standard auth system. One popular set of Express middleware for this is [passport.js](http://www.passportjs.org/) and you can see it in use in [node-express-realworld-example-app](https://github.com/gothinkster/node-express-realworld-example-app).
