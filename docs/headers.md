## Headers

!!! todo
    add content

### Stores must support conditional PUT requests

A store resource uses the PUT method for both insert and update, which means it is difficult for a REST API to know the true intent of a client's PUT request.

Through headers, HTTP provides the necessary support to help an API resolve any potential ambiguity.

A REST API must rely on the client to include the If-Unmodified-Since and/ or If-Match request headers to express their intent.

The If-Unmodified-Since request header asks the API to proceed with the operation if, and only if, the resource's state representation hasn't changed since the time indicated by the header's supplied timestamp value.

The If-Match header's value is an entity tag, which the client remembers from an earlier response's ETag header value.

The If-Match header makes the request conditional, based upon an exact match of the header's supplied entity tag value and the representational state's current entity tag value, as stored or computed by the REST API. The following example illustrates how a REST API can support conditional PUT requests using these two headers.

Two client programs, client#1 and client#2, use a REST API's /objects store resource to share some information between them.

Client#1 sends a PUT request in order to store some new data that it identifies with a URI path of /objects/2113.

This is a new URI that the REST API has never seen before, meaning that it does not map to any previously stored resource.

Therefore, the REST API interprets the request as an insert and creates a new resource based on the client's provided state representation and then it returns a 201 (“Created”) response.

Some time later, client#2 decides to share some data and it requests the exact same storage URI (/objects/2113).

Now the REST API is able to map this URI to an existing resource, which makes it unclear about the client request's intent.

The REST API has not been given enough information to decide whether or not it should overwrite client#1's stored resource state with the new data from client#2.

In this scenario, the API is forced to return a 409 (“Conflict”) response to client#2's request.

The API should also provide some additional information about the error in the response's body.

If client#2 decides to update the stored data, it may retry its request to include the IfMatch header.

However, if the supplied header value does not match the current entity tag value, the REST API must return error code 412 (“Precondition Failed”).

If the supplied condition does match, the REST API must update the stored resource's state, and return a 200 (“OK”) or 204 (“No Content”) response.

If the response does include an updated representation of the resource's state, the API must include values for the Last-Modified and ETag headers that reflect the update.

HTTP supports conditional requests with the GET, POST, and DELETE methods in the same fashion that is illustrated by the example above.

This pattern is the key that allows writable REST APIs to support collaboration between their clients. 

### Location must be used to specify the URI of a newly created resource

The `Location` response header's value is a URI that identifies a resource that may be of interest to the client.

In response to the successful creation of a resource within a collection or store, a REST API must include the Location header to designate the URI of the newly created resource.

In a `202` (`Accepted`) response, this header may be used to direct clients to the operational status of an asynchronous controller resource.
