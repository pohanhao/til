# Restful API tips

1. Use nouns but no verbs
2. GET method and query parameters should not alter the state
3. Use plural nouns
4. Use sub-resources for relations
5. Use HTTP headers for serialization formats
6. Use HATEOAS

  ```json
  {
    "id": 711,
    "manufacturer": "bmw",
    "model": "X5",
    "seats": 5,
    "drivers": [{
      "id": "23",
      "name": "Stefan Jauker",
      "links": [{
        "rel": "self",
        "href": "/api/v1/drivers/23"
      }]
    }]
  }
  ```
7. Provide filtering, sorting, field selection and paging for collections

  ```
  GET /cars?color=red Returns a list of red cars
  GET /cars?seats<=2 Returns a list of cars with a maximum of 2 seats

  GET /cars?sort=-manufactorer,+model

  GET /cars?fields=manufacturer,model,id,color

  GET /cars?offset=10&limit=5
  ```
8. Version your API
9. Handle Errors with HTTP status codes
10. Allow overriding HTTP method

**Reference**

1. http://blog.mwaysolutions.com/2014/06/05/10-best-practices-for-better-restful-api/
