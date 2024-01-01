- Import `FastAPI`.
- Create an `app` instance.
- Write a **path operation decorator** (like `@app.get("/")`).
- Write a **path operation function** (like `def root(): ...` above).
- Run the development server (like `uvicorn main:app --reload`).

**FastAPI** will recognize that the function parameters that match path parameters should be **taken from the path**, and that function parameters that are declared to be Pydantic models should be **taken from the request body**.

## Query params
#### **Multiple values in query params**: 

```Python
@app.get("/items/") 
async def read_items(q: Annotated[list[str] | None, Query()] = None): 
	query_items = {"q": q} 
	return query_items
```

The response:

```JSON
{ "q": [ "foo", "bar" ] }
```

*To declare a query parameter with a type of `list`, like in the example above, you need to explicitly use `Query`, otherwise it would be interpreted as a request body.*

Defaults are also possible:

```Python
@app.get("/items/")
async def read_items(q: Annotated[List[str], Query()] = ["foo", "bar"]):
    query_items = {"q": q}
    return query_items
```

!!! `List[int]` would check (and document) that the contents of the list are integers. But `list` alone wouldn't.

#### Declare more metadata

```Python
@app.get("/items/")
async def read_items(
    q: Annotated[
	    str | None, 
	    Query(
		    title="Query string", 
		    description="Query string for the items to search in the database that have a good match"
		    min_length=3,
		    max_length=100,
		    pattern='regular_expression'
		    deprecated=True, # the docs will show it as deprecated
		    include_in_schema=False, # exclude param from any auto documentation schema
		)
	] = None
):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results

```

#### Alias parameters

http://127.0.0.1:8000/items/?item-query=foobaritems

```Python
@app.get("/items/")
async def read_items(q: Annotated[str | None, Query(alias="item-query")] = None):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results

```

## Path params

```Python
@app.get("/items/{item_id}")
async def read_items(
	item_id: Annotated[
		int, 
		Path(
			title="The ID of the item to get", 
			gt=0, 
			le=1000
		)
	],
    q: str,
):
    results = {"item_id": item_id}
    if q:
        results.update({"q": q})
    return results

```

Number validations also work for `float` values.

Declare numeric validations:
	- `gt`: `g`reater `t`han
	- `ge`: `g`reater than or `e`qual
	- `lt`: `l`ess `t`han
	- `le`: `l`ess than or `e`qual
## Body

### Multiple body parameters

```Python
class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


class User(BaseModel):
    username: str
    full_name: str | None = None


@app.put("/items/{item_id}")
async def update_item(
		item_id: int, 
		item: Item, 
		user: User,
		importance: Annotated[int, Body()] # Singular value in body
	):
    results = {"item_id": item_id, "item": item, "user": user}
    return results

```

Expected body:

```JSON
{
    "item": {
        "name": "Foo",
        "description": "The pretender",
        "price": 42.0,
        "tax": 3.2
    },
    "user": {
        "username": "dave",
        "full_name": "Dave Grohl"
    },
    "importance": 5
}

```

!!! `Body` also has all the same extra validation and metadata parameters as `Query`,`Path` and others you will see later.

In case on single body param the `Body(embed=True)` should be used:

```Python
@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Annotated[Item, Body(embed=True)]):
    results = {"item_id": item_id, "item": item}
    return results

```

In this case **FastAPI** will expect a body like:

```JSON
{
    "item": {
        "name": "Foo",
        "description": "The pretender",
        "price": 42.0,
        "tax": 3.2
    }
}

```

instead of:

```JSON
{
    "name": "Foo",
    "description": "The pretender",
    "price": 42.0,
    "tax": 3.2
}

```

### Fields and Nested Models

```Python

class Image(BaseModel): 
	url: HttpUrl 
	name: str

class Item(BaseModel):
    name: str
    description: str | None = Field(
        default=None, 
        title="The description of the item", 
        max_length=300,
    )
    price: float = Field(
	    gt=0, 
	    description="The price must be greater than zero",
	)
    tax: float | None = None
    tags: List[str] = []
    unique_tags: set[str] = set(). # if you receive a request with duplicate data, it will be converted to a set of unique items.
    image: Image | None = None
	images: list[Image] | None = None
```

!!! `Field` works the same way as `Query`, `Path` and `Body`, it has all the same parameters, etc.

More info about specialised types: https://docs.pydantic.dev/latest/usage/types/types/ 
	1. `UUID`
	2. `datetime.datetime`: a `str` in ISO 8601 format, like: `2008-09-15T15:53:00+05:00`
	3. `datetime.date`: a `str` in ISO 8601 format, like: `2008-09-15`
	4. `datetime.time`: a `str` in ISO 8601 format, like: `14:23:55.003`
	5. `datetime.timedelta`: a `float` of total seconds
	6. `frozenset`: 
		1. requests: a list will be read, eliminating duplicates and converting it to a `set`
		2. responses: he `set` will be converted to a `list`
	7. `bytes`: standard Python `Decimal`. In requests and responses handled the same as a `float`

#### Bodies of pure lists

```Python
class Image(BaseModel):
    url: HttpUrl
    name: str


@app.post("/images/multiple/")
async def create_multiple_images(images: List[Image]):
    return images

```

#### Bodies of arbitrary `dict`s

!!! This would be useful if you want to receive keys that you don't already know.

!!! Other useful case is when you want to have keys of other type, e.g. `int`

```Python
@app.post("/index-weights/")
async def create_index_weights(weights: Dict[int, float]):
    return weights
```
## Cookie params

```Python
@app.get("/items/")
async def read_items(ads_id: Annotated[str | None, Cookie()] = None):
    return {"ads_id": ads_id}
```

## Header params

```Python
@app.get("/items/")
async def read_items(user_agent: Annotated[str | None, Header()] = None):
    return {"User-Agent": user_agent}
```

!!! By default, `Header` will convert the parameter names characters from underscore (`_`) to hyphen (`-`) to extract and document the headers. (because vars like `user-agent` are invalid in Python)

!!! HTTP headers are case-insensitive, so, you can declare them with standard Python style (also known as "snake_case") => can use `user_agent`

### Duplicate headers

```Python
@app.get("/items/")
async def read_items(x_token: Annotated[list[str] | None, Header()] = None):  # `X-Token` can appear more than once
    return {"X-Token values": x_token}

```

```
X-Token: foo 
X-Token: bar
```

## Response Model - Return Type

For example, you could want to **return a dictionary** or a database object, but **declare it as a Pydantic model**.

```Python
@app.get("/items/", response_model=list[Item])
async def read_items() -> Any:
    return [
        {"name": "Portal Gun", "price": 42.0},
        {"name": "Plumbus", "price": 32.0},
    ]

```

%% If you have strict type checks in your editor, mypy, etc, you can declare the function return type as `Any`. That way you tell the editor that you are intentionally returning anything. But FastAPI will still do the data documentation, validation, filtering, etc. with the `response_model`. %%

If you declare both a return type and a `response_model`, the ==`response_model` will take priority and be used by FastAPI.==

If you do not want to have the default data validation, documentation, filtering, etc. that is performed by FastAPI, but you might want to still keep the return type annotation in the function to get the support from tools like editors and type checkers (e.g. mypy) you can ==disable the response model generation by setting `response_model=None`==

```Python
@app.get("/portal", response_model=None)
async def get_portal(teleport: bool = False) -> Response | dict:
    if teleport:
        return RedirectResponse(url="https://www.youtube.com/watch?v=dQw4w9WgXcQ")
    return {"message": "Here's your interdimensional portal."}
```

*This will make FastAPI skip the response model generation and that way you can have any return type annotations you need without it affecting your FastAPI application.*
##### To return only the values explicitly set. - `response_model_exclude_unset` 

1. `response_model_exclude_defaults=True`
2. `response_model_exclude_none=True`

```Python
class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float = 10.5
    tags: list[str] = []


items = {
    "foo": {"name": "Foo", "price": 50.2},
    "bar": {"name": "Bar", "description": "The bartenders", "price": 62, "tax": 20.2},
    "baz": {"name": "Baz", "description": None, "price": 50.2, "tax": 10.5, "tags": []},
}

@app.get("/items/{item_id}", response_model=Item, response_model_exclude_unset=True)
async def read_item(item_id: str):
    return items[item_id]
```

and those default values won't be included in the response, only the values actually set.

So, if you send a request to that _path operation_ for the item with ID `foo`, the response (not including default values) will be:

```JSON
{
    "name": "Foo",
    "price": 50.2
}
```

==`response_model_include` and `response_model_exclude`It is still recommended to use multiple classes, instead of these parameters.==

#### Unwrapping a `dict` and extra keywords

```Python
UserInDB(**user_in.dict(), hashed_password=hashed_password)
```

#### `Union` or `anyOf`

You can declare a response to be the `Union` of two types, that means, that the response would be any of the two.

It will be defined in OpenAPI with `anyOf`.

%% When defining a [`Union`](https://pydantic-docs.helpmanual.io/usage/types/#unions), include the ==most specific type first, followed by the less specific type==. In the example below, the more specific `PlaneItem` comes before `CarItem` in `Union[PlaneItem, CarItem]`. %%

**Python 3.10:**
	For typing it is possible to use `some_variable: PlaneItem | CarItem`. But if we put that in `response_model=PlaneItem | CarItem` we would get an error, because Python would try to perform an **invalid operation** between `PlaneItem` and `CarItem` instead of interpreting that as a type annotation.

```Python
class BaseItem(BaseModel):
    description: str
    type: str


class CarItem(BaseItem):
    type: str = "car"


class PlaneItem(BaseItem):
    type: str = "plane"
    size: int


items = {
    "item1": {"description": "All my friends drive a low rider", "type": "car"},
    "item2": {
        "description": "Music is my aeroplane, it's my aeroplane",
        "type": "plane",
        "size": 5,
    },
}


@app.get("/items/{item_id}", response_model=Union[PlaneItem, CarItem])
async def read_item(item_id: str):
    return items[item_id]
```

## Response Status Code

The `status_code` parameter receives a number with the HTTP status code.

! `status_code` can alternatively also receive an `IntEnum`, such as Python's [`http.HTTPStatus`](https://docs.python.org/3/library/http.html#http.HTTPStatus)

## Form Data

When you need to receive form fields instead of JSON, you can use `Form`.

`pip install python-multipart`

```Python
@app.post("/login/")
async def login(username: Annotated[str, Form()], password: Annotated[str, Form()]):
    return {"username": username}
```

For example, in one of the ways the OAuth2 specification can be used (called "password flow") it is required to send a `username` and `password` as form fields.

! `Form` is a class that inherits directly from `Body`

!! To declare form bodies, you need to use `Form` explicitly, because without it the parameters would be interpreted as query parameters or body (JSON) parameters.

!! Data from forms is normally encoded using the "media type" `application/x-www-form-urlencoded`. But when the form includes files, it is encoded as `multipart/form-data`. You'll read about handling files in the next chapter.

!! You can declare multiple `Form` parameters in a _path operation_, but you can't also declare `Body` fields that you expect to receive as JSON, as the request will have the body encoded using `application/x-www-form-urlencoded` instead of `application/json`.

## Request Files

You can define files to be uploaded by the client using `File`.

```Python
@app.post("/files/")
async def create_file(file: Annotated[bytes, File()]):
    return {"file_size": len(file)}


@app.post("/uploadfile/")
async def create_upload_file(file: UploadFile):
    return {"filename": file.filename}
```

If you declare the type of your _path operation function_ parameter as `bytes`, **FastAPI** will read the file for you and you will receive the contents as `bytes`. Have in mind that this means that the whole contents will be stored in memory. This will work well for small files.

==Using `UploadFile` has several advantages over `bytes`:==
- You don't have to use `File()` in the default value of the parameter.
- It uses a "spooled" file:
    - A file stored in memory up to a maximum size limit, and after passing this limit it will be stored in disk.
- This means that it will work well for large files like images, videos, large binaries, etc. without consuming all the memory.
- You can get metadata from the uploaded file.
- It has a [file-like](https://docs.python.org/3/glossary.html#term-file-like-object) `async` interface.
- It exposes an actual Python [`SpooledTemporaryFile`](https://docs.python.org/3/library/tempfile.html#tempfile.SpooledTemporaryFile) object that you can pass directly to other libraries that expect a file-like object.

! When you use the `async` methods, **FastAPI** runs the file methods in a threadpool and awaits for them.
#### What is "Form Data"

The way HTML forms (`<form></form>`) sends the data to the server normally uses a "special" encoding for that data, it's different from JSON.
#### Optional File Upload

```Python
@app.post("/files/")
async def create_file(file: Annotated[bytes | None, File()] = None):
    if not file:
        return {"message": "No file sent"}
    else:
        return {"file_size": len(file)}


@app.post("/uploadfile/")
async def create_upload_file(file: UploadFile | None = None):
    if not file:
        return {"message": "No upload file sent"}
    else:
        return {"filename": file.filename}
```
#### Multiple File Uploads

It's possible to upload several files at the same time.

They would be associated to the same "form field" sent using "form data".

```Python
@app.post("/files/")
async def create_files(
		files: Annotated[
			List[bytes], 
			File(description="A file read as bytes")
		]
	):
    return {"file_sizes": [len(file) for file in files]}


@app.post("/uploadfiles/")
async def create_upload_files(
		files: Annotated[
			List[UploadFile],
			File(description="Multiple files as UploadFile"),
		]
	):
    return {"filenames": [file.filename for file in files]}
```
## Request Forms and Files

```Python
@app.post("/files/")
async def create_file(
    file: Annotated[bytes, File()],
    fileb: Annotated[UploadFile, File()],
    token: Annotated[str, Form()],
):
    return {
        "file_size": len(file),
        "token": token,
        "fileb_content_type": fileb.content_type,
    }
```
## Handling Errors

To return HTTP responses with errors to the client you use `HTTPException`

```Python
@app.get("/items/{item_id}")
async def read_item(item_id: str):
    if item_id not in items:
        raise HTTPException(
	        status_code=404, 
	        detail="Item not found",
	        headers={"X-Error": "There goes my error"},
	    )
    return {"item": items[item_id]}
```

! When raising an `HTTPException`, you can pass any value that can be converted to JSON as the parameter `detail`, not only `str`. You could pass a `dict`, a `list`, etc.
### Install custom exception handlers

```Python
class UnicornException(Exception):
    def __init__(self, name: str):
        self.name = name

@app.exception_handler(UnicornException)
async def unicorn_exception_handler(request: Request, exc: UnicornException):
    return JSONResponse(
        status_code=418,
        content={"message": f"Oops! {exc.name} did something. There goes a rainbow..."},
    )

@app.get("/unicorns/{name}")
async def read_unicorn(name: str):
    if name == "yolo":
        raise UnicornException(name=name)
    return {"unicorn_name": name}
```
### Override the default exception handlers

**FastAPI** has some default exception handlers.

These handlers are in charge of returning the default JSON responses when you `raise` an `HTTPException` and when the request has invalid data.

You can override these exception handlers with your own.

1. `@app.exception_handler(RequestValidationError)` to decorate the exception handler.
2. `@app.exception_handler(StarletteHTTPException)` to override the `HTTPException` handler.
3. `@app.exception_handler(RequestValidationError)` to override the `RequestValidationError` and log the body and debug it, return it to the user, etc. The `RequestValidationError` contains the `body` it received with invalid data.

```Python

from starlette.exceptions import HTTPException as StarletteHTTPException

@app.exception_handler(StarletteHTTPException)
async def http_exception_handler(request, exc):
    return PlainTextResponse(str(exc.detail), status_code=exc.status_code)

from fastapi.exceptions import RequestValidationError

@app.exception_handler(RequestValidationError)
async def validation_exception_handler(request, exc):
    return PlainTextResponse(str(exc), status_code=400)

from fastapi.exceptions import RequestValidationError

@app.exception_handler(RequestValidationError)
async def validation_exception_handler(request: Request, exc: RequestValidationError):
    return JSONResponse(
        status_code=status.HTTP_422_UNPROCESSABLE_ENTITY,
        content=jsonable_encoder({"detail": exc.errors(), "body": exc.body}),
    )
```
#### FastAPI's `HTTPException` vs Starlette's `HTTPException`

**FastAPI** has its own `HTTPException`.

And **FastAPI**'s `HTTPException` error class inherits from Starlette's `HTTPException` error class.

The only difference, is that **FastAPI**'s `HTTPException` allows you to add headers to be included in the response.

This is needed/used internally for OAuth 2.0 and some security utilities.

So, ==you can keep raising **FastAPI**'s `HTTPException` as normally in your code.==

==But when you register an exception handler, you should register it for Starlette's `HTTPException`.==

This way, if any part of Starlette's internal code, or a Starlette extension or plug-in, raises a Starlette `HTTPException`, your handler will be able to catch and handle it.

#### Re-use **FastAPI**'s exception handlers

```Python
from fastapi import FastAPI, HTTPException
from fastapi.exception_handlers import (
    http_exception_handler,
    request_validation_exception_handler,
)
from fastapi.exceptions import RequestValidationError
from starlette.exceptions import HTTPException as StarletteHTTPException

@app.exception_handler(StarletteHTTPException)
async def custom_http_exception_handler(request, exc):
    print(f"OMG! An HTTP error!: {repr(exc)}")
    return await http_exception_handler(request, exc)

@app.exception_handler(RequestValidationError)
async def validation_exception_handler(request, exc):
    print(f"OMG! The client sent invalid data!: {exc}")
    return await request_validation_exception_handler(request, exc)
```

%% In this example you are just `print`ing the error with a very expressive message, but you get the idea. You can use the exception and then just re-use the default exception handlers. %%

## Path Operation Configuration

1. `status_code` to be used in the response of your _path operation_.

2. `tags` with a `list` of `str` (commonly just one `str`). 

They will be added to the OpenAPI schema and used by the automatic documentation interface
   
 ```Python
 @app.post("/items/", response_model=Item, tags=["items"])
 async def get_items():
	 pass
 
 
 class Tags(Enum): 
	 items = "items" 
	 users = "users"

@app.get("/items/", tags=[Tags.items])
 async def get_items():
	 pass
```
   
3. `summary` , `description` and `response_description`

```Python
@app.post(
    "/items/",
    response_model=Item,
    summary="Create an item",
    description="Create an item with all the information, name, description, price, tax and a set of unique tags",
    response_description="The created item",
)
async def create_item(item: Item):
    return item
```

Description from doc-string:

```Python
@app.post("/items/", response_model=Item, summary="Create an item")
async def create_item(item: Item):
    """
    Create an item with all the information:

    - **name**: each item must have a name
    - **description**: a long description
    - **price**: required
    - **tax**: if the item doesn't have tax, you can omit this
    - **tags**: a set of unique tag strings for this item
    """
    return item
```

! Notice that `response_description` refers specifically to the response, the `description` refers to the _path operation_ in general.

! OpenAPI specifies that each _path operation_ requires a response description. => If you don't provide one, **FastAPI** will automatically generate one of "Successful response".

4. `deprecated` if you need to mark a _path operation_ as deprecated, but without removing it.

```Python
@app.get("/elements/", tags=["items"], deprecated=True)
async def read_elements():
    return [{"item_id": "Foo"}]
```

![[Pasted image 20230930140616.png]]

## JSON Compatible Encoder

There are some cases where you might need to convert a data type (like a Pydantic model) to something compatible with JSON (like a `dict`, `list`, etc).

For example, if you need to store it in a database.

For that, **FastAPI** provides a ==`jsonable_encoder()`== function.

```Python
class Item(BaseModel):
    title: str
    timestamp: datetime
    description: str | None = None

item = Item(title='title', timestamp=now())
json_compatible_item_data = jsonable_encoder(item)
```

In this example, it would convert the Pydantic model to a `dict`, and the `datetime` to a `str`.

! It doesn't return a large `str` containing the data in JSON format (as a string). It returns a Python standard data structure (e.g. a `dict`) with values and sub-values that are all compatible with JSON.

!! `jsonable_encoder` is actually used by **FastAPI** internally to convert data. But it is useful in many other scenarios.

## Body - Updates

`item.dict(exclude_unset=True)` That would generate a `dict` with only the data that was set when creating the `item` model, excluding default values.

Now, you can create a copy of the existing model using `.copy()`, and pass the `update` parameter with a `dict` containing the data to update.

`stored_item_model.copy(update=update_data)`

```Python
class Item(BaseModel):
    name: str | None = None
    description: str | None = None
    price: float | None = None
    tax: float = 10.5
    tags: list[str] = []


items = {
    "foo": {"name": "Foo", "price": 50.2},
    "bar": {"name": "Bar", "description": "The bartenders", "price": 62, "tax": 20.2},
    "baz": {"name": "Baz", "description": None, "price": 50.2, "tax": 10.5, "tags": []},
}

@app.patch("/items/{item_id}", response_model=Item)
async def update_item(item_id: str, item: Item):
    stored_item_data = items[item_id]
    stored_item_model = Item(**stored_item_data)
    update_data = item.dict(exclude_unset=True)
    updated_item = stored_item_model.copy(update=update_data)
    items[item_id] = jsonable_encoder(updated_item)
    return updated_item
```

# Dependencies

**"Dependency Injection"** means, in programming, that there is a way for your code (in this case, your _path operation functions_) to declare things that it requires to work and use: "dependencies".

And then, that system (in this case **FastAPI**) will take care of doing whatever is needed to provide your code with those needed dependencies ("inject" the dependencies).

This is very useful when you need to:
- Have shared logic (the same code logic again and again).
- Share database connections.
- Enforce security, authentication, role requirements, etc.
- And many other things...

All these, while minimizing code repetition.

```Python
async def common_parameters(q: str | None = None, skip: int = 0, limit: int = 100):
    return {"q": q, "skip": skip, "limit": limit}


@app.get("/items/")
async def read_items(commons: Annotated[dict, Depends(common_parameters)]):
    return commons
```

Whenever a new request arrives, **FastAPI** will take care of:
- Calling your dependency ("dependable") function with the correct parameters.
- Get the result from your function.
- Assign that result to the parameter in your _path operation function_.

!! ==Dependencies can be async and not==

!! You never call those functions directly. They are called by your framework (in this case, **FastAPI**).

You can define dependencies that in turn can define dependencies themselves.

In the end, a hierarchical tree of dependencies is built, and the **Dependency Injection** system takes care of solving all these dependencies for you (and their sub-dependencies) and providing (injecting) the results at each step.

For example, let's say you have 4 API endpoints (_path operations_):

- `/items/public/`
- `/items/private/`
- `/users/{user_id}/activate`
- `/items/pro/`

then you could add different permission requirements for each of them just with dependencies and sub-dependencies:
![[Pasted image 20230930142941.png]]
#### Share `Annotated` dependencies

```Python
CommonsDep = Annotated[dict, Depends(common_parameters)]

@app.get("/items/")
async def read_items(commons: CommonsDep):
    return commons

@app.get("/users/")
async def read_users(commons: CommonsDep):
    return commons
```

## Classes as Dependencies

