# Create an RRSet

Creating the RRset is simple enough, these are the headers curl sends from my PHP application

```HTTP
POST https://desec.io/api/v1/domains/example.com/rrsets/

Host: desec.io
Accept: */*
Authorization: Token 0123456789abcdef
Content-Type: application/json
Content-Length: xy

{"type":"AAAA","records":["::1"],"ttl":60}
```

so Let's break this down a bit. 

## Body

the body is one single JSON containing the following things:

The `type`, which is a normal string, in this case "`AAAA`"

The `ttl`, which is a positive integer, in this case `60`.

and then you have the `recods`, which is an array that just contains your Records:

```json
["::1"]
```
multiple entries look like this:

```json
["::1","::2"]
```

#### Attention

in case of string-based records like TXT, you need an ADDITIONAL set of quotes inside, 
a PHP `var_dump()` shows this:

```
array(2) {
  [0]=>
  string(13) ""test""
  [1]=>
  string(14) ""random stuff""
}
```
as you can see you have an array of 2 strings with extra quotes inside, which are "`"test"`" and "`"random stuff"`".

The Quotes outside the code highlight are the string indicators and the quotes inside art actual part of the string.

In JSON the array will have escaped quotes making it look like this:

```JSON
["\"test\"","\"random stuff\""]
```
## Headers

### Automatic Headers

For starters we have the headers that in most cases your HTTP client should take care of, 
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
Content-Type: application/json
```

`0123456789abcdef` is obviously whatever your token is, 
and the Content Type needs to be set to json because otherwise you wont get any Luck.

### HTTP Method

The HTTP Method in this case is `POST`.
