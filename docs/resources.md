# Resources

## Introduction

!!! important
    You use resource modeling for REST API design just as you use a relational DB schema for a data model or a class hierarchy for OOP. Design first, then write URI paths.

Each path segment in a URI belongs to a unique resource in the REST API resource model. This URI:

```
http://api.subdomain.tld.com/cities/new-york/museums/moma
```

implies that each of these URIs point to a resource:

```
http://api.subdomain.tld.com
http://api.subdomain.tld.com/cities
http://api.subdomain.tld.com/cities/new-york
http://api.subdomain.tld.com/cities/new-york/museums
```

## Document

!!! note "Definition"
    A singular concept like an object instance or database record.

A document's state representation typically includes both fields with values and links to other related resources.

The document type is the conceptual base archetype of the other three other resource archetypes, which can be viewed as specializations of the document archetype.

A document may have child resources that represent its specific subordinate concepts.

!!! example
    A user retrieves information about New York, the New York Museum of Modern Art, and the May 2020 exhibit therein:
    ```
    GET http://api.subdomain.tld.com/cities/new-york
    GET http://api.subdomain.tld.com/cities/new-york/museums/moma
    GET http://api.subdomain.tld.com/cities/new-york/museums/moma/exhibits/may-2020
    ```

Use a document as the REST API root resource, a.k.a. the `docroot`.

!!! success
    The `docroot` as the REST API entry point:
    ```
    http://api.subdomain.tld.com
    ```

## Collection

!!! note "Definition"
    A server-managed directory of resources.

A collection resource decides what resources to contain and their URIs.

Clients may propose adding resources to a collection, but the collection decides to create a new resource, or not.

!!! example
    A user retrieves lists of cities, New York museums, and exhibits in the New York Museum of Modern Art:
    ```
    GET http://api.subdomain.tld.com/cities
    GET http://api.subdomain.tld.com/cities/new-york/museums
    GET http://api.subdomain.tld.com/cities/new-york/museums/moma/exhibits
    ```

## Store

!!! note "Definition"
    A client-managed resource repository.

A store resource relies on the API client to add, retrieve or delete resources.

Stores do not create resources or URIs by themselves. The client chooses the URI when putting the resource into the store.

!!! example
    A user with ID 1234 inserts a document resource named `moma` (the Museum of Modern Art) in the store of visited museums in New York: 
    ```
    PUT user/1234/cities/new-york/museums/moma
    ```

## Controller

!!! note "Definition"
    A resource that models a procedural concept.

Controller resources are like executable functions, with parameters and return values, inputs and outputs.

Like a HTML forms in a web app, a REST API uses controllers for actions that don't map to standard CRUD methods (create, retrieve, update, and delete).

!!! success "Do..."
    - keep the controller name last in the URI path, without child resources

!!! example
    Restart a router in the network:
    ```
    POST /routers/1234/restart
    ```
