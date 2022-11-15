Cookies are an essential part of developing backends. They are useful for accessing e.g. the user on the server-side, from which Server-Side-Rendering profits from.

Doing it in Spring is as easy as getting the request, and then using the following code.

```java
public ... yourEndpoint(HttpServletResponse response, ...) {
```
```Java
var cookie = new Cookie(cookieName, cookieValue);

// Cookie Settings
cookie.setHttpOnly(true);
cookie.setSecure(true);
cookie.setMaxAge(1200);
cookie.setPath("/");
...

// Add it to the response
response.addCookie(cookie);
```