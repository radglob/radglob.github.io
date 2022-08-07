---
title: "APIs should be more usable."
date:  2020-03-04T13:18:00-05:00 
tags: ["apis"]
---

I've used a lot of third party APIs for work and personal projects. While some
APIs are phenomenally documented and well-designed (for example, [Stripe](https://stripe.com/docs)), to a fault, error handling is not great.

I see two fundamental issues here. First, most APIs don't document their failure
cases well. Second, when an API library is provided, they aren't particularly
explorable.

<!--more-->

Solving the first problem seems straightforward to me. In documentation, error
cases and their causes should be documented. This doesn't need to be exhaustive.
Enumerating how one might trigger a 5XX error seems unnecessary, but if there
are other known ways one might trigger a 404, that should be clear. Given that
most APIs I've seen barely adhere to proper response codes across the board, I
don't predict we'll see this anytime soon.

The second issue is much more involved, but would greatly improve adoption. When
someone downloads your first-pary API wrapper, it should do a few things.

First, work offline. This might seem odd, but I think API clients should include
some way to trigger known response without having to actually reach your servers.

Let's assume I have a `/users` endpoint. It can take two parameters:
a `last_name` substring for what the users' last name starts with, and a `count`
of how many users the query should return. A call to this endpoint might look
like this:

```elixir
API.list_users(%{last_name: "Ar", count: 10})
```

If this is live on production, it should query the server and return the correct
data. However, if we started our client with an `offline: true` flag, the client
should be able to return some random generated data. Given that you know the
schema of your API, this should be easy to generate, and could be produced
consistently by setting seed values for your RNG.

If you're then operating on some fake, local data, you should be able to handle
most cases where you would need to create new data or mutate something as well.
Deletion would be an odd case, but as long as you created new data when a client
was started, this could be reasonably handled.

The second improvement I would suggest is explorable errors. With our above API
call, if instead of executing the function, if we instead ran
`API.list_users.errors`, we should get a log of the possible error cases.
Calling something like `API.list_users.errors.NegativeCount` could then
explain the error case in further detail.

I might through together an example API for this to demonstrate, but for now, it's
just something I'm mulling over. I've worked with enough APIs that can't be used
offline without somewhat precarious mocks, and it feels like a place that could
use improvement.
