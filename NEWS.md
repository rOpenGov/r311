# r311 0.4.3

* API list upkeep
  * Added: Espoo, Oulu (#2)
  * Changed: Turku (#2)
  * Deprecated: Ottawa, Siegburg, Northfield, Annaberg-Buchholz
* Introduced deprecation system of available APIs
  * Added a key "questioned" to the API list which signals that an API might be abandoned
  * Added a key "deprecated" to the API list which signals that an API is no longer functional
  * `o311_api()` now warns when an API is questioned
  * `o311_api()` now aborts when an API is deprecated
* Added pretty formatting of error messages
* Changed heuristics in `o311_request_all()` to compare requests using set equality of request IDs instead of exact object equality
* Fix bug where formal checks in `validate_endpoints()` create an invalid dataframe
* Ensure that API error handling always yields a valid condition object
* `o311_api` does not match regular expressions anymore
* `o311_ok(error = TRUE)` now returns an error instead of an error object
* Add an explanatory note if an HTTP error is unknown
* Added a CITATION file (#3)
* Added a codemeta file
* Add precomputed vignettes


# r311 0.3.7

* Initial CRAN submission.
