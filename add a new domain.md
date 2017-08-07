# How to add a new domain

This is probably the first step you have to do (unless you are hacking on your dedyn.io-domain) when working with your DESEC Account.

## Domain Preperation

> it's not really important whether you do the API Command first or prepare the domain.

You need to set the nameservers to 
  * ns1.desec.io
  * ns2.desec.io

also remove other DS Records from your Registrar, otherwise you cant look up anything until you added the new Keys, which you will get after you created the domain with the API.

## API Command

First we start with the complete HTTP Request:

```HTTP
POST https://desec.io/api/v1/domains/

Host: desec.io
Accept: */*
Authorization: Token 0123456789abcdef
Content-Type: application/json
Content-Length: xy

{"name":"example.com"}
```

so Let's break this down a bit. 


## Body

the body is one single JSON containing just one thing:

The `name`, which is a normal string, in this case "`example.com`"

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
