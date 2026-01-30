# Select an open311 API

Select an open311 API and attach it to the active session. An open311
API is an implementation of the open311 standard. It consists of an
endpoint name (e.g. a city), a root URL, and a jurisdiction ID. To
unambiguously identify an API, you can provide an endpoint, a
jurisdiction ID, or both. The input is matched with
[`o311_endpoints`](https://ropengov.github.io/r311/reference/o311_endpoints.md)
to select an API. The selected API is available to other `o311_*`
functions until the session is terminated or until it is overwritten.

## Usage

``` r
o311_api(
  endpoint = NULL,
  jurisdiction = NULL,
  key = NULL,
  format = c("json", "xml")
)
```

## Arguments

- endpoint:

  `[character]`

  Name of an endpoint that runs an open311 API. This is usually a city,
  but can be any provider of an open311 API.

- jurisdiction:

  `[character]`

  ID of a jurisdiction that is served by an open311 API. A jurisdiction
  ID is usually the root URL of the jurisdiction website, e.g.
  `"sandiego.gov"` for San Diego.

- key:

  `[character]`

  If a key is required by the selected API, this argument can be used to
  store the key in the R session. The API key is automatically used in
  API requests. If `key` is `NULL` although a key is required, a warning
  is emitted.

- format:

  `[character]`

  Response format. Must be one of `"json"` or `"xml"`. Defaults to
  `"json"` because simplification is more difficult and unsafe for
  `xml2` objects. It is advisable to use `"json"` whenever possible and
  applicable. Additionally, `"xml"` requires the `xml2` package for
  queries and the `xmlconvert` package for simplification.

## Value

A list containing the most important information on a given
jurisdiction, invisibly. This list is attached to the session and can be
retrieved by calling `o311_api()` without arguments. Passing no
arguments returns the currently attached API object.

## Details

In theory, several jurisdictions can exist for a single endpoints, e.g.
if a region serves multiple jurisdictions. Similarly, multiple endpoints
can exist for a single jurisdiction, e.g. if a provider has set up both
production and test endpoints for a jurisdictions. Providing both
endpoint and jurisdiction is thus the most safe way to identify an API.

By default, only a handful of endpoints are supported. For a list of
currently supported endpoints, run
[`o311_endpoints`](https://ropengov.github.io/r311/reference/o311_endpoints.md).
You can add non-default endpoints using
[`o311_add_endpoint`](https://ropengov.github.io/r311/reference/o311_endpoints.md).

## See also

[`o311_requests`](https://ropengov.github.io/r311/reference/o311_requests.md),
[`o311_request`](https://ropengov.github.io/r311/reference/o311_requests.md),
[`o311_services`](https://ropengov.github.io/r311/reference/o311_services.md)

## Examples

``` r
# cities are matched using regex
o311_api("Cologne")

# passing a jurisdiction is more explicit
o311_api(jurisdiction = "stadt-koeln.de")

# calls without arguments return the current API
o311_api()
#> <r311_api>
#>  name         : Köln / Cologne, DE
#>  root         : https://sags-uns.stadt-koeln.de/georeport/v2/
#>  jurisdiction : stadt-koeln.de
#>  key          : FALSE
#>  pagination   : TRUE
#>  limit        : 50
#>  json         : TRUE
#>  dialect      : Mark-a-Spot
#>  deprecated   : FALSE 
```
