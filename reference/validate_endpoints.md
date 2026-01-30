# Validate endpoints

Checks whether and which endpoints are correctly defined, reachable,
and/or valid. Iterates through all endpoints defined in
[`o311_endpoints`](https://ropengov.github.io/r311/reference/o311_endpoints.md)
and returns their status along with a reason, if applicable.

## Usage

``` r
validate_endpoints(
  idx = NULL,
  checks = c("discovery", "services", "requests"),
  methods = c("formal", "down", "valid")
)
```

## Arguments

- idx:

  `[integer]`

  Index numbers of endpoints to check. Index numbers follow row numbers
  in
  [`o311_endpoints`](https://ropengov.github.io/r311/reference/o311_endpoints.md).

- checks:

  `[character]`

  Which open311 method to check. By default, checks all methods.

- methods:

  `[character]`

  Which checks to apply. `formal` checks whether an endpoint is uniquely
  identifiable through given names and jurisdictions in
  [`o311_endpoints`](https://ropengov.github.io/r311/reference/o311_endpoints.md).
  `down` checks whether an endpoint is reachable and ready for requests.
  `valid` checks whether a method returns a valid output, i.e. a list or
  dataframe with more than 0 rows/elements. By default, applies all
  methods.

## Value

A dataframe containing the name of the endpoint, one to three columns on
check results, and one to three columns on reasons if a check turned out
to be negative.

## Examples

``` r
# \donttest{
# check the first three endpoints in o311_endpoints()
validate_endpoints(1:3)
#> Receiving endpoint 1 out of 3...Receiving endpoint 2 out of 3...Receiving endpoint 3 out of 3...
#> # A tibble: 3 × 7
#>   name              discovery services requests reason_discovery reason_services
#>   <chr>             <lgl>     <lgl>    <lgl>    <chr>            <chr>          
#> 1 Annaberg-Buchhol… FALSE     FALSE    FALSE    Deprecated       Deprecated     
#> 2 Bloomington, IN   FALSE     TRUE     TRUE     API not reachab… NA             
#> 3 Bonn, DE          FALSE     FALSE    FALSE    API not reachab… API not reacha…
#> # ℹ 1 more variable: reason_requests <chr>

# check only requests
validate_endpoints(1:3, checks = "requests")
#> Receiving endpoint 1 out of 3...Receiving endpoint 2 out of 3...Receiving endpoint 3 out of 3...
#> # A tibble: 3 × 3
#>   name                  requests reason_requests  
#>   <chr>                 <lgl>    <chr>            
#> 1 Annaberg-Buchholz, DE FALSE    Deprecated       
#> 2 Bloomington, IN       TRUE     NA               
#> 3 Bonn, DE              FALSE    API not reachable

# check only whether an endpoint is down
validate_endpoints(1:3, methods = "down")
#> Receiving endpoint 1 out of 3...Receiving endpoint 2 out of 3...Receiving endpoint 3 out of 3...
#> # A tibble: 3 × 7
#>   name              discovery services requests reason_discovery reason_services
#>   <chr>             <lgl>     <lgl>    <lgl>    <chr>            <chr>          
#> 1 Annaberg-Buchhol… FALSE     FALSE    FALSE    Deprecated       Deprecated     
#> 2 Bloomington, IN   TRUE      TRUE     TRUE     NA               NA             
#> 3 Bonn, DE          TRUE      TRUE     TRUE     NA               NA             
#> # ℹ 1 more variable: reason_requests <chr>
# }
```
