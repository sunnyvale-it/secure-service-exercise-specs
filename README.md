# Secure Service exercise specifications

![](img/1.jpg)

## Introduction

In this exercise you are required to write an HTTP client to invoke a server API, authenticating with it using "HTTP Signature" authorization mechanism.

Target of this exercise is to replicate how clients authenticates with servers using asymmetric cryptography and RSA keys.

## What is provided to you

For this exercise you  are given:

- A working, publicly available, server API and its URL (communicated once enrolled to the exercise)
- The server public RSA key (click to [server_public_key.pem](server_public_key.pem) download it)
- A pre-authenticated key-id, which is `client1`
- The client public RSA key (click to [client_public_key.pem](client_public_key.pem) download it)
- The client private RSA key (click to [client_private_key.pem](client_private_key.pem) download it)
- An "attempt code" you will pass in the request body (assigned once enrolled to the exercise)
- Server specifications
- Client requirements, constraints, hints, nice to have
- A procedure to submit your work

## Server specifications

- The server implements HTTP POST method only and accepts request with `Content-Type`equals to `application/json`
- The server verifies the content of the `Digest` HTTP request header
- The server authenticates clients looking at the content of the `Authorization` header, where it expects to find the `Signature` type, followed by `keyId`, `signature`, `algorithm` and `headers` parameters
- The server verifies the use of a valid `keyId`
- The server verifies the client `signature`
- The server makes sure that for signing, the `rsa-sha256` algorithm is used
- In turns, the server replies to authenticated clients adding its `Signature` response header (to allow the clients to trust its response)

## Client requirements / constraints

- To write the client choose your preferred programming/scripting language, unless otherwise communicated
- Add the HTTP `Digest` request header, so as it contains a value calculated using the following pseudo formula: `"SHA-256="+b64encode(sha256(request body))`
- The client-side signature must be calculated using the following pseudo formula: `b64encode(sign(digest header content)`
- The algorithm to use for calculating the digest must be `sha256`
- The algorithm to use for signing must be `rsa-sha256` and `PKCS1v15` for padding
- Specify a request body as the following `{'code': 'My attempt code', 'author': 'Name Surname'}`, were in the `code` property you must insert your "attempt code" whilst in the `author` property just specify your name and surname.

## Hints

A working client request is similar to this

```
POST / HTTP/1.1
Host: x.y.z.q
Content-Type: application/json
Digest: SHA-256=4evwMDj9wJr9iwg5qOM2hp52bT/tgsPzEcXVZ/74sz8=
Authorization: Signature keyId="client1",algorithm="rsa-sha256",signature="HLv6dbrSxORYbGpDr9tYXVEQ58kkhMs5Q0I1Kpb7klkhsaLAh2l70aEKyD604RQviPimN9p/AA5FZQFL/vAquUe7LFHQLets/2ULuzHWqLzb9K1HHvL+UUshN4lr1bSYxmsYImQqS/J6Hr3NZ2ue1LowLnHZQCTrRJUuNlbRDUO9Ooi6n+lcRP5nx/UEwba4sYoAAi8Ftf9CDJZ4L2d4mlxZnWv2u6Asv45upnj0oKzntFmJR4wTLoZDETMoEXtHQA9D1NfTNF9YEYu52juK7hL7AjiHfDKNm3jDx1tQNizS6T60TchBA4O0x8cb2OkCKYp1sHZJbSZ9SQdBsx79iw==",headers="digest"

{'code': '12345', 'author': 'Denis Maggiorotto'}
```

An example of a successful server response is similar to this

```
HTTP/1.1 200 OK
Date: Mon, 18 Nov 2024 13:43:41 GMT
Content-Type: application/json
Digest: SHA-256=4evwMDj9wJr9iwg5qOM2hp52bT/tgsPzEcXVZ/74sz8=
Signature: keyId="server1",algorithm="rsa-sha256",signature="RY/fW5KH0UvigTwyADH8XrwWVYg7pCGDS2QT9C+CqCBXXxnS6XaK8uCJYVcg4/baH4Z7DsPTcfFqOOw2OGk/NFOMpu1Box/lNst+VrQSEiLzejKnmE2WXv9V25O/fy7TZU0nThLN3sL33y8yWR44oSgF4wi52YAiNoFHNtOY/oIC164Od5gvO4ruhSHBwk6IbB8sEeRIuHjta2sV9pk9jWPmSGeZXJRmaufe0qOMUeej/z6zePNs1vYbRS4TXCWc7FecioIiMzTTdD1ebkieXNpIGwIgqld1j8ghynr0TonOyGRuk6j1mD9e0Np2v6NF4zawdaDyjb4NJw3ijfYyJA==",headers="digest"

{'code': '12345', 'author': 'Denis Maggiorotto'}
```

If you get HTTP error 400, one of the following problem occurred: 
- Missing or empty `Digest` header
- The `Digest` header is valid but its content is different than the one expected by the server

If you get HTTP error 401, one of the following problems occurred: 
- Missing or invalid `Autorization` header
- The signature algorithm is not `rsa-sha256`
- The signature is invalid

If you get HTTP error 403, one of the following problem occurred: 
- The keyId is not authorized

## Nice to have

- Even if it is not mandatory, your client could validate the server signature (available in the Signature response header)
- Although not requested, you can deliver your client as a container image 
- It would be nice to receive unit tests along with your software artifact 

## How to submit your work

You  submit your work in different steps:

1) Send us, by email, a dump of your complete server response (HTTP return code + headers + body)
2) Upon our request, send us the client source code and all the instructions to run it on our side
3) Upon our request, join us for an interview

## How your work will be evaluated

You work will be evaluated against the following criteria. For each of them, a rating percentage will be assigned (0 = low rating, 100 = high rating)

| Criteria  | Description | 
|---|---|
| Requirements coverage  | The extent to which the requirements of this exercise has been addressed through your implementation  | 
| Supporting documentation  | How much and how well your solution has been documented. A good documentation allows the evaluator to install and run your artifact without (too much) guessing or troubleshooting    |  
| Functioning degree   | Is your artifact working? Is the functionality deterministic or intermittent? |  
| Code style   | Refers to a set of guidelines and conventions that dictate how code should be written, formatted, and organized to ensure consistency, readability, and maintainability. This criteria will evaluate the syntax, formatting, naming conventions, code structure, best practices, and commenting of your code |   
| Detected use of AI or plagiarism   | The identification if your content has been either totally generated by artificial intelligence and/or copied or heavily derived from existing work. Low rating means that is evident that the submitted solution has not been ingenierized by the candidate |   

The arithmetic mean of the ratings described here above will be your final score.

## References

Documentation on the HTTP Signature protocol can be found:

- on this draft: https://tools.ietf.org/html/draft-cavage-http-signatures-05
