1. Bypassing rate limiting on Login, OTP using [Aliases](https://portswigger.net/web-security/graphql#bypassing-rate-limiting-using-aliases)/[GraphQL Batching attack](https://lab.wallarm.com/graphql-batching-attack/)
2. Send same reset password token to 2 emails (similar to race condition) using GraphQL Batching attack
3. If a query takes long time to complete, it used to overwhelm the server by using GraphQL Batching attack
4. If The application offers two types, which reference eachother (Type1 has Type2, and Type2 has Type1), it recursive query to be executed successfully.
  ![image](https://github.com/0xGLSS/Bug-Bounty-Methodology/assets/85647797/cc9686ca-662c-4327-95c6-e2e47ceca255)

'

    query {
      pastes {
        owner {
          pastes {
            owner {
              pastes {
                owner {
                  pastes {
                    owner {
                      pastes {
                        owner {
                          pastes {
                            owner {
                              name
                            }
                          }
                        }
                      }
                    }
                  }
                }
              }
            }
          }
        }
      }
    }
> [!NOTE]
> if a depth check is implemented it should prevent malicious queries from going through.

    



5. CSRF over Graphql
6. 
