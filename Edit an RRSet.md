# Edit an RRSet

Editing the RRset isn't a lot harder than creating one. There are 2 ways of doing it, one which Replaces all the editable data, and one which only replaces the data you want to have replaced (either the list of records or the TTL)
For simplicity's sake, we are going to replace everything

These are the headers curl sends from my PHP application

```HTTP
PUT https://desec.io/api/v1/domains/example.com/rrsets/.../AAAA

Host: desec.io
Accept: */*
Authorization: Token 0123456789abcdef
Content-Type: application/json
Content-Length: xy

{"records":["::1"],"ttl":62}
```

so Let's break this down a bit. 

## URL

The URL in this case is a special. It consists mainly of 3 parts that need to be changed depending on what you do.
It can be summarized as:
`https://desec.io/api/v1/domains/DOMAIN/rrsets/SUB.../TYPE/`

the first part is is DOMAIN, which obviously is the domain, in this case example.com.
the last type is obviously the type of the RRSet you work with. Remind the / at the end.

The second part needs a bit of care.

If you are working at the domain itself, aka the Apex of the domain, you just keep `...`

If you use a subdomain it is the subname with `...` following it.

The Reasoning for this is that `//` `/./` and and `/../` would all cause chaos on webservers, and they didnt have any better idea of what do do for the apex.

## Body

the body is one single JSON containing the following things:

The `ttl`, which is a positive integer, in this case `62`.

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

Records that involve a domain, like CNAME or MX, require a period at the end, e.g. for an MX
`["10 mx.example.com."]`

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

The HTTP Method in this case is `PUT`.
