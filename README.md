# Cookie-Attributes

## Information

Cookies is small piece of information stored on the client side, which are sent to the server when every request send by the client. 

* **Cookies are primarily used for:**
    - Sessions Maintaining (Authentication, Logins)
    - Personalization (User preferences, Themes) 
    - Tracking (Store and Analyzing user behavior data)

## Cookie Attributes     

Cookies can be secured by properly setting using cookie attributes

### Secure Attributes
The Secure attribute tells the browser to only send the cookie if the request is being sent over a secure channel such as HTTPS.

Mainly Attacker can steal information like data/cookie using sniffing Traffic. so using *Secure* Attributes in cookie will ensure there safety.  

Example: `Set-Cookie: key=123; Secure;`

### HTTPOnly Attributes
The cookie cannot be read by client-side script only sent to the server.  

It Means Java Script are not allowed to access the Cookie

Example: `Set-Cookie: key=123; HTTPOnly`

### Domain and Path Attributes
The Domain and Path directives define the scope of the cookie: what URLs the cookies should be sent to.

**Domain** specifies allowed hosts to receive the cookie for domain or its subdomains.
> If Domain attributes unspecified, it defaults to the hostname of the current document location, excluding subdomains.

Example: `Domain=github.com`

**Path** attribute signifies the URL or path for which the cookie is valid. 
> Default path attribute is set as '/'

Example: `Path=/admin`


### Expires or Max-Age Attributes
This attribute is used to set persistent cookies. It signifies how long the browser should use the persistent cookie and when the cookie should be deleted.

Example: `Set-Cookie: Expires=Mon, 22 Jan 2021 03:30:00 GMT;`


### SameSite Attributes
SameSite cookies shouldn't be sent with cross-site requests. data associated with this cookie will only be sent on requests matching the current site shown on the browser URL bar.

> SameSite attribute protection against CSRF.

This attribute can be configured in three different value:
1. Strict: The browser will only send cookies for same-site requests
2. Lax: Same-site cookies are refuse to give on cross-site subrequests, such as calls to load images or frames, but will be sent when a user navigates to the URL from an external site; for example, by following a link.
3. None: The browser will send cookies with both cross-site requests and same-site requests

Example: `Set-Cookie: key=123; SameSite=Strict`

### Cookie prefixes
By design cookies do not have the capabilities to guarantee the integrity and confidentiality of the information stored in them. Those limitations make it impossible for a server to have confidence about how a given cookie’s attributes were set at creation. In order to give the servers such features in a backwards-compatible way, the industry has introduced the concept of Cookie Name Prefixes to facilitate passing such details embedded as part of the cookie name.

* **Host Prefix**

The `__Host-` prefix expects cookies to fulfill the following conditions:

1. The cookie must be set with the Secure attribute.
2. The cookie must be set from a URI considered secure by the user agent.
3. Sent only to the host who set the cookie and MUST NOT include any Domain attribute.
4. The cookie must be set with the Pathattribute with a value of / so it would be sent to every request to the host.

Example: Accepted 
- [x] `Set-Cookie: __Host-SID=12345; Secure; Path=/` 

Example: Rejected: 
- [ ] `Set-Cookie: __Host-SID=12345 Set-Cookie: __Host-SID=12345; Secure Set-Cookie: __Host-SID=12345; Domain=site.example Set-Cookie: __Host-SID=12345; Domain=site.example; Path=/ Set-Cookie: __Host-SID=12345; Secure; Domain=site.example; Path=/`

* **Secure Prefix**

The `__Secure-` prefix is less restrictive and can be introduced by adding the case-sensitive string *__Secure-* to the cookie name. 

Any cookie that matches the prefix **__Secure-** would be expected to fulfill the following conditions:

1. The cookie must be set with the Secure attribute.
2. The cookie must be set from a URI considered secure by the user agent.

Example: `Set-Cookie: __Secure-ID=123; Secure; Domain=github.com`


## Best Practice
Most secure cookie attribute configuration
```javascript
Set-Cookie: __Host-SID=123; path=/; Secure; HttpOnly; SameSite=Strict
```


## Refrence

- https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies

- https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie
