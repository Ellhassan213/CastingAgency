# The Cool Casting Agency (^^)

The Cool Casting Agency models a company that is responsible for creating movies and managing and assigning actors to those movies. You are an Executive Producer within the company and are creating a system to simplify and streamline your process. 

## Getting Started

### Installing Dependencies

#### Python 3.9.2

Follow instructions to install the latest version of python for your platform in the [python docs](https://docs.python.org/3/using/unix.html#getting-and-installing-the-latest-version-of-python)

#### Virtual Environment

We recommend working within a virtual environment whenever using Python for projects. This keeps your dependencies for each project separate and organized. Instructions for setting up a virtual environment for your platform can be found in the [python docs](https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/)

#### PIP Dependencies

Once you have your virtual environment setup and running, install dependencies by navigating to the `/CastingAgency` directory and running:

```bash
pip install -r requirements.txt
```

This will install all of the required packages we selected within the `requirements.txt` file.

##### Key Dependencies

- [Flask](http://flask.pocoo.org/)  is a lightweight backend microservices framework. Flask is required to handle requests and responses.

- [SQLAlchemy](https://www.sqlalchemy.org/) is the Python SQL toolkit and ORM we'll use handle the lightweight sqlite database. You'll primarily work in app.py and can reference models.py. 

- [Flask-CORS](https://flask-cors.readthedocs.io/en/latest/#) is the extension we'll use to handle cross origin requests from our frontend server. 

## Database Setup
With Postgres running, restore a database using the test_db.psql file provided. From the CastingAgency folder in terminal run:
```bash
psql casting_agency_db < test_db.psql
```

## Running the server

From within the `CastingAgency` directory first ensure you are working using your created virtual environment.

To run the server, execute:

```bash
export FLASK_APP=app,py
export FLASK_ENV=development
flask run
```

Setting the `FLASK_ENV` variable to `development` will detect file changes and restart the server automatically.

Setting the `FLASK_APP` variable to `app.py` directs flask to use the `CastingAgency` directory and the `app.py` file to find the application. 

#### Base URL

The app can be run locally, hosted at default http://127.0.0.1:5000/
The App is also deployed on Heroku, default at https://lawalcastingagency.herokuapp.com

#### API Keys/Authentication

This application is utilising Auth0 for Authentication.

## Auth0 Setup
1. Create an Auth0 account or use an existing one if you wish
2. Choose a unique tenant domain
3. Create a new Single Page Application for the Casting Agency
4. Create a new API for the Casting Agency
  - Navigate to API Settings
    1. Enable "RBAC"
    2. Enable "Add Permissions in the Access Token
    3. Set Token Expiration and Token Expiration for Browser Flows to value >= "80,000"
5. Navigate to API Permissions tab and create permissions below
```bash
  - get:movies
  - delete:movie
  - post:movie
  - patch:movie
  - get:actors
  - delete:actor
  - post:actor
  - patch:actor
```
6. Go to User Management -> Roles: Create the following roles
  - Casting Assistant
    - Can get:movies and get:actors
  - Casting Director
    - All permissions a casting assistance has and ...
    - post:actor or delete:actor
    - patch:movie or patch:actor
  - Executive Producer
    - All permissions a Casting Director has and ...
    - post:movie or delete:movie

7. Go to User Management -> Users
  - Create 3 seperate users
  - Assign each user with a role from step 6

## Error Handling

Errors are returned as JSON objects in the following format:
```bash
{
    "success": False,
    "error": 400,
    "message": "bad request"
}
```

The app will return the following error types when request fail:

- 400: Bad request
- 401: No authorization
- 404: Resource not found
- 422: Unprocessable
- 405: Method not allowed
- 500: Internal server error

## Resource Endpoints

You can test endpoints using Curl, Postman or through the unit test integrated in the application.
The example requests provided using Curl were achieved by testing without the authentication layer.
A more complete test should be done using the other two methods.

### GET /movies

#### General
- Returns a dictionary of all movies and success value

#### Sample Request

```bash
curl http://127.0.0.1:5000/movies
```

#### Sample Response

```bash
{
  "movies": [
    {
      "id": 1, 
      "release_date": "Sat, 12 Jun 2021 00:00:00 GMT", 
      "title": "Avengers"
    }
  ], 
  "success": true
}
```

### GET /actors

#### General
- Returns a dictionary of all actors and success value

#### Sample Request

```bash
curl http://127.0.0.1:5000/actors
```

#### Sample Response

```bash
{
  “actors: [
    {
      "age": 30, 
      "gender": "Male", 
      "id": 1, 
      "name": "Tommy Egan"
    }
  ], 
  "success": true
}
```

### POST /movies

#### General
- Creates a new movie record
- Returns ID of created movie and success value

#### Sample Request

```bash
curl -X POST -H "Content-Type: application/json" -d '{"title": "Big Bang", "release_date": "2020-06-12"}' http://127.0.0.1:5000/movies 
```

#### Sample Response

```bash
{
  "created": 2, 
  "movies": [
    {
      "id": 1, 
      "release_date": "Sat, 12 Jun 2021 00:00:00 GMT", 
      "title": "Avengers"
    }, 
    {
      "id": 2, 
      "release_date": "Fri, 12 Jun 2020 00:00:00 GMT", 
      "title": "Big Bang"
    }
  ], 
  "success": true
}
```

### POST /actors

#### General
- Creates a new actor record
- Returns ID of created actor and success value

#### Sample Request

```bash
curl -X POST -H "Content-Type: application/json" -d '{"name": "Ghost", "age": "35", "gender": "Male"}' 
http://127.0.0.1:5000/actors
```

#### Sample Response

```bash
{
  "actors": [
    {
      "age": 30, 
      "gender": "Male", 
      "id": 1, 
      "name": "Tommy Egan"
    }, 
    {
      "age": 35, 
      "gender": "Male", 
      "id": 2, 
      "name": "Ghost"
    }
  ], 
  "created": 2, 
  "success": true
}
```

### PATCH /movies/{id}

#### General
- Modifies a movie record
- Returns ID of modified movie, success value and list of movies

#### Sample Request

```bash
curl -X PATCH -H "Content-Type: application/json" -d '{"title": "BOOM", "release_date": "2021-10-25"}'
http://127.0.0.1:5000/movies/3
```

#### Sample Response

```bash
{
  "movies": [
    {
      "id": 1, 
      "release_date": "Sat, 12 Jun 2021 00:00:00 GMT", 
      "title": "Avengers"
    }, 
    {
      "id": 2, 
      "release_date": "Fri, 12 Jun 2020 00:00:00 GMT", 
      "title": "Big Bang"
    }, 
    {
      "id": 3, 
      "release_date": "Mon, 25 Oct 2021 00:00:00 GMT", 
      "title": "BOOM"
    }
  ], 
  "patched_movie": "3", 
  "success": true
}
```

### PATCH /actors/{id}

#### General
- Modifies an actor record
- Returns ID of modified actor, success value and list of actors

#### Sample Request

```bash
curl -X PATCH -H "Content-Type: application/json" -d '{"name": "Tasha", "age": "25", "gender": "Female"}'
http://127.0.0.1:5000/actors/2
```

#### Sample Response

```bash
{
  "actors": [
    {
      "age": 30, 
      "gender": "Male", 
      "id": 1, 
      "name": "Tommy Egan"
    }, 
    {
      "age": 25, 
      "gender": "Female", 
      "id": 2, 
      "name": "Tasha"
    }
  ], 
  "patched_actor": "2", 
  "success": true
}
```

### DELETE /movies/{id}

#### General
- Deletes the record of a movie
- Returns ID of deleted movie and success value

#### Sample Request

```bash
curl -X DELETE http://127.0.0.1:5000/movie/4
```

#### Sample Response

```bash
{
  "deleted_movie": "4", 
  "success": true
}
```

### DELETE /actors/{id}

#### General
- Deletes the record of an actor
- Returns ID of deleted actor and success value

#### Sample Request

```bash
curl -X DELETE http://127.0.0.1:5000/actor/3
```

#### Sample Response

```bash
{
  "deleted_actor": "3", 
  "success": true
}
```

## Testing

#### General
Each endpoint is tested for successful and unsuccessful execution. Expected outputs are used for validation
See tests below and feel free to add more useful tests:
- test_get_all_movies_success: Test successful retrieval of all movies
- test_get_all_movies_404_not_found: Test unsuccessful deletion of a movie that does not exist
- test_post_movies_success: Test successful addition of a movie
- test_post_movies_400_bad_request: Test unsuccessful addition of movie due to a bad request
- test_post_movies_401_not_authorized: Test unsuccessful addition of a movie due to user not authorized
- test_patch_movie_success: Test successful modification of a movie record
- test_patch_movie_404_not_found: Test unsuccessful modification of a movie that does not exist
- test_delete_movie_success: Test successful deletion of a movie
- test_delete_movie_404_not_found: Test unsuccessful deletion of a movie that does not exist

Note: There are more actor related tests in test_app.py. The test cases are thesame as movie tests

#### Test Execution
To run the tests, run
```
dropdb casting_agency_db_test
createdb casting_agency_db_test
psql casting_agency_db_test < test_db.psql
python test_app.py
```

## Deployment - HEROKU 
