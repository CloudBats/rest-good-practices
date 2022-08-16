# URI Format

## The RFC 3986 definition

```
URI = scheme "://" authority "/" path [ "?" query ] [ "#" fragment ]
```

## Rules

### Form hierarchies with slash `/` as a separator, but don't end with a slash `/` 

!!! success
    ```
    http://api.subdomain.tld.com/cities/new-york/museums/moma
    ```

!!! failure
    ```
    http://api.subdomain.tld.com/cities/new-york/museums/moma/
    ```

### Use hyphens `-` to separate words, don't use underscores `_`

!!! success
    ```
    http://api.subdomain.tld.com/cities/new-york/museums/moma/exhibits/may-2020
    ```

!!! failure
    ```
    http://api.subdomain.tld.com/cities/newyork/museums/moma/exhibits/may2020
    http://api.subdomain.tld.com/cities/new_york/museums/moma/exhibits/may_2020
    ```

### Use lowercase letters

!!! success
    ```
    http://api.subdomain.tld.com/cities/new-york/museums/moma/exhibits/may-2020
    ```

!!! failure
    ```
    http://api.subdomain.tld.com/CITIES/NewYork/museums/moma/exhibits/May2020
    ```

### Avoid file extensions

Don't use file extensions to specify format, use the `Accept` request header instead.
The REST API is not a file system, and URIs are not file system paths.

!!! success
    ```
    http://api.subdomain.tld.com/cities/new-york/museums
    ```

!!! failure
    ```
    http://api.subdomain.tld.com/cities/new-york/museums.json
    ```

### Use consistent subdomain names

The top-level domain and first subdomain names (e.g., subdomain.tld.com) of an API should identify its service owner.

#### Use an `api` subdomain in the full domain name of your API

!!! example
    ```
    http://api.subdomain.tld.com
    ```

#### Use a `developer` subdomain in the full domain name of your developer portal

!!! example
    ```
    http://developer.subdomain.tld.com
    ```
