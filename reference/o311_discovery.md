# API discovery

Retrieve discovery information about the mounted endpoint.

## Usage

``` r
o311_discovery()
```

## Value

A list containing details on the given open311 API.

## Examples

``` r
o311_api("zurich")
# \donttest{
if (o311_ok()) {
  o311_discovery()
}
#> $discovery
#> $discovery$changeset
#> [1] "2021-03-01T00:00:00Z"
#> 
#> $discovery$endpoints
#> $discovery$endpoints[[1]]
#> $discovery$endpoints[[1]]$formats
#> $discovery$endpoints[[1]]$formats[[1]]
#> [1] "text/xml"
#> 
#> $discovery$endpoints[[1]]$formats[[2]]
#> [1] "application/json"
#> 
#> $discovery$endpoints[[1]]$formats[[3]]
#> [1] "text/html"
#> 
#> 
#> $discovery$endpoints[[1]]$changeset
#> [1] "2021-03-01T00:00:00Z"
#> 
#> $discovery$endpoints[[1]]$type
#> [1] "production"
#> 
#> $discovery$endpoints[[1]]$specification
#> [1] "http://wiki.open311.org/GeoReport_v2"
#> 
#> $discovery$endpoints[[1]]$url
#> [1] "https://www.zueriwieneu.ch/open311"
#> 
#> 
#> 
#> $discovery$contact
#> [1] "Send email to zurich@fixmystreet.com."
#> 
#> $discovery$max_requests
#> [1] "1000"
#> 
#> 
# }
```
