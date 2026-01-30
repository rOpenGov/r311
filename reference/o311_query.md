# Query an open311 endpoint

Low-level function to perform a generic request to the API currently
attached by `o311_api`. Some open311 implementations support unique
operations that are not included in the official documentation. This
function can be used to access these URL paths.

## Usage

``` r
o311_query(path, ..., simplify = TRUE)
```

## Arguments

- path:

  Path appendix used to access endpoint-specific operations.

- ...:

  Additional query parameters.

- simplify:

  Whether to simplify the output using
  `jsonlite::toJSON(..., simplify = TRUE)`.

## Value

The parsed query output, either as a list or dataframe.

## Details

You can set `options(r311_echo = TRUE)` to display all requests sent
using `o311_query`.

## Examples

``` r
o311_api("rostock")
# \donttest{
# manually query discovery
o311_query(path = "discovery", simplify = FALSE)
#> $changeset
#> [1] "2015-11-05 08:43"
#> 
#> $contact
#> [1] "Hanse- und Universitätsstadt Rostock, Kataster-, Vermessungs- und Liegenschaftsamt, An der Jägerbäk 3, 18069 Rostock, klarschiff.hro@rostock.de"
#> 
#> $key_service
#> [1] "klarschiff.hro@rostock.de"
#> 
#> $endpoints
#> $endpoints[[1]]
#> $endpoints[[1]]$specification
#> [1] "http://wiki.open311.org/GeoReport_v2"
#> 
#> $endpoints[[1]]$url
#> [1] "https://www.klarschiff-hro.de/backoffice/citysdk"
#> 
#> $endpoints[[1]]$changeset
#> [1] "2015-11-05 08:43"
#> 
#> $endpoints[[1]]$type
#> [1] "production"
#> 
#> $endpoints[[1]]$formats
#> $endpoints[[1]]$formats[[1]]
#> [1] "application/json"
#> 
#> $endpoints[[1]]$formats[[2]]
#> [1] "text/xml"
#> 
#> 
#> 
#> $endpoints[[2]]
#> $endpoints[[2]]$specification
#> [1] "http://wiki.open311.org/GeoReport_v2"
#> 
#> $endpoints[[2]]$url
#> [1] "https://support.klarschiff-hro.de/backoffice/citysdk"
#> 
#> $endpoints[[2]]$changeset
#> [1] "2015-11-05 08:43"
#> 
#> $endpoints[[2]]$type
#> [1] "test"
#> 
#> $endpoints[[2]]$formats
#> $endpoints[[2]]$formats[[1]]
#> [1] "application/json"
#> 
#> $endpoints[[2]]$formats[[2]]
#> [1] "text/xml"
#> 
#> 
#> 
#> 

# query a custom path defined by the Klarschiff API
o311_query(path = "areas")
#> Simple feature collection with 1 feature and 2 fields
#> Geometry type: MULTIPOLYGON
#> Dimension:     XY
#> Bounding box:  xmin: 11.99837 ymin: 54.0508 xmax: 12.2954 ymax: 54.24451
#> CRS:           NA
#> # A tibble: 1 × 3
#>      id name                                                              grenze
#>   <int> <chr>                                                     <MULTIPOLYGON>
#> 1     1 Klarschiff.HRO (((11.99844 54.17483, 11.99852 54.17496, 11.9988 54.1750…
# }
```
