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
#> Bounding box:  xmin: 8.47187 ymin: 47.3324 xmax: 8.600715 ymax: 47.43103
#> Geodetic CRS:  WGS 84
#> # A tibble: 1,000 × 14
#>    requested_datetime  status service_name title interface_used detail media_url
#>    <chr>               <chr>  <chr>        <chr> <chr>          <chr>  <chr>    
#>  1 2026-01-30T17:13:2… open   Grünflächen… Ast … iOS            Ein A… https://…
#>  2 2026-01-30T16:58:0… open   Strasse/Tro… Wass… Android        Wasse… https://…
#>  3 2026-01-30T15:27:3… closed VBZ/ÖV       Kurs… Android        Kurs … https://…
#>  4 2026-01-30T11:25:1… closed Abfall/Samm… Hier… mobile         Hier … https://…
#>  5 2026-01-30T10:46:2… closed Abfall/Samm… Defe… iOS            Defek… https://…
#>  6 2026-01-30T10:41:5… closed Abfall/Samm… Lieg… iOS            Liegt… https://…
#>  7 2026-01-30T10:19:1… closed Abfall/Samm… Abfa… iOS            Abfall https://…
#>  8 2026-01-30T13:19:4… closed Strasse/Tro… Dole… desktop        Der r… https://…
#>  9 2026-01-30T13:09:5… closed Grünflächen… Neu … mobile         Neu g… https://…
#> 10 2026-01-30T09:32:2… closed Allgemein    Dive… mobile         Diver… https://…
#> # ℹ 990 more rows
#> # ℹ 7 more variables: updated_datetime <chr>, service_notice <chr>,
#> #   description <chr>, agency_sent_datetime <chr>, service_request_id <int>,
#> #   service_code <chr>, geometry <POINT [°]>
# }
```
