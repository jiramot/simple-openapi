# Simple Open API

- [x] [oauth2 server](https://github.com/jiramot/go-oauth2)
- [x] [login server (idp)](https://github.com/jiramot/oauth2-sso-web)
- [ ] consent server
- [ ] resource server
- [x] [api gateway (kong)](https://github.com/jiramot/kong-oauth2-plugin)
- [x] [frontend javascript library](https://github.com/jiramot/openapi-frontend-lib)

## System Architecture
```
                                      ┌────────────────────────────────────────────────────┐
                                      │┌───────────┐               ┌──────────┐            │
                           get        ││           │               │ Consent  │            │
          ┌──────────────consent──────┼┼───────────┼──────────────▶│          │            │
          │                           ││           │               └──────────┘            │
          │                           ││           │   ┌──────────┐      │                 │
          │                           ││           │   │Resources │      │                 │
          │                          ┌┼┼───────────┼──▶│   API    │      │                 │
          │                          │││           │   └──────────┘      ▼                 │
┌──────────────────┐                 │││           │               ┌──────────┐            │
│                  │                 │││           │               │  Oauth2  │            │
│                  │                 │││API Gate───┼─introspect───▶│          │◀─┐         │
│   Web Browser    │        get      │││  (Kong)   │               └──────────┘  │         │
│      Client      │─────resources───┘││           │                     ▲       │         │
│       ┌──────────┤                  ││           │             accept  │       │         │
│       │  JS SDK  │───────────login─┐││           │          ┌──login ──┘       │         │
└───────┴──────────┘                 │││           │          │ request          │         │
                                     │││           │          │               accept       │
                                     │││           │    ┌──────────┐          login        │
                                     │││           │    │ SSO Web  │         request       │
                                     └┼┼───────────┼───▶│  (idp)   │             │         │
                                      ││           │    └──────────┘             │         │
                                      ││           │                             │         │
             ┌──────────────────┐     │└───────────┘                             │         │
             │                  │     └──────────────────────────────────────────┼─────────┘
             │                  │     ┌──────────────────────────────────────────┴─────────┐
             │  Native Client   │     │  implicit    ┌──────────┐      ┌──────────────────┐│
             │                  │─────┼───login────┐ │  Login   │      │   Native Login   ││
             │       ┌──────────┤     │            └─┤          ├─────▶│     Adapter      ││
             │       │  JS SDK  │     │              └──────────┘      └──────────────────┘│
             └───────┴──────────┘     └────────────────────────────────────────────────────┘
```