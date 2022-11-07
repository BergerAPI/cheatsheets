Cookies are an essential part of developing backends. They are useful for accessing e.g. the user on the server-side, from which Server-Side-Rendering profits from.

Doing it in ASP.net is as easy as doing:
```C#
var cookieOptions = new CookieOptions()  
{  
    ...
};
```
One can now configure the cookies via the parameters of the class.
A possible cookie might look like this:

```C#
var cookieOptions = new CookieOptions()  
{  
    IsEssential = true,  
    Expires = DateTime.Now.AddDays(1),  
    Secure = true,  
    HttpOnly = true,  
    SameSite = SameSiteMode.None  
};
```

Now, one only need to append to cookie to the response, and return a status.

```C#
Response.Cookies.Append("cookie_name", cookie_value, cookieOptions);

return Ok();
```