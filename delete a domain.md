# delete a domain

Creating the RRset is simple enough, these are the headers curl sends from my PHP application

```HTTP
DELETE https://desec.io/api/v1/domains/example.com/

Host: desec.io
Accept: */*
Authorization: Token 0123456789abcdef
Content-Length: xy


```

so Let's break this down a bit. 

## Body

the Request body is empty.

## Headers

### Automatic Headers

For starters we have the headers that in most cases your HTTP client should take care of (or don't matter to us), 
therefore eliminating the need to set them yourself:

```HTTP
Host: desec.io
Accept: */*
Content-Length: xy
```

### Manual Headers

now we have the actually crucial headers that we really need to set:

```HTTP
Authorization: Token 0123456789abcdef
```

`0123456789abcdef` is obviously whatever your token is, 

### HTTP Method

The HTTP Method in this case is `DELETE`.

# Response

`204 No Content` no matter whether you actually deleted something or there was nothing to delete, or you couldn't delete it because it is not yours.
