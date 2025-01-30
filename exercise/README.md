<h1>
  <span class="headline">FastAPI Serializers and Controllers Lab</span>
  <span class="subhead">Exercise</span>
</h1>

**Learning Objective:** By the end of this lab, you will be able to create and integrate a comments controller in a FastAPI application. This controller will allow users to add, retrieve, update, and delete comments related to teas.

## Implementing a Comments Controller in FastAPI

### Prerequisites

Before starting, ensure that your application already includes:

- A **working CRUD API** for teas using FastAPI and SQLAlchemy ORM.
- **Pydantic serializers** (`TeaSchema`) to validate and format data.
- The following tea-related API endpoints:

| HTTP Method | Endpoint         | Description                     |
| ----------- | ---------------- | ------------------------------- |
| **GET**     | `/teas/`         | Retrieve all teas.              |
| **GET**     | `/teas/{tea_id}` | Retrieve an existing tea by ID. |
| **POST**    | `/teas/`         | Add a new tea.                  |
| **PUT**     | `/teas/{tea_id}` | Update an existing tea by ID.   |
| **DELETE**  | `/teas/{tea_id}` | Delete a tea by ID.             |

### Overview

In this lab, you will:

1. **Create a Comments Controller**: A new file, `controllers/comments.py`, to handle comment-related API requests.
2. **Implement CRUD Endpoints**: Add routes for creating, retrieving, updating, and deleting comments.
3. **Test the Endpoints**: Use FastAPI’s interactive documentation to ensure all routes work correctly.

## Create the Comments Controller

A controller in FastAPI is a **module that defines API routes**. You will create a new file to handle comment-related operations.

1. **Create a new file:**  
   📂 `controllers/comments.py`

2. **Inside `comments.py`, set up a router**:

```python
from fastapi import APIRouter, Depends, HTTPException
from sqlalchemy.orm import Session
from models.comment import CommentModel
from models.tea import TeaModel
from serializers.comment import CommentSchema
from typing import List
from database import get_db

# Initialize the router
router = APIRouter()
```

## Add CRUD Endpoints for Comments

Each endpoint performs a different action on comments. Below is a **RESTful API routing table** for handling comments.

| HTTP Method | Endpoint                  | Description                               |
| ----------- | ------------------------- | ----------------------------------------- |
| **GET**     | `/teas/{tea_id}/comments` | Retrieve all comments for a specific tea. |
| **GET**     | `/comments/{comment_id}`  | Retrieve a single comment by its ID.      |
| **POST**    | `/teas/{tea_id}/comments` | Add a new comment to a specific tea.      |
| **PUT**     | `/comments/{comment_id}`  | Update an existing comment by its ID.     |
| **DELETE**  | `/comments/{comment_id}`  | Delete a comment by its ID.               |

### 1️. Get all `comments` for a `tea` 

_This one has been done for you as an example_

This endpoint retrieves all comments linked to a specific tea.

```python
@router.get("/teas/{tea_id}/comments", response_model=List[CommentSchema])
def get_comments_for_tea(tea_id: int, db: Session = Depends(get_db)):
    tea = db.query(TeaModel).filter(TeaModel.id == tea_id).first()
    if not tea:
        raise HTTPException(status_code=404, detail="Tea not found")
    return tea.comments
```

### 2. Get a single `comment` by `id`

This endpoint retrieves a specific comment using its unique ID.

```python
@router.get("/comments/{comment_id}", response_model=CommentSchema)

```

### 3. Add a new `comment` to a `tea`

This endpoint creates a new comment and links it to an existing tea.

```python
@router.post("/teas/{tea_id}/comments", response_model=CommentSchema)

```

### 4. Update an existing `comment` by `id`

This endpoint modifies an existing comment.

```python
@router.put("/comments/{comment_id}", response_model=CommentSchema)

```

### 5. Delete a `comment` by `id`

This endpoint removes a comment from the database.

```python
@router.delete("/comments/{comment_id}")

```

## Register the Comments Controller

For FastAPI to recognize the new routes, you need to **register the router** in `main.py`.

## Testing Your API

FastAPI provides built-in **interactive API documentation** to test your endpoints.

1. Run your FastAPI application:
   
```sh
uvicorn main:app --reload
```

2. Open your browser and go to: [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)

3. Expand the `/teas/{tea_id}/comments` and `/comments/{comment_id}` sections.

4. Try **creating**, **retrieving**, **updating**, and **deleting** comments.

