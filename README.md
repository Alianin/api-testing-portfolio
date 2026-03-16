# API Testing Portfolio
This project was created to demonstrate my API testing skills using a real public API. The collection was designed by first understanding the API from a user perspective, then examining actual response data to build accurate schemas, rather than relying solely on documentation.

## Attribution
This product uses TMDB and the TMDB APIs but is not endorsed, certified, or otherwise approved by TMDB.

## Files
- `tmdb-api-tests.json` - Postman collection (v2.1)

## Collection structure
- **Popular lists** - validates movie and TV popular list endpoints
- **Page detail** - validates movie and TV detail endpoints, chained from list results
- **Search** - (coming soon)

## The API
The Movie Database (TMDB) is a community-built movie and TV database. This collection tests the following endpoints:
- https://api.themoviedb.org/3/movie/popular - returns a paginated list of popular movies
- https://api.themoviedb.org/3/tv/popular - returns a paginated list of popular TV shows
- https://api.themoviedb.org/3/movie/{movie_id} - returns the details of a movie
- https://api.themoviedb.org/3/tv/{series_id} - returns the details of a TV show

## What the collection tests
### Popular lists
- Schema validation with AJV on movie and TV list results
- Range validation on numeric fields
- Array item type validation
- Null handling on fields with known undocumented behavior
- Date format validation (ISO 8601)
- ID integrity checks — type and range validation via schema and duplicate detection
- Pagination metadata validation
- Status code, Content-Type header, and response time validation

### Page detail
- Schema validation with AJV on movie and TV detail responses
- Nested object and array validation
- Nullable object validation using `oneOf`
- ISO 3166-1 and ISO 639-1 format validation using regex patterns
- Date format validation (ISO 8601) including nested episode and season dates
- Data integrity checks (season count consistency, episode/season ratio, ended shows without next episodes)
- Response ID verification confirming chained requests return the correct record
- Status code, Content-Type header, and response time validation

### Search
- Coming soon

## Design decisions
- The schemas were designed from actual response data, not just documentation.
- Null handling was added based on observed API behavior vs documented behavior.
- List sort order validation was investigated but not implemented. TMDB's popularity scores update in real time, meaning the sort order of a given page can shift between requests. Due to this inconsistency, a test written against this behavior would pass or fail at different times based on the data state. I decided to document this limitation instead of including a test that provides no reliable signal.
- Detail request IDs are captured dynamically from list responses using collection variables, requiring no manual configuration beyond the access token.
- `poster_path` and `backdrop_path` were not marked as required fields despite being displayed on the UI, as they are not guaranteed for all entries on this community-contributed platform.
- Response time threshold set at 500ms based on observed response times consistently under 100ms.

## Findings during development
These findings were identified during schema design by comparing official documentation against actual API responses.
### TV popular list
- The `adult` field is present in responses but missing from the official documentation.
- The documentation field order doesn't match the actual response order.

### Movie details
- `belongs_to_collection` is documented as a string but returns either null or an object containing `id`, `name`, `poster_path`, and `backdrop_path`.

### TV details
- `next_episode_to_air` is documented as a string but returns either null or a full episode object.
- `vote_average` in season objects is documented as integer but returns a number with decimal values.
- `episode_type` appears in episode objects in observed responses but is not documented.

## How to run
1. Register for a free TMDB API key by creating an account at https://www.themoviedb.org/signup and then going to https://www.themoviedb.org/settings/api.
2. Import the collection into Postman.
3. Create an environment with the following variables:
   - `accessToken` - your TMDB Read Access Token (used as a Bearer token)
4. Run the collection.
