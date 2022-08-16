# Request Methods

## Definitions

Each HTTP method is defined within the [REST API resource model](resources.md):

`GET`
:   retrieve a representation of a resource's state

`HEAD`
:   retrieve the metadata associated with the resource's state

`PUT`
:   add a new resource to a store or update a resource

`DELETE`
:   remove a resource from its parent

`POST`
:   create a new resource within a collection and execute controllers


## Rules

### Don't use `GET` and `POST` instead of other request methods

!!! success "Do..."
    - use HTTP methods as specified in this section 

!!! failure "Don't..."
    - change message intent and undermine the protocol's transparency
    - compromise REST API design by misusing HTTP request methods because clients have limited HTTP vocabulary


### Use `GET` to retrieve a representation of a resource

!!! success "Do..."
    - _(optional)_ include headers in the request
    - ensure idempotency i.e., repeatability without side effects (especially for caching)
    - include a body in the response

!!! failure "Don't..."
    - include a body in the request


### Use `HEAD` to retrieve response headers without a body

Use `HEAD` to check if a resource exists or to read its metadata

!!! success "Do..." 
    - _(optional)_ include headers in the request
    - ensure idempotency i.e., request repeatability without side effects (especially for caching)

!!! failure "Don't..."
    - include a body in the request
    - include a body in the response

### Use `PUT` to both insert and update a stored resource

Use `PUT` to:
- add a new resource to a store, with a URI specified by the client.
- update or replace an already stored resource.

!!! success "Do..."
    - include a body in the request

!!! example
    A service-oriented REST API provides a store resource that allows its clients to persist their data as objects:
    ```
    PUT /users/1234/files/4b099410-0ed9-4719-b3e6-3a847bc3fade
    ```

!!! warning
    The `PUT` request body may be different from the one received from a subsequent `GET` request because the resource might be partially immutable.

    See [Stores must support conditional PUT requests](headers.md#stores-must-support-conditional-put-requests) for how to use headers to handle overloading `PUT` to both insert and update resources. 

### Use `PUT` to update mutable resources

!!! success "Do..."
    - _(optional)_ include a body in the request with the desired changes

### Use `POST` to create a new resource in a collection

!!! example
    A client uses `POST` to request a new addition to a server-owned collection:
    ```
    POST /cities/new-york/museums/moma/exhibits
    ```

!!! success "Do..."
    - _(optional)_ include a body in the request with the suggested initial state of the resource

### Use `POST` to execute controllers

HTTP defines `POST` as unsafe and non-idempotent, which means that it may have unwanted side effects if repeated, similar to submitting a web form on an unresponsive site twice for one payment can lead to being charged twice.

Using `POST` for controller resources is the natural choice, as gaining extra flexibility is worth losing some transparency and robustness.

!!! success "Do..."
    - _(optional)_ include headers in the request
    - _(optional)_ include a body in the request

!!! success "Don't..."
    Use `POST` to get, store, or delete resources, as specific HTTP methods for those actions already exist.

!!! example
    A controller executed using `POST` to restart a router in the network:
    ```
    POST /routers/1234/restart
    ```

### Use DELETE to remove a resource from its parent collection or store

Clients must be unable to find a resource after a `DELETE` request has been processed for it.
Subsequent `GET` or `HEAD` requests on the resource must result in a `404` (`Not Found`) response.

!!! example
    A user removes a document from a store:
    ```
    DELETE /users/1234/recipes/lasagna
    ```

!!! failure "Don't..."
    - overload the role of `DELETE` by mapping it to a lesser action that leaves the resource, and its URI, available to clients

!!! success "Do..."
    - implement "soft" delete or other state-changing interaction via a controller resource and ask clients to use `POST` instead of `DELETE` 

### Use OPTIONS to retrieve available interactions for a resource

!!! example
    A user obtains resource metadata including the `Allow` header value via the OPTIONS request method:
    ```
    Allow: GET, PUT, DELETE
    ```
