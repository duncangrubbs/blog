---
layout: post
title: "A tiny date hydration middleware"
date: 2022-01-12 22:47:19 -0400
categories: engineering
---

[Flowlie](https://www.flowlie.com/)'s entire stack is written in Typescript (React and Express) and we use Mongo for our database. In many ways, having your frontend and backend written in the same language is really nice. We were able to define interfaces that were shared between codebases, essentially providing end-to-end type safety at build time. That being said, to take full advantage of the one-language stack it's useful to use a few tricks.

One issue that we were running into quite often is that when we were sending `Date` objects between our API and frontend they we getting serialized as strings in `JSON`. For example, on our frontend we would initialize a date when someone updated a fundraising round using `new Date()` and then store it in some object and hand it off to the API via `POST` request. However, when the API received the HTTP request it would see the string as something like `2022-01-12T23:54:34.574Z`. Suddenly, using the one-language stack didn't feel like magic anymore. We were needing to remember to find date fields and parse them manually for each API endpoint.

Luckily, Express.js has a super nice [middleware feature](https://expressjs.com/en/guide/using-middleware.html) that allows you to intercept requests and do something to them before passing them off to the next handler. [Here](https://gist.github.com/duncangrubbs/d5133117fce31559576dbec97bf7832f) is a super simple example of a middleware that solves this problem. It takes the request body data, looks for patterns that match a string serialization of a `Date` object and hydrates them back into `Date` objects.

This is great and all, but it's only half of the equation. What about sending dates _to_ the frontend _from_ the API; how could we handle that? You could use something like [axios interceptors](https://axios-http.com/docs/interceptors), or you could use [ozzzy](https://github.com/duncangrubbs/ozzzy) if you want a more stripped down, simple HTTP API interface. In fact, ozzzy comes with a middleware for date hydration [out of the box](https://github.com/duncangrubbs/ozzzy/blob/main/src/examples/basic.ts#L37-L42)!
