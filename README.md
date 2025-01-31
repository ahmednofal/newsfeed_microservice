# Flask-based newsfeed microservice

To get started manipulating the data; all endpoints will be listed in the logs.

- [Flask-based newsfeed microservice](#flask-based-newsfeed-microservice)
- [Project Structure](#project-structure)
- [Design](#design)
- [Database](#database)
  - [Setup](#setup)
- [Sample Output](#sample-output) ⬅️
- [Usage](#usage)
  - [Starting the microservice](#starting-the-microservice)
  - [Basic fetching and updating using CURL](#basic-fetching-and-updating-using-curl)
    - [User endpoints](#user-endpoints)
    - [Post endpoints](#post-endpoints)
    - [Comment endpoints](#comment-endpoints)
    - [Like endpoints](#like-endpoints)
    - [Share endpoints](#share-endpoints)
    - [Follow endpoints](#follow-endpoints)

# Project Structure

The project follows a structured layout:
```bash
.
├── app/                      # Main application directory
│   ├── api/                  # API-related code
│   │   ├── deps.py           # Dependencies and utilities for API
│   │   ├── main.py           # Main API entry point
│   │   └── routes/           # Route handlers for different endpoints
│   │       ├── follows.py    # Follow-related routes
│   │       ├── posts.py      # Post-related routes
│   │       └── users.py      # User-related routes
│   ├── core/                 # Core application components
│   │   ├── config.py         # Configuration settings
│   │   ├── db.py             # Database connection and operations
│   ├── db/                   # Database-related files
│   │   └── migrations/       # Database migration scripts
│   │       ├── ddl.sql       # Data Definition Language SQL
│   │       ├── drop_all.sql  # Script to drop all database objects
│   │       └── init_test.sql # Initial test data population
│   ├── Dockerfile            # Docker configuration for the app
│   ├── main.py               # Main application entry point
│   └── requirements.txt      # Python package dependencies
├── docker-compose.yml        # Docker Compose configuration
└── README.md                 # Project documentation
```

# Design
![ERD Diagram](./docs/erd.png)

# Database 
## Setup

To set up the database schema, you can find the DDL file in the following location:

- **DDL SQL File implementing the ERD diagram:** [Database](app/db/migrations/ddl.sql)

The schema is automatically applied when no tables are in the database.
The file can be run via connecting to the database using any connector (e.g vscode extension) and applying the file



# Usage
## Starting the microservice

`FLASK_ENV=dev docker compose -p <some_unique_id> up -d --build`

## Basic fetching and updating using CURL

### User endpoints
```bash
curl -X GET "http://localhost:5000/api/v1/users/"  # Get all users

curl -X GET "http://localhost:5000/api/v1/users/<user_id>"  # Get user by ID

curl -X POST "http://localhost:5000/api/v1/users/" -d '{"handle": "<handle>", "email": "<email>", "password_hash": "<password_hash>"}'  # Create user
```


### Post endpoints
```bash
curl -X GET "http://localhost:5000/api/v1/posts/"  # Get all posts

curl -X GET "http://localhost:5000/api/v1/posts/<post_id>"  # Get post by ID

curl -X POST "http://localhost:5000/api/v1/posts/" -d '{"content": "<content>", "author_id": "<author_id>"}'  # Create post

curl -X PUT "http://localhost:5000/api/v1/posts/<post_id>" -d '{"content": "<updated_content>"}'  # Update post

curl -X DELETE "http://localhost:5000/api/v1/posts/<post_id>"  # Delete post
```
### Sample Output
```bash
curl -X GET "http://localhost:5000/api/v1/posts/" 
{
  "data": {
    "posts": [
      {
        "author_id": 1,
        "content": "This is the first post!",
        "created_at": "Sun, 13 Oct 2024 05:27:12 GMT",
        "id": 1
      },
      {
        "author_id": 1,
        "content": "This is the second post!",
        "created_at": "Sun, 13 Oct 2024 05:27:12 GMT",
        "id": 2
      },
      {
        "author_id": 2,
        "content": "This is the third post!",
        "created_at": "Sun, 13 Oct 2024 05:27:12 GMT",
        "id": 3
      },
      {
        "author_id": 3,
        "content": "This is the fourth post!",
        "created_at": "Sun, 13 Oct 2024 05:27:12 GMT",
        "id": 4
      },
      {
        "author_id": 2,
        "content": "This is a deleted post!",
        "created_at": "Sun, 13 Oct 2024 05:27:12 GMT",
        "id": 5
      }
    ]
  }
}
```
### Comment endpoints
```bash
curl -X GET "http://localhost:5000/api/v1/posts/<post_id>/comments"  # Get comments for post

curl -X GET "http://localhost:5000/api/v1/posts/<post_id>/comments/<comment_id>"  # Get comment by ID

curl -X POST "http://localhost:5000/api/v1/posts/<post_id>/comments" -d '{"content": "<content>", "author_id": "<author_id>"}'  # Create comment

curl -X PUT "http://localhost:5000/api/v1/posts/<post_id>/comments/<comment_id>" -d '{"content": "<updated_content>"}'  # Update comment

curl -X DELETE "http://localhost:5000/api/v1/posts/<post_id>/comments/<comment_id>"  # Delete comment
```

### Like endpoints
```bash
curl -X GET "http://localhost:5000/api/v1/posts/<post_id>/likes"  # Get likes for post

curl -X GET "http://localhost:5000/api/v1/posts/<post_id>/likes/<like_id>"  # Get like by ID

curl -X POST "http://localhost:5000/api/v1/posts/<post_id>/likes" -d '{"liker_id": "<liker_id>"}'  # Create like

curl -X DELETE "http://localhost:5000/api/v1/posts/<post_id>/likes/<like_id>"  # Delete like
```

### Share endpoints
```bash
curl -X GET "http://localhost:5000/api/v1/posts/<post_id>/shares"  # Get shares for post

curl -X GET "http://localhost:5000/api/v1/posts/<post_id>/shares/<share_id>"  # Get share by ID

curl -X POST "http://localhost:5000/api/v1/posts/<post_id>/shares" -d '{"sharer_id": "<sharer_id>"}'  # Create share

curl -X DELETE "http://localhost:5000/api/v1/posts/<post_id>/shares/<share_id>"  # Delete share
```

### Follow endpoints
```bash
curl -X GET "http://localhost:5000/api/v1/follows/"  # Get all follows

curl -X POST "http://localhost:5000/api/v1/follows/" -d '{"follower_id": "<follower_id>", "followed_id": "<followed_id>"}'  # Create follow

curl -X GET "http://localhost:5000/api/v1/follows/<followed_id>"  # Get followers for followed user

curl -X GET "http://localhost:5000/api/v1/follows/<follower_id>"  # Get followed users for follower
```
