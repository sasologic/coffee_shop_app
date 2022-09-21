# COFFEE SHOP FULL STACK APPLICATION

## Introduction
The application is aimed at solving Udacity's need to open a new digitally enabled cafe for students to order drinks, socialize, and study hard. The app sets up their menu experience.

My skills was demonstrated to create a full stack drink menu application. The application does the following: -

1. Display graphics representing the ratios of ingredients in each drink.
2. Allow public users to view drink names and graphics.
3. Allow the shop baristas to see the recipe information.
4. Allow the shop managers to create new drinks and edit existing drinks.


## Stacks Used

The application was built using Python, Flask, SQLalchemy, and SQlite local database for the Backend and Ionic for the Frontend. 

# Coffee Shop Backend

### Installing Dependencies

#### Python 3.7

Follow instructions to install the latest version of python in the [python docs](https://docs.python.org/3/using/unix.html#getting-and-installing-the-latest-version-of-python)

#### Virtual Environment

Instructions for setting up a virtual environment for your platform can be found in the [python docs](https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/)

#### PIP Dependencies

Once the virtual environment is setup and running, install necessary dependencies by naviging to the `/backend` directory and running:

```bash
pip install -r requirements.txt
```

This will install all of the required packages that are specified within the `requirements.txt` file.

##### Key Dependencies

- [Flask](http://flask.pocoo.org/) is a lightweight backend microservices framework. Flask is required to handle requests and responses.

- [SQLAlchemy](https://www.sqlalchemy.org/) and [Flask-SQLAlchemy](https://flask-sqlalchemy.palletsprojects.com/en/2.x/) are libraries to handle the lightweight sqlite database. 

- [jose](https://python-jose.readthedocs.io/en/latest/) JavaScript Object Signing and Encryption for JWTs. Useful for encoding, decoding, and verifying JWTS.

## Database Setup
The database is already set up.

## Running the server

From within the `./src` directory first ensure you are working using your created virtual environment.

Each time you open a new terminal session, run:

```bash
export FLASK_APP=api.py;
```

To run the server, execute:

```bash
flask run --reload
```

The `--reload` flag will detect file changes and restart the server automatically.

### Setup Auth0

1. Create a new Auth0 Account
2. Select a unique tenant domain
3. Create a new, single page web application
4. Create a new API
   - in API Settings:
     - Enable RBAC
     - Enable Add Permissions in the Access Token
5. Create new API permissions:
   - `get:drinks`
   - `get:drinks-detail`
   - `post:drinks`
   - `patch:drinks`
   - `delete:drinks`
6. Create new roles for:
   - Barista
     - can `get:drinks-detail`
     - can `get:drinks`
   - Manager
     - can perform all actions
7. Test your endpoints with [Postman](https://getpostman.com).
   - Register 2 users - assign the Barista role to one and Manager role to the other using your Auth0.
   - Sign into each account and make note of the JWT.
   - Import the postman collection `./starter_code/backend/udacity-fsnd-udaspicelatte.postman_collection.json`
   - Right-clicking the collection folder for barista and manager, navigate to the authorization tab, and including the JWT in the token field (you should have noted these JWTs).
   - Run the collection and correct any errors.
   

# Coffee Shop Frontend
The `./frontend` directory contains a complete Ionic frontend to consume the data from the Flask server. 

### Installing Dependencies

#### Installing Node and NPM

This project depends on Nodejs and Node Package Manager (NPM). Before continuing, you must download and install Node (the download includes NPM) from [https://nodejs.com/en/download](https://nodejs.org/en/download/).

#### Installing Ionic Cli

The Ionic Command Line Interface is required to serve and build the frontend. Instructions for installing the CLI is in the [Ionic Framework Docs](https://ionicframework.com/docs/installation/cli).

#### Installing project dependencies

This project uses NPM to manage software dependencies. NPM Relies on the package.json file located in the `frontend` directory of this repository. After cloning, open your terminal and run:

```bash
npm install
```

> _tip_: **npm i** is shorthand for **npm install**


### Configure Environment Variables

Ionic uses a configuration file to manage environment variables. These variables ship with the transpiled software and should not include secrets.

- Open `./src/environments/environments.ts` and ensure each variable reflects the system you stood up for the backend.

## Running Your Frontend in Dev Mode

Ionic ships with a useful development server which detects changes and transpiles as you work. The application is then accessible through the browser on a localhost port. To run the development server, cd into the `frontend` directory and run:

```bash
ionic serve
```

> _tip_: Do not use **ionic serve** in production. Instead, build Ionic into a build artifact for your desired platforms.
> [Checkout the Ionic docs to learn more](https://ionicframework.com/docs/cli/commands/build)


### Authentication

The authentication system used for this project is Auth0. `./src/app/services/auth.service.ts` contains the logic to direct a user to the Auth0 login page, managing the JWT token upon successful callback, and handle setting and retrieving the token from the local store. This token is then consumed by our DrinkService (`./src/app/services/drinks.service.ts`) and passed as an Authorization header when making requests to our backend.

### Authorization

The Auth0 JWT includes claims for permissions based on the user's role within the Auth0 system. This project makes use of these claims using the `auth.can(permission)` method which checks if particular permissions exist within the JWT permissions claim of the currently logged in user. This method is defined in  `./src/app/services/auth.service.ts` and is then used to enable and disable buttons in `./src/app/pages/drink-menu/drink-form/drink-form.html`.

# API ENDPOINTS AND DOCUMENTATION

**All GET Endpoints**
`GET '/drinks'`

*   Fetches a list of *drinks* with miminal details.
*   It is a route meant for the public.
*   Request Parameters: None
*   Response Body: status code 200 and json {"success": True, "drinks": drinks} where drinks is the list of     drinks or appropriate status code indicating reason for failure
```json
{
  "success": "True",
  "drinks":{
    "id": 1,
    "title": "matcha shake",  
    "recipe": [
          {
            "name": "milk",
            "color": "grey",
            "parts": 1
          }]
}
```

`GET '/drinks-detail'`

*   Fetches list of drinks with more detailed  drink informationi.
*   It requires the 'get:drinks-detail' permission
*   Request Parameters: None
*   Response Body: status code 200 and json {"success": True, "drinks": drinks} where drinks is the list of drinks or appropriate status code indicating reason for failure

```json
{
  "success": "True",
  "drinks":{
    "id": 1,
    "title": "matcha shake",  
    "recipe": [
          {
            "name": "milk",
            "color": "grey",
            "parts": 1
          }]
}
```

**POST ENDPOINT**
`POST '/drinks'`

*   it creates a new row in the drinks table
*   it requires the 'post:drinks' permission
*   Request Body:
```json
{
    "id": -1,
    "title": "matcha shake",  
    "recipe": [
          {
            "name": "milk",
            "color": "grey",
            "parts": 1
          }]
}
```

*   Response body: It returns a single new *drink* object

```json
{
  "success": "True",
  "drinks":{
    "id": 1,
    "title": "matcha shake",  
    "recipe": [
          {
            "name": "milk",
            "color": "grey",
            "parts": 1
          }]
}
```

**PATCH Endpoint**
`PATCH '/drinks/<id>'`

*   It updates a *drinks* row
*   It requires the 'patch:drinks' permission
*   Request Parameter: id - integer
*   Request Body:
```json
{
    "id": 1,
    "title": "matcha shake",  
    "recipe": [
          {
            "name": "milk",
            "color": "grey",
            "parts": 1
          }]
}
```
*   Response body: It returns the partially or fully modified *drink* object
```json
{
  "success": true,
  "drinks":{
    "id": 1,
    "title": "matcha shake",  
    "recipe": [
          {
            "name": "milk",
            "color": "grey",
            "parts": 1
          }]
}
```

**Delete Endpoint**
`DELETE '/drinks/<id>'`

*   Deletes a specified drink using the id of the drink
*   it requires the 'delete:drinks' permission
*   Request Parameter: id - integer
*   Response body:
```json
{
  "success": true, 
  "delete": id
}
```

Thanks for reading up this app documentation!