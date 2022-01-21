# Sorting, filtering and paging

## Send a request

An example using this container model:

```json
{
  "code": "ABCU8880048",
  "lastMaintenanceDate": "2022-01-02T11:00:00Z",
  "color": "black",
  "capacity": 10000,
  "compartments": 3,
  "clean": true,
  "id": "00000000-0000-0000-0000-000000000001",
  "inactivityInformation": {
    "Inactive": false,
    "MadeInactiveAt": null,
    "MadeInactiveBy": null,
    "ReasonInactive": null
  }
}
```

```curl
GET /GetPosts

?sorts=     code,capacity,-lastMaintenanceDate  // sort by code, then capacity, then descendingly by last maintenance date 
&filters=   capacity>1000, code@=ABCU8880198,   // filter to capacity with more than 1000, and a code that contains the phrase "ABCU8880198"
&page=      1                                   // get the first page...
&pageSize=  10                                  // ...which contains 10 containers

```
More formally:
* `sorts` is a comma-delimited ordered list of property names to sort by. Adding a `-` before the name switches to sorting descendingly.
* `filters` is a comma-delimited list of `{Name}{Operator}{Value}` where
    * `{Name}` is the name of a property or the name of a custom filter method
        * You can also have multiple names (for OR logic) by enclosing them in brackets and using a pipe delimiter, eg. `(capacity|compartments)>10` asks if `capacity` or `compartments` is `>10`
    * `{Operator}` is one of the [Operators](#operators)
    * `{Value}` is the value to use for filtering
        * You can also have multiple values (for OR logic) by using a pipe delimiter, eg. `code@=8880|ABC` will return posts with codes that contain the text "`8880`" or "`ABC`"
* `page` is the number of page to return
* `pageSize` is the number of items returned per page 

Notes:
* You can use backslashes to escape special characters and sequences:
    * commas: `code@=some\,code` makes a match with "some,code"
    * pipes: `code@=some\|code` makes a match with "some|code"
    * null values: `code@=\null` will search for items with code equal to "null" (not a missing value, but "null"-string literally)
* You can have spaces anywhere except *within* `{Name}` or `{Operator}` fields

## Operators
| Operator   | Meaning                  |
|------------|--------------------------|
| `==`       | Equals                   |
| `!=`       | Not equals               |
| `>`        | Greater than             |
| `<`        | Less than                |
| `>=`       | Greater than or equal to |
| `<=`       | Less than or equal to    |
| `@=`       | Contains                 |
| `_=`       | Starts with              |
| `!@=`      | Does not Contains        |
| `!_=`      | Does not Starts with     |
| `@=*`      | Case-insensitive string Contains |
| `_=*`      | Case-insensitive string Starts with |
| `==*`      | Case-insensitive string Equals |
| `!=*`      | Case-insensitive string Not equals |
| `!@=*`     | Case-insensitive string does not Contains |
| `!_=*`     | Case-insensitive string does not Starts with |
