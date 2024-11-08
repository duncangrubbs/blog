---
layout: post
title: "Modularizing a Javascript API Interface"
date: 2020-10-01 22:47:19 -0400
categories: engineering
---

Disclaimer: this is not aimed at the expert web developer, I wouldn't even describe myself as one. Many of the ideas I discuss are common practice. This is just a discussion of ideas I found helpful in my own experience building web applications in React.

After building a lot of frontends in React, I got sick of re-writing the same code for interfacing with APIs. I would have so many different components making API calls, and in each one I would have a fetch statement in which I would have to set headers and options, do JSON parsing, and handle errors. Avoiding duplicate code is a common programming best pratice, but it is too often ignored in the web development space. Javascript has come a long way even in the last few years, and there are now so many ways to modularize.

With this is mind, I went googling around to see what was out there, and there were some good solutions. The key in most of them was true abstraction. There are some things you only really need to do once. If we abstract these away into their own classes, you can write less code, it will be more predictable and testable, and if you need to change something, you only have to change it in one place. The way I went about making these abstractions was to make a few files. I also opted to write everything in vanilla JS with zero dependencies so I could easily reuse the code on any future project. The files are:

1. API
1. ErrorService
1. Error
1. AuthService
1. Storage
1. constants

The first file, API, will act as a fetch service. This can be thought of the orchestrator, all API requests will pass through this. The second file is an ErrorService. This will be used by the fetch service to handle errors if they arise. The third file is an AuthService. This can handle auth specific tasks such as logging a user in, verifying they are logged in, and updating locally stored auth tokens. To follow the principles of dependency injection, I also made a Storage file that abstracts away the persistant storage library, whether it is localStorage, Redux, or something else. This way you only have to update the Storage class if you change how you persistantly store things, instead of a huge refactoring job. Finally, I added file to store some widely used constants, like the base API url, the version of the API you are using, and an API key if applicable.

In the API file I declare a fetch function that will override the default fetch API, this way I only have to declare my headers once, standardizing all of the requests I am making. Of course if you need the option of changing them, you can offer an optional method parameter to set them. Then, I declared functions for GET, POST, PUT, and DELETE requests. These can simply take a url and return parsed JSON, taking the work out of the calling component. This also helps if the backend API structure changes, since parsing is done is just one place.

In AuthService, I declare public functions for logging a user in and out. These utilize the other services to make API requests, but they validate and store the tokens themselves. I also wrote functions for retrieving the auth token from storage and parsing it to return a permission level, etc. Again, to follow the principles of dependency injection, the library to parse the token is injected, not hardcoded.

In the API class, my overridden fetch method checks the response code and if it is bad, hands the response over to the ErrorService. From here, I parse the error according to the REST API's format and even return an htmlelement that nicely displays the error. In the component that made the original request, it would be as simple as adding a catch statement that renders the error element object that is returned in the place of your choice. Again, taking work out of the components.

One small example of the usefulness of this idea in practice happened while I was working on BarterOut. I created a setup similar to the one I described above, with the hope of saving some time in the future, and it paid off. At some point, we changed the format of the Authorization header our backend API accepted from Token user_token to Bearer user_token. Because we were using a fetch service, this was a one line fix on the frontend. We could just change where the headers we set in our API file and we were done.

To conclude, this is a very basic setup, and in real large-scale production settings there would be many more services similar to the ones I described above. You can really take the idea of an API interface as far as you want. That being said, for small projects, you don't need a crazy complex architecture, and something this simple can suffice. Because these classes were so useful to me, I made a boilerplate called ozzy, and you can find it on Github here. Since it is a boilerplate and not a library, it is meant to be completely configurable. You can find more details on what each file does, function documentation, tests, and the source code in Github. Plus it's just a few hundred lines of code!
