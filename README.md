# REST API Testing Framework

Automated REST API tests for [Restful Booker](https://restful-booker.herokuapp.com/apidoc/index.html), a hotel booking API, built with **Postman** and run from the command line with **Newman**.

## What's covered
| Area | Tests |
|---|---|
| Health | API is up |
| Auth | valid token, wrong password |
| Booking CRUD | create, get by ID, search, full update (PUT), partial update (PATCH), delete, get after delete |
| Security | update/delete without token, invalid token |
| Validation | missing required fields |
| Performance | every response under 3000 ms |

**15 requests · 43 assertions**, including:
- **Status code** checks on every request
- **JSON Schema validation** of booking responses (types, required fields, date format)
- **Data validation**: what the API returns matches what was sent
- **Request chaining**: the token and booking ID from one request feed the next
- **Dynamic test data**: random names and prices generated in a pre-request script

Manual test cases: [`docs/test_cases.csv`](docs/test_cases.csv)

## Known API issues found
| ID | Issue | Expected | Actual |
|---|---|---|---|
| BUG-001 | Wrong password accepted with success status | 401 Unauthorized | 200 OK |
| BUG-002 | Delete returns "Created" | 200 OK / 204 No Content | 201 Created |
| BUG-003 | Missing fields crash the server | 400 Bad Request | 500 Internal Server Error |

## Project structure
```
collections/    # Postman collection (requests + test scripts)
environments/   # Environment variables (base URL, credentials)
docs/           # Test cases, bug reports
reports/        # HTML report (generated)
```

## Run in Postman
1. Import `collections/restful-booker.postman_collection.json` and `environments/restful-booker.postman_environment.json`
2. Select the **Restful Booker** environment (top right)
3. Right-click the collection → **Run collection** → **Run**

## Run from the command line
```bash
npm install
npm test
```
Report: `reports/report.html`

## CI
GitHub Actions runs the full collection with Newman on every push (`.github/workflows/api-tests.yml`) and uploads the HTML report.
