1. Bypassing rate limiting on Login, OTP using [Aliases](https://portswigger.net/web-security/graphql#bypassing-rate-limiting-using-aliases)/[GraphQL Batching attack](https://lab.wallarm.com/graphql-batching-attack/)
2. Send same reset password token to 2 emails (similar to race condition)
3. If a query takes long time to complete, it used to overwhelm the server by using [GraphQL Batching attack](https://lab.wallarm.com/graphql-batching-attack/)
