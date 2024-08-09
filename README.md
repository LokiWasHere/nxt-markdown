# API Specification: NXT Reseller Line Management System

## 1. Authentication (private):

- [x] Master API Key:
  - [x] Purpose: Primary key for overarching system access.

## 2. Sub API Key Management (private):

- [x] Create Sub API Key:
  - [x] Purpose: Generate a secondary API key for finer-grained access.
- [x] List Sub API Key:
  - [x] Purpose: List all API keys created for this user.
- [x] Remove Sub API Key:
  - [x] Purpose: Invalidate and delete a previously generated Sub API key.

## 3. Credits System:

- [x] Get Credits:
  - [x] Purpose: Retrieve the current number of credits available to the authenticated entity.

## 4. Line Management:

- [x] Get Active Lines:
  - [x] Purpose: Fetch a list of all currently active lines.
- [x] Get Line info:
  - [x] Purpose: Allows to fetch all details associated with a specific line.
- [x] Create Line:
  - [x] Normal:
    - [x] Action: Create a regular line and deducts corresponding credits based on chosen duration.
  - [x] Trial:
    - [x] Requirements: Must have at least x number of credits.
    - [x] Action: Create a trial line but does not deduct any credits.
- [x] Link MAG to Line:
  - [x] Purpose: Associate a MAG device with a specific line.
- [x] Manage Line:
  - [x] Username: Modify the username associated with a line.
  - [x] Password: Update or reset the password for a line.
  - [x] Packages: Manage the packages or services linked to a line.
  - [x] Suspend: Temporarily disable access for a line.
  - [x] Remove: Permanently delete a line from the system.
- [x] Upgrade Line:
  - [x] Extra Connection:
    - [x] Action: Deducts credits on a prorata basis based on line expiration date.
- [x] Extend Line:
  - [x] Action: Deducts relevant credits based on chosen extension duration.

## 5. Credit Deduction Guide (variables) (private):

- [x] New Line Creation & Extensions:
  - [x] 1 month = 5 credits
  - [x] 3 months = 10 credits
  - [x] 6 months = 15 credits
  - [x] 12 months = 25 credits
- [x] Extra Connections:
  - [x] Expiration ≤ 30 days: 1 Extra connection = 4 credits (prorated)
  - [x] 30 days < Expiration ≤ 90 days: 1 Extra connection = 7 credits (prorated)
  - [x] 90 days < Expiration ≤ 180 days: 1 Extra connection = 12 credits (prorated)
  - [x] Expiration > 180 days: 1 Extra connection = 20 credits (prorated)
- [x] Trial line:
  - [x] 12 hours = 0 credits
  - [x] 24 hours = 0 credits
  - [x] 48 hours = 0 credits

## 6. Rate Limiting:

- [x] Purpose: Prevent abuse of the API and ensure fair usage.
- [x] Implementation:
  - [x] Rate limits are applied per API key.
  - [x] Limits are set for each endpoint separately.
  - [x] Example limits:
    - [x] Authentication attempts: 10 per minute
    - [x] Get requests: 100 per minute
    - [x] Post/Put/Delete requests: 50 per minute
- [x] Response:
  - [x] When rate limit is exceeded, return 429 (Too Many Requests) status code.
  - [x] Include headers:
    - [x] X-RateLimit-Limit: maximum number of requests per time window
    - [x] X-RateLimit-Remaining: number of requests left for the time window
    - [x] X-RateLimit-Reset: the remaining window before the rate limit resets in UTC epoch seconds

## 7. Audit Trail:

- [ ] Purpose: Track all actions performed through the API for security and accountability.
- [ ] Implementation:
  - [ ] Log every API call with the following details:
    - [ ] Timestamp
    - [ ] API Key used
    - [ ] IP Address
    - [ ] Endpoint accessed
    - [ ] HTTP method
    - [ ] Request parameters
    - [ ] Response status code
  - [ ] Ensure audit logs are stored securely and retained

## 8. API Documentation:

- [x] Purpose: Provide comprehensive guidance for developers integrating with the API.
- [x] Implementation:
  - [x] Include for each endpoint:
    - [x] URL
    - [x] HTTP method
    - [x] Request parameters
    - [x] Request body schema (if applicable)
    - [x] Response schema
    - [x] Example requests and responses
    - [x] Possible error codes and their meanings
  - [x] Provide a "Getting Started" guide covering:
    - [x] Authentication process
    - [x] Rate limiting rules
    - [x] Best practices for API usage
  - [x] Include a changelog to track API updates and versioning
  - [x] ~~Provide SDK examples in popular programming languages (e.g., Python, JavaScript, PHP)~~
  - [x] Host the documentation on a dedicated developer portal

## General Guidelines & Requirements:

- [x] Prorated credits are calculated based of the highest value then divided by X day and rounded to the highest credit. using a whole number. (exemple 1: a line with 7 days to expiration: (4/30)*7 = 0,91 the highest whole number is 1. exemple 2: a line with 234 days to expiration: (20/365)*234 = 12,82 the highest whole number is 13)
- [x] Credit values for each action should be retrieved from a reliable source, such as a database or the .env file of the API project.
- [x] All actions that deduct credits should check for sufficient balance. If the balance is insufficient, the action should be refused.
- [x] User can only make actions on lines that has his member_id, this is very important to avoid access to lines or other users.
- [x] The API structure should take in consideration that in the next version each reseller can have sub-resellers, Basicaly sub-reseller should have the same actions than the reseller, however the main reseller can transfer credit to his sub-resellers.
- [x] If the user tries to create a line with a duration that do not match the list above, then give error message.
- [ ] Provide clear definitions of error codes/messages for common issues, such as insufficient credits, invalid API keys, system errors, etc.
- [x] Ensure strict handling by the API for bad or incomplete requests.
- [x] Use versionning if needed like /v1/lines/active and /v2/lines/active.
- [ ] Logs & Errors need to be stored and well structured.
- [x] A trial line can be upgrade/extended to official line.
- [x] A trial line cannot add extra connections.
- [x] Downgrade of extra connections can be done, it will always cost 0 credits however no credits will be refunded.
- [x] You should always take in consideration that the extention of a line with extra-connection have to be calculated in the correct way.
- [x] Transactional Integrity: Ensure a transactional mechanism to avoid credit inconsistencies or losses. For MySQL, use operations like BEGIN; and COMMIT; to handle transactions securely.
- [x] Rate limiting should be carefully tuned based on system capacity and adjusted as needed.
- [ ] Audit logs should be regularly backed up and protected against unauthorized access or modification.
- [x] API documentation should be kept up-to-date with each change or new release of the API.
- [ ] Consider providing a sandbox environment for developers to test API integrations without affecting production data.
