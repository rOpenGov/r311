# Get service list

Get a list of available services. Services are unique to the endpoint /
city and thus require an attached jurisdiction using
[`o311_api`](https://ropengov.github.io/r311/reference/o311_api.md).

## Usage

``` r
o311_services(...)

o311_service(service_code, ...)
```

## Arguments

- ...:

  Further endpoint-specific parameters as documented in the respective
  endpoint reference.

- service_code:

  Identifier of a single service definition. Service codes can usually
  be retrieved from `o311_services`.

## Value

A dataframe or list containing information about each service.

## Examples

``` r
# set up a jurisdiction
o311_api("san francisco")
# \donttest{
if (o311_ok()) {
  # get a list of all services
  services <- o311_services()

  # inspect a service code
  o311_service(services$service_code[1])
}
#> # A tibble: 1 × 2
#>   service_code        attributes$variable $code       $datatype $required $order
#>   <chr>               <lgl>               <chr>       <chr>     <lgl>      <int>
#> 1 RPD:General:General TRUE                oform.rpd_… singleva… TRUE           1
#> # ℹ 2 more variables: attributes$description <chr>, $values <list>
# }
```
