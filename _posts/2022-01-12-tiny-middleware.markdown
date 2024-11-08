---
layout: post
title: "A tiny date hydration middleware"
date: 2022-01-12 22:47:19 -0400
categories: engineering
---

I have been helping to build Flowlie since January of 2021 and it has been an incredible learning experience. Our entire stack is written in Typescript (React and Express) and we use MongoDB for our database. One issue that we were running into quite often is that when we were sending dates from our frontend to our API and vice versa they we getting serialized as strings. So for example, on our frontend we would initialize a date when someone updated a fundraising round using new Date() and then store it in some object and hand it off to the API via POST request. However, the backend would see something like this 2022-01-12T23:54:34.574Z.

The problem here is that native Javascript Date objects are serialized as strings in JSON, which is the most common format for REST APIs. This was frustrating because if we wanted to do any calculations with the date or compare it to another one we needed to initialize a new Date object. Luckily, Express.js has a super nice middleware feature that allows you to intercept requests and do something to them before passing them off to the next handler. Here is a super simple example of a middleware that solves this problem. It takes the request body data, looks for patterns that match a string serialization of a Date object and hydrates them back into Date objects. This of course is only practical if your backend is also written in TS/JS.

This was great and all, but it was only half of the equation. What about sending dates to the frontend from the API, how could we handle that situation? After giving it some thought we decided why not create a wrapper that would let us apply middleware to our frontend request handlers. In my earlier blog post, I introduced ozzy. Since then I have been working on an update that supports a similar middleware API to Express.js. Using ozzy, we could solve the same problem on the frontend and the backend using essentially the same code.

This also extends beyond this one use case. Middlewares are great for all kinds of problems you might be facing, and then are just as fun to use on the frontend as they are on the backend.
