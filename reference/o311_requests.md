# Get civic service request data

Get civic service request data from a registered open311 endpoint.
`o311_request` queries a single service request by ID. `o311_requests`
queries a single page of service requests. `o311_request_all` tries to
iterate through all pages of an endpoint to return a complete dataset of
service requests.

## Usage

``` r
o311_requests(
  service_code = NULL,
  start_date = NULL,
  end_date = NULL,
  status = NULL,
  page = NULL,
  ...
)

o311_request(service_request_id, ...)

o311_request_all(
  service_code = NULL,
  start_date = NULL,
  end_date = NULL,
  status = NULL,
  ...,
  max_pages = Inf,
  progress = TRUE
)
```

## Arguments

- service_code:

  `[character]`

  IDs of the service types to be queried. Defaults to all available
  codes of an endpoint. A list of all available service codes can be
  retrieved using
  [`o311_services`](https://ropengov.github.io/r311/reference/o311_services.md).

- start_date, end_date:

  `[POSIXt]`

  Start date and end date of the query results. Must be date-time
  objects. If not specified, defaults to the last 90 days.

- status:

  `[character]`

  Status of the public service ticket. Can be one of `"open"` or
  `"closed"`. If `NULL`, returns all types of tickets.

- page:

  `[integer]`

  Page of the response. Most endpoints paginate their responses in a way
  that only a limited number of tickets are returned with each query. To
  retrieve all data, consider using `o311_request_all`.

- ...:

  Further endpoint-specific parameters as documented in the respective
  endpoint reference.

- service_request_id:

  `[character]`

  Identifier of a single service request. Request IDs can usually be
  retrieved from `o311_requests`.

- max_pages:

  `[integer]`

  Number of pages to search until the result is returned.

- progress:

  `[logical]`

  Whether to show a waiter indicating the current page iteration.

## Value

A dataframe containing data on civic service requests. The dataframe can
contain varying columns depending on the open311 implementation.

## Details

`o311_request_all` applies a number of checks to determine when to stop
searching. First, many endpoints return an error if the last page is
exceeded. Thus, if the last page request failed, break. Second, if
exceeding the pagination limit does not return an error, the response is
compared with the previous response. If identical, the response is
discarded and all previous responses returned. Finally, if the page
exceeds `max_pages`, the responses up to this point are returned.

open311 leaves space for endpoints to implement their own request
parameters. These parameters can be provided using dot arguments. These
arguments are not validated or pre-processed. Date-time objects must be
formatted according to the [w3c](https://www.w3.org/TR/NOTE-datetime)
standard. Some more common parameters include:

- `q`: Perform a text search across all requests.

- `update_after`/`updated_before`: Limit request according to request
  update dates.

- `per_page`: Specifiy the maximum number of requests per page.

- `extensions`: Adds a nested attribute `"extended_attributes"` to the
  response.

- `long`/`lat`/`radius`: Searches for requests in a fixed radius around
  a coordinate.

As dot arguments deviate from the open311 standard, they are not
guaranteed to be available for every endpoint and might be removed
without further notice. Refer to the endpoint docs to learn more about
custom parameters (`o311_endpoints()$docs`).

## See also

[`o311_api`](https://ropengov.github.io/r311/reference/o311_api.md)

## Examples

``` r
o311_api("zurich")
# \donttest{
if (o311_ok()) {
  # retrieve requests from the last two days
  now <- Sys.time()
  two_days <- 60 * 60 * 24 * 2
  o311_requests(end_date = now, start_date = now - two_days)

  # retrieve only open tickets
  tickets <- o311_requests(status = "open")

  # request the first ticket of the previous response
  rid <- as.character(tickets$service_request_id[1])
  o311_request(rid)

  if (interactive()) {
    # request all data
    o311_request_all()
  }

  # request data of the first 5 pages
  o311_request_all(max_pages = 5)
}
#> Simple feature collection with 1000 features and 13 fields
#> Geometry type: POINT
#> Dimension:     XY
#> Bounding box:  xmin: 8.468205 ymin: 47.32461 xmax: 8.602686 ymax: 47.43051
#> Geodetic CRS:  WGS 84
#> # A tibble: 1,000 × 14
#>    media_url updated_datetime detail description service_code requested_datetime
#>    <chr>     <chr>            <chr>  <chr>       <chr>        <chr>             
#>  1 https://… 2026-02-26T09:0… Mittl… Mittleres … Grünflächen… 2026-02-26T08:39:…
#>  2 NA        2026-02-26T08:5… Unbew… Unbewillig… Allgemein    2026-02-26T07:49:…
#>  3 NA        2026-02-26T08:5… Der L… Der Lift i… Strasse/Tro… 2026-02-26T07:44:…
#>  4 NA        2026-02-26T08:4… Sehr … Sehr geehr… Strasse/Tro… 2026-02-25T20:36:…
#>  5 NA        2026-02-26T06:0… Parkb… Parkbank l… Grünflächen… 2026-02-25T19:34:…
#>  6 https://… 2026-02-26T07:0… Abfal… Abfallents… Abfall/Samm… 2026-02-25T19:29:…
#>  7 https://… 2026-02-26T08:4… Der G… Gully vers… Strasse/Tro… 2026-02-25T19:21:…
#>  8 https://… 2026-02-25T16:4… Schaf… Schaffhaus… Signalisati… 2026-02-25T16:28:…
#>  9 https://… 2026-02-25T16:5… Depon… Deponieren… Abfall/Samm… 2026-02-25T16:13:…
#> 10 https://… 2026-02-25T15:1… Die P… Die Putzak… Abfall/Samm… 2026-02-25T15:08:…
#> # ℹ 990 more rows
#> # ℹ 8 more variables: status <chr>, title <chr>, service_notice <chr>,
#> #   interface_used <chr>, service_request_id <int>, service_name <chr>,
#> #   agency_sent_datetime <chr>, geometry <POINT [°]>
# }
```
