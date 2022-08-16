# URI Paths

As a general rule, any mentions of noun or verb also implies noun phrases and verb phrases.

## Rules

### Use a singular noun for document name path segments

!!! success
    The URI for a single animal document uses a singular noun:
    ```
    http://api.subdomain.tld.com/continents/africa/animals/elephant 
    ```

### Use a plural noun for collection name path segments

A collection's name should be chosen to reflect what it uniformly contains.

!!! success
    A collection uses a plural noun for contained resources:
    ```
    http://api.subdomain.tld.com/cities/new-york/museums/moma/exhibits 
    ```

### Use a plural noun for store name path segments

!!! success
    The URI for a store of music playlists may use the plural noun form as follows:
    ```
    http://api.subdomain.tld.com/artists/mikemassedotcom/playlists 
    ```

### Use a verb for controller name path segments

Like computer program functions, the URI name for a controller resource should indicate its action.

!!! success
    ```
    http://api.subdomain.tld.com/users/john-smith/activate
    http://api.subdomain.tld.com/dbs/postgres/reindex
    http://api.subdomain.tld.com/routers/1234/restart 
    ```

### Variable path segments may be substituted with identity-based values

URI path segments can be:
- static: fixed names that may be chosen by the REST API author
- dynamic: automatically inserted identifier that ensures URI uniqueness

The REST API or its clients must substitute variables in the URI template with a numeric or alphanumeric identifier before resolution.

!!! success
    This template has three variables (`cityId`, `museumId`, `exhibitId`):
    ```
    http://api.subdomain.tld.com/cities/{cityId}/museums/{museumId}/exhibits/{exhibitId}
    ```
    and becomes, after substitution:
    ```
    http://api.subdomain.tld.com/cities/new-york/museums/moma/exhibits/may_2020
    ```

REST API clients must use URIs as the only relevant resource identifiers.
With URIs as the only IDs, the REST API backend implementation is free to evolve without impact on existing clients.
If backend system identifiers, such as database IDs, appear in URI paths, they must be irrelevant to client code.

### Avoid CRUD function names in URIs

!!! failure "Don't..."
    - use URIs to indicate that a CRUD function is performed
    ```
    GET /deleteUser?id=1234
    GET /deleteUser/1234
    DELETE /deleteUser/1234
    POST /users/1234/delete
    ```

!!! success "Do..."
    - use URIs to uniquely identify resources
    - named resources as described in the rules above
    - use HTTP request methods to indicate which CRUD function is performed
    ```
    DELETE /users/1234
    ```
