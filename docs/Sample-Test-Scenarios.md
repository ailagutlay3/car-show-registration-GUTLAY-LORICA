# Car Show Registration System
## Sample Test Scenarios – Manual API Testing

**Testing Tool:** Scalar API Reference  
**Base URL:** `http://localhost:5205`  
**Database:** SQLite (`carshow.db`)

### Test Results

| Case | Test Scenario | Expected Result | Actual Result | Status |
|---|---|---|---|---|
| 01 | Create Competitor registration with partial payment | 201 Created | Registration created, Partial payment | PASS |
| 02 | Create Exhibitor registration with no payment | 201 Created | Registration created, Unpaid | PASS |
| 03 | Create VIP registration with full payment | 201 Created | Registration created, Paid | PASS |
| 04 | Check automatic registration numbers | Sequential numbers | CS27-0027, 0028, 0029 | PASS |
| 05 | Attempt to set server-controlled fields | Server values should be used | Server-generated values retained | PASS |
| 06 | Submit empty required fields | 400 Bad Request | Validation errors returned | PASS |
| 07 | Submit invalid contact number | 400 Bad Request | Contact number validation error | PASS |
| 08 | Submit invalid email | 400 Bad Request | Email validation error | PASS |
| 09 | Amount paid exceeds registration fee | 400 Bad Request | Invalid amount error returned | PASS |
| 10 | Submit negative amount paid | 400 Bad Request | Amount validation error returned | PASS |
| 11 | Submit invalid registration type | 400 Bad Request | Registration type validation error returned | PASS |
| 12 | Submit participant name exceeding 150 characters | 400 Bad Request | Length validation error returned | PASS |
| 13 | Submit duplicate plate number | 409 Conflict | Duplicate plate error returned | PASS |
| 14 | Get registrations with default pagination | 200 OK, page 1, page size 10 | Returned 31 registrations, 10 per page | PASS |
| 15 | Get registrations sorted newest first | Newest registrations first | CS27-0031 through CS27-0026 appeared first | PASS |
| 16 | Get page 2 with page size 5 | 200 OK | verified | PASS |
| 17 | Request page 999 | Empty items, total count unchanged | verified | PASS |
| 18 | Request page 0 | 400 Bad Request | verified | PASS |
| 19 | Request page size 100 | 400 Bad Request | Not yet verified | PENDING |
| 20 | Filter registrations by VIP type | Only VIP registrations returned | 8 VIP registrations returned | PASS |
| 21 | Filter registrations by Confirmed status | Only Confirmed registrations returned | 10 Confirmed registrations returned | PASS |
| 22 | Search participant name using lowercase `juan` | Matching names returned | 5 matching registrations returned | PASS |
| 23 | Search participant name using uppercase `JUAN` | Case-insensitive matching | Not yet verified | PENDING |
| 24 | Search participant name using `dela` | Matching names returned | Not yet verified | PENDING |
| 25 | Combine type, status, and name filters | All filters applied | Not yet verified | PENDING |
| 26 | Search a name with no matches | Empty items returned | Not yet verified | PENDING |
| 27 | Filter using invalid registration type | 400 Bad Request | Not yet verified | PENDING |
| 28 | Filter using invalid status | 400 Bad Request | Not yet verified | PENDING |
| 29 | Get an existing registration by ID | 200 OK | CS27-0027 returned | PASS |
| 30 | Get a nonexistent registration ID | 404 Not Found | Registration not found error returned | PASS |
| 31 | Get registration using invalid GUID | 404 Not Found | Not yet verified | PENDING |
| 32 | Get registration summary | 200 OK with summary totals | 31 registrations and summary totals returned | PASS |
| 33 | Verify summary status counts | Status counts equal total registrations | 10 + 6 + 12 + 3 = 31 | PASS |
| 34 | Compare summary total with list total | Both totals should match | Both returned 31 | PASS |
| 35 | Verify uncollected payments | Fees minus payments | ₱80,500 − ₱50,000 = ₱30,500 | PASS |
| 36 | Verify cancelled registrations excluded from financial totals | Cancelled registrations excluded | Not yet verified | PENDING |
| 37 | Create another registration and verify summary updates | Summary totals increase accordingly | Not yet verified | PENDING |
| 38 | Access `/api/registrations/summary` | Summary endpoint returns 200 OK | Summary returned successfully | PASS |
| 39 | Restart API and verify database persistence | Existing records remain after restart | Total count remained 31; CS27-0027 was still accessible | PASS |
| 40 | Verify unique database indexes | Unique indexes on PlateNumber and RegistrationNumber | Both unique indexes found in InitialCreate migration | PASS |

### Database Verification

- SQLite database: `backend/Api/carshow.db`
- Registrations table contains 31 records.
- Unique index on `PlateNumber`.
- Unique index on `RegistrationNumber`.
- Database records remained available after restarting the API.

### Notes

Cases marked **PENDING** still need to be executed and verified. Do not mark them as PASS until the actual API response matches the expected result.