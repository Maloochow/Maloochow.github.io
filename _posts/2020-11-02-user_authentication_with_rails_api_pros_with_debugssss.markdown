---
layout: post
title:      "User Authentication with Rails API (pros with debugssss)"
date:       2020-11-02 06:42:43 +0000
permalink:  user_authentication_with_rails_api_pros_with_debugssss
---

You have a web app and want to implement an user authentication feature, how are you going to check if the user is logged in every time the browser is refreshed? You have two concerns, (1) how to fetch the user's information securely and (2) how to store it in the browser with the least security concern.

For (1), there are a lot of options. You can use Auth0 to create a secured API or authenticate with 'bcrypt' with whatever server you are using (can be Rails API or node.js as part of your frontend server).

If you decided to go with Rails API + 'bcrypt' combo, [this article](https://medium.com/swlh/react-reactions-cfdde7f08dff) talks about how to set it up. It is absolutely fine to create the Rails app **without** the '--api' flag. Because in this case, api only app will need to add extra configurations to establish the connection with browser cookies.

For (2), a common pratice is to store the user info in localStorage, whether it is the token, or user_id. The drawback is, it is javascript accessible. [This article](https://pragmaticstudio.com/tutorials/rails-session-cookies-for-api-authentication) has a great quote of the drawbacks of this method:
> That means if an attacker can inject some JavaScript code that runs on the web app’s domain, they can steal all the data in localStorage. The same is true for any third-party JavaScript libraries used by the web app. Indeed, any sensitive data stored in localStorage can be compromised by JavaScript. In particular, if an attacker is able to snag an API token, then they can access the API masquerading as an authenticated user.
> 

The rest of the section is going to be highlights from this article. If you would like, please read the article instead and jump to the next section.

Rails session storage provides a great solution to it (This is only viable if the API client is a browser).
When a user is authenticated (through bcrypt or any OAuth platforms), you can store the user_id in the session (or JWT token if you prefer)
```
session[:user_id]=@user.id
```
This allows the response to the fetch call from your web app to include a Set-Cookie HTTP header. "The cookie (`_session_id`) has the HttpOnly flag set which means it can’t be accessed by JavaScript, thus it’s not vulnerable to XSS attacks. As well, the cookie value is encrypted."
Now that seems a lot more secure than storing in localStorage. The article has detailed implementations. Please read this in conjunction with the first article to do your setup.

***Debug Time***

Now it's my favourite make-all-mistakes-possible time.

**First,** Rails's default session store has a weakness in [persisting all the session cookies in the browser (at least for version 2.0 to 4.0](https://github.com/heartcombo/devise/issues/3031). It seems like a better idea to use ActiveRecordStore as the session store instead. [The implementation is pretty easy](https://github.com/rails/activerecord-session_store) and the login process is the same as before: `session[:user_id]=@user.id`.

**Second**, Even after I set up my Rails API exactly as the first article instructed, I encountered a problem where after I logout, my session in Rails shows it's already cleared and my web app received a logged out: true statement, but after I refresh my page to check if the visitor is logged in, the session somehow still contains the [:user_id] I created before log out.
This shows there is a discommunication between the session in my Rails and the cookies in my browser. Remember, the session cookie in the browser cannot be accessed through javascript, so the only place to solve it is in the Rails app. The solution to this is in the second article:
> By default, Rails embeds a unique CSRF token in the rendered HTML: both in a meta tag and as a hidden field in each form. Here’s an example meta tag:

<meta name="csrf-token" content="z/ra9NM9bQCCZM8LI4Yx5nlojtpp58Z7VLPLR32sqPrVvsx2ckZMSAbFD6M4FiGxzdItINFBuhBkVcywD/oMmg==" />
Conveniently, that same CSRF token is stored in the session cookie.

Then whenever a user makes a non-GET request, the CSRF token gets sent along with the request. And, by default, a Rails ApplicationController includes this line:

protect_from_forgery :exception
This adds a before_action that compares the CSRF token in the request with the token in the session cookie to ensure they match. Otherwise, an exception is raised.
> 
The way to solve it is to read the cookie and set the header for every fetch request.
```
axios
  .get("/my-stuff", {
    headers: { "X-CSRF-Token": getCookie("CSRF-TOKEN") }
  })
  .then(response => {
    const myStuff = response.data;
    // render my stuff
  });
```
If you are using axios as the first article suggested, you can do it by adding the follwing code to the file where you do the fetch:
```
axios.defaults.xsrfCookieName = "CSRF-TOKEN";

axios.defaults.xsrfHeaderName = "X-CSRF-Token";

axios.defaults.withCredentials = true;
```
This will allow the post request, such as logout request be communicted between the browser cookies and the rails session.
