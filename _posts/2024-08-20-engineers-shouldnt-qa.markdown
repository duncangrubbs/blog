---
layout: post
title: "Engineers Shouldn't QA"
date: 2024-08-20 23:01:19 -0400
categories: engineering
---

Like engineering, QA testing is a skill. Some engineers are good at it, some are mediocre, and some are bad. But that's beside the point. Engineers should focus on engineering, not on QA testing. It's not that we shouldn't understand how the application works or what the expected behavior is, it's just that QA testing requires a context switch; an entirely different headspace. To truly thoroughly QA you need a well-defined process and plenty of time.

Often times engineers work in small chunks of work. This way of working allows you to heavily focus on one small part of the application without worrying about the impact on the rest of it. In theory, a well tested codebase should do the rest and ensure you have not impacted anything else. However, many codebases are not well tested and require human QA. While I believe that peer QA testing is an important part of the review process, I do not believe that it is enough to ensure there are no regressions.

For companies with the means, this additional testing is perfect for QA engineers. But for most companies who aren't in a position to hire QA engineers, this responsibility should be on the product manager. In theory, they have the best understanding of the desired behavior of the product as well as unqiue insight into customer behavior and potential deal breakers.
