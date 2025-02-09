# All the code snippets for FastAPI Development

## Creating the virtual env

```python
python3 -mvenv virtualenvname
```

### Activate the virtualenv

In the directory of the virtual environment run the command

```python
source bin/activate
```

### Install all dependencies

```python
pip install fastapi[all]
```

### Running the Server

```python
uvicorn [app-name]:app --reload
```

## BaseModel, UUID, Field & Optional

The BaseModel allows us to create models easily this can be imported from pydantic.
It also allows us to validate the model. Below we are going to create a simple books model.

```python
from pydantic import BaseModel, Field
from typing import Optional
from uuid import UUID

class Book(BaseModel):
    id : UUID
    title: str = Field(min_length=1)
    author: str
    description: Optional[str] = Field(title="Description of book", min_length=1, max_length=100)
    rating: int
```

- The Field method allows to validate client input.
- Optional allows users to leave input blank.
- UUID handles the id field
