# Filter Card Issuing Requests

With this filter, it is possible to find the requests to issue cards, by removing intermediate steps (`activate`, `personalize` and `pin`):

```
message: "request: POST/v1/issuing/cards" AND NOT message: "pin" AND NOT message: "activate" AND NOT message: "personalize"
```