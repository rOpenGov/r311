# Endpoints

Modify and examine defined open311 endpoints. `o311_endpoints()`
retrieves a list of endpoints including additional information.
`o311_add_endpoint` adds to this list to define a new endpoint that can
be used for queries. `o311_reset_endpoints` restores the initial state
of the endpoints list.

## Usage

``` r
o311_add_endpoint(
  name,
  root,
  jurisdiction = NULL,
  key = FALSE,
  pagination = FALSE,
  limit = NULL,
  json = TRUE,
  dialect = NULL
)

o311_reset_endpoints()

o311_endpoints(...)
```

## Arguments

- name:

  `[character]`

  Name of an endpoint / city. This name can be arbitrary and only serves
  for identification in `[o311_api]`.

- root:

  `[character]`

  Base URL of the endpoint for sending production-grade requests. The
  root URL commonly points to `"georeport/v2/"`.

- jurisdiction:

  `[character]`

  Unique identifier of the jurisdiction. The jurisdiction is typically
  defined as the domain of the respective city website. It is optional
  as most endpoints only serve one jurisdiction.

- key:

  `[logical]`

  Is an API key mandatory?

- pagination:

  `[logical]`

  Are `requests` responses paginated?

- limit:

  `[integer]`

  If paginated, how many requests does one page contain?

- json:

  `[logical]`

  Are JSON responses supported? If `FALSE`, defaults to `"XML"`
  responses. See also
  [`o311_api`](https://ropengov.github.io/r311/reference/o311_api.md).

- dialect:

  `[character]`

  open311 extension that the endpoint is built on. Common dialects
  include CitySDK, Connected Bits, SeeClickFix and Mark-a-Spot.
  Currently, this argument does nothing, but it could be used in the
  future to adjust response handling based on dialect.

- ...:

  List of key-value pairs where each pair is a filter. The key
  represents the column and the value the requested column value. All
  keys must be present in the column names of `o311_endpoints()`.

## Value

For `o311_endpoints`, a dataframe containing all relevant information on
an endpoint. For `o311_add_endpoint`, the new endpoint, invisibly.
`o311_reset_endpoints` returns `NULL` invisibly. If the new endpoint is
a duplicate, `NULL` is returned invisibly.

## Details

`o311_endpoints()` returns a static list defined in the package
installation directory. This list contains a limited number of endpoints
that were proven to work at the time of package development. It does not
include newer/smaller/less known endpoints or test APIs. These can be
manually added using `o311_add_endpoint`. If an API is down and it is
unknown whether it will be up again or not, its `questioning` value is
set to `TRUE`. If they will not go up again, they are removed upon the
next release.

## Note

This function uses [`R_user_dir`](https://rdrr.io/r/tools/userdir.html)
to persistently store custom endpoints data between sessions. To set a
different directory, you may use `options("o311_user_dir")`. To clean
up, run `o311_reset_endpoints()` which deletes the package-specific user
directory and defaults back to
`system.file("endpoints.json", package = "r311")`.

## See also

[`o311_api`](https://ropengov.github.io/r311/reference/o311_api.md)

## Examples

``` r
# read default endpoints
o311_endpoints()
#> # A tibble: 70 × 12
#>    name            root  docs  jurisdiction key   pagination limit json  dialect
#>    <chr>           <chr> <chr> <chr>        <lgl> <lgl>      <int> <lgl> <chr>  
#>  1 Annaberg-Buchh… http… http… annberg-buc… FALSE TRUE          50 TRUE  Mark-a…
#>  2 Bloomington, IN http… NA    bloomington… FALSE TRUE        1000 TRUE  uReport
#>  3 Bonn, DE        http… http… bonn.de      FALSE TRUE         100 TRUE  Mark-a…
#>  4 Boston, MA      http… http… NA           FALSE TRUE          50 TRUE  Connec…
#>  5 Brookline, MA   http… http… brooklinema… FALSE TRUE          50 TRUE  Connec…
#>  6 Austin, TX      http… http… NA           FALSE TRUE          50 TRUE  Connec…
#>  7 Chicago, IL     311a… 311a… cityofchica… FALSE TRUE          50 TRUE  Connec…
#>  8 Newport News, … http… http… cityofnewpo… FALSE TRUE          50 TRUE  Connec…
#>  9 San Diego, CA   http… http… sandiego.gov FALSE TRUE          50 TRUE  Connec…
#> 10 Köln / Cologne… http… NA    stadt-koeln… FALSE TRUE          50 TRUE  Mark-a…
#> # ℹ 60 more rows
#> # ℹ 3 more variables: deprecated <lgl>, deprecated_reason <chr>,
#> #   deprecated_url <chr>

# get all endpoints powered by Connected Bits
o311_endpoints(dialect = "Connected Bits")
#> # A tibble: 7 × 12
#>   name  root  docs  jurisdiction key   pagination limit json  dialect deprecated
#>   <chr> <chr> <chr> <chr>        <lgl> <lgl>      <int> <lgl> <chr>   <lgl>     
#> 1 Bost… http… http… NA           FALSE TRUE          50 TRUE  Connec… FALSE     
#> 2 Broo… http… http… brooklinema… FALSE TRUE          50 TRUE  Connec… FALSE     
#> 3 Aust… http… http… NA           FALSE TRUE          50 TRUE  Connec… FALSE     
#> 4 Chic… 311a… 311a… cityofchica… FALSE TRUE          50 TRUE  Connec… FALSE     
#> 5 Newp… http… http… cityofnewpo… FALSE TRUE          50 TRUE  Connec… FALSE     
#> 6 San … http… http… sandiego.gov FALSE TRUE          50 TRUE  Connec… FALSE     
#> 7 San … http… http… NA           FALSE TRUE          50 TRUE  Connec… FALSE     
#> # ℹ 2 more variables: deprecated_reason <chr>, deprecated_url <chr>

# add a new endpoint
o311_add_endpoint(name = "test", root = "test.org/georeport/v2")

# read new endpoints
o311_endpoints()
#> # A tibble: 71 × 12
#>    name            root  docs  jurisdiction key   pagination limit json  dialect
#>    <chr>           <chr> <chr> <chr>        <lgl> <lgl>      <int> <lgl> <chr>  
#>  1 Annaberg-Buchh… http… http… annberg-buc… FALSE TRUE          50 TRUE  Mark-a…
#>  2 Bloomington, IN http… NA    bloomington… FALSE TRUE        1000 TRUE  uReport
#>  3 Bonn, DE        http… http… bonn.de      FALSE TRUE         100 TRUE  Mark-a…
#>  4 Boston, MA      http… http… NA           FALSE TRUE          50 TRUE  Connec…
#>  5 Brookline, MA   http… http… brooklinema… FALSE TRUE          50 TRUE  Connec…
#>  6 Austin, TX      http… http… NA           FALSE TRUE          50 TRUE  Connec…
#>  7 Chicago, IL     311a… 311a… cityofchica… FALSE TRUE          50 TRUE  Connec…
#>  8 Newport News, … http… http… cityofnewpo… FALSE TRUE          50 TRUE  Connec…
#>  9 San Diego, CA   http… http… sandiego.gov FALSE TRUE          50 TRUE  Connec…
#> 10 Köln / Cologne… http… NA    stadt-koeln… FALSE TRUE          50 TRUE  Mark-a…
#> # ℹ 61 more rows
#> # ℹ 3 more variables: deprecated <lgl>, deprecated_reason <chr>,
#> #   deprecated_url <chr>

# reset endpoints back to default
o311_reset_endpoints()
```
