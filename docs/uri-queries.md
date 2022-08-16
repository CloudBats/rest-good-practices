# URI Queries

## Introduction

RFC 3986 states that a URI's optional query comes after the path and before the optional fragment:

```
URI = scheme "://" authority "/" path [ "?" query ] [ "#" fragment ]
```

As a component of a URI, the query contributes to the unique identification of a resource.

!!! example
    The URI of a controller resource that sends an SMS message with a text value of `hello`:
    ```
    http://api.college.tld.com/students/morgan/send-sms
    http://api.college.tld.com/students/morgan/send-sms?text=hello
    ```
    The query component contains a set of parameters to be interpreted as a variation of the resource.
    These two resources are not the same, but are very closely related.

The query component can provide clients with additional interaction capabilities such as ad hoc searching and filtering.
Thus, unlike the other elements of a URI, the query part may be transparent to a REST API's client.

The entirety of a resource's URI should be treated opaquely by basic network-based intermediaries such as HTTP caches.

## Caches

Caches must not vary their behavior based on the presence or absence of a query in a given URI.

!!! success "Do..."
    - use headers to direct a cache intermediary's behavior

!!! failure "Don't..."
    - use queries to direct a cache intermediary's behavior
    - exclude response messages from caches based solely upon the presence of a query in the requested URI

## Rules

### Use the query component of a URI to filter collections or stores

A URI's query component is a natural fit for supplying search criteria to a collection or store.

!!! example
    The response would contain a listing of all the cities in the collection:
    ```
    GET /cities
    ```
    The response would contain a filtered list of all the cities in the collection with a `Nevada` value for `state`:
    ```
    GET /cities?state=Nevada
    ```

### Use either the query component of a URI or a controller to paginate collection or store results

Use the query component to paginate collection and store results with the following parameters:
- pageSize: maximum number of contained elements desired in the response
- pageStartIndex: the zero-based index of the first element desired in the response

!!! example
    ```
    GET /users?pageSize=50&pageStartIndex=200
    ```

For complex client pagination or filtering requirements, which exceeds the simple formatting capabilities of the query part, design a controller resource associated with a collection or store.
This allows for custom range types and special sort orders to be easily specified in the client request message body.
Be careful to ensure that the controller's cacheable results are marked accordingly.

!!! example
    This controller may accept more complex inputs via a request's entity body instead of the URI's query part:
    ```
    POST /cities/search
    ```
