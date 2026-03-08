# API Testing Portfolio
This project was created to demonstrate my API testing skills using a real public API. The collection was designed by first understanding the API from a user perspective, then examining actual response data to build accurate schemas, rather than relying solely on documentation.

## Attribution
This product uses TMDB and the TMDB APIs but is not endorsed, certified, or otherwise approved by TMDB.

## The API
The Movie Database (TMDB) is a community-built movie and TV database. This collection tests the following endpoints:
- https://api.themoviedb.org/3/movie/popular - returns a paginated list of popular movies
- https://api.themoviedb.org/3/tv/popular - returns a paginated list of popular TV shows

## What the collection tests
- Schema validation with AJV across movie and TV list endpoints
- Range validation on numeric fields (`vote_average` 0-10, `popularity`, `vote_count`)
- Array item type validation (`genre_ids`, `origin_country`)
- Null handling on fields with known undocumented behavior (`backdrop_path`, `poster_path`)
- Date format validation (ISO 8601)
- ID integrity checks
- Duplicate detection
- Pagination metadata validation
- Content-Type header validation
- Response time assertion

## Design decisions
- The schemas were designed from actual response data, not just documentation.
- Null handling was added based on observed API behavior vs documented behavior.

## Findings during development
These findings were identified during schema design by comparing official documentation against actual API responses.
- The `adult` field is present in the TV responses but missing from the official documentation.
- The documentation field order doesn't match the actual response order for the TV endpoint.

## How to run
1. Register for a free TMDB API key by creating an account at https://www.themoviedb.org/signup and then going to https://www.themoviedb.org/settings/api
2. Import the collection into Postman
3. Create an environment with the following variables:
   - `accessToken` — your TMDB Read Access Token (used as a Bearer token)
4. Run the collection
