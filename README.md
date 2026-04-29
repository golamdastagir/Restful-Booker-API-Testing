# Restful Booker API Testing with Postman & Newman

## Overview

This project demonstrates API testing using **Postman** and **Newman** on the [Restful Booker API](https://restful-booker.herokuapp.com) — a live booking management system. It covers the full CRUD lifecycle, response validation, dynamic test data generation, and automated HTML report generation.

| | |
|---|---|
| **API Documentation** | [Postman API Documentation](https://documenter.getpostman.com/view/54244746/2sBXqJKgFi) |
| **Live Test Report** | [Newman Report](https://golamdastagir.github.io/Restful-Booker-API-Testing/Reports/RestfulBooker-2026-04-23-06-40-42-162-0.html) |

---

## Features

- Tests for GET, POST, PUT, DELETE requests
- Assertions for status codes and response body validation
- Dynamic test data using Postman built-in variables (`{{$randomFirstName}}`, `{{$randomInt}}`, etc.)
- Environment variables for flexible configuration
- Pre-request scripts for automated data setup
- Automated HTML report generation via Newman HTMLExtra

---

## Technology Used

- **Postman** — API testing and collection management
- **Newman** — Command-line collection runner
- **Newman HTMLExtra** — HTML report generation

---

## Prerequisites

- Node.js
- Newman — `npm install -g newman`
- Newman HTMLExtra — `npm install -g newman-reporter-htmlextra`

---

## Installation

```bash
git clone https://github.com/golamdastagir/Restful-Booker-API-Testing.git
```

Then import the collection and environment files into Postman.

---

## Usage

**Console run:**
```bash
newman run RestfulBooker.postman_collection.json -e RestfulBooker.postman_environment.json
```

**Generate HTML report:**
```bash
newman run RestfulBooker.postman_collection.json -e RestfulBooker.postman_environment.json -r cli,htmlextra
```

---

## Test Case Scenarios

### 1. Create Booking
- **Method:** POST
- **URL:** `https://restful-booker.herokuapp.com/booking`
- **Description:** Creates a new booking record with dynamically generated test data. Returns a `bookingid` used in all subsequent requests.
- **Pre-request Script:** Generates random values for `firstname`, `lastname`, `totalprice`, `depositpaid`, `checkin`, `checkout`, and `additionalneeds` using Postman dynamic variables and stores them as environment variables.
- **Assertions:** Status code is 200, response contains `bookingid`, response body matches submitted data.

### 2. Get Booking by ID
- **Method:** GET
- **URL:** `https://restful-booker.herokuapp.com/booking/{{bookingid}}`
- **Description:** Retrieves the booking record created in the previous step to verify it was persisted correctly.
- **Assertions:** Status code is 200, response fields match the values set during CreateBooking.

### 3. Create Authentication Token
- **Method:** POST
- **URL:** `https://restful-booker.herokuapp.com/auth`
- **Description:** Authenticates using admin credentials and returns a token required for update and delete operations. The token is stored as an environment variable for use in subsequent requests.
- **Assertions:** Status code is 200, response contains a valid `token`.

### 4. Update Booking
- **Method:** PUT
- **URL:** `https://restful-booker.herokuapp.com/booking/{{bookingid}}`
- **Description:** Replaces all fields of the existing booking with newly generated data. Requires the authentication token passed as a `Cookie` header.
- **Pre-request Script:** Regenerates all booking fields using Postman dynamic variables.
- **Assertions:** Status code is 200, response body reflects the updated values.

### 5. Verify Updated Booking
- **Method:** GET
- **URL:** `https://restful-booker.herokuapp.com/booking/{{bookingid}}`
- **Description:** Retrieves the booking after the update to confirm all changes were correctly persisted.
- **Assertions:** Status code is 200, response fields match the values from the UpdateBooking request.

### 6. Delete Booking
- **Method:** DELETE
- **URL:** `https://restful-booker.herokuapp.com/booking/{{bookingid}}`
- **Description:** Permanently removes the booking record from the system. Requires the authentication token passed as a `Cookie` header.
- **Assertions:** Status code is 201.

### 7. Verify Booking Deletion
- **Method:** GET
- **URL:** `https://restful-booker.herokuapp.com/booking/{{bookingid}}`
- **Description:** Attempts to retrieve the deleted booking to confirm it no longer exists in the system.
- **Assertions:** Status code is 404.

---

## Author

**Golam Dastagir** — Aspiring QA Engineer

This project was built as a hands-on exercise in API testing, focusing on real-world practices including dynamic data handling, authentication flows, and automated reporting.
