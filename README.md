# Restful Booker Performance Test

This repository contains an Apache JMeter performance test plan for the [Restful Booker API](https://restful-booker.herokuapp.com/). 

## Test Scenario Overview
The test plan is built using JMeter 5.6.3 and simulates 20 concurrent users with a 1-second ramp-up time. The test executes a single loop per user and follows this sequence:

1. **Authentication (`POST /auth`)**: Submits static credentials to generate an auth token, which is extracted via a JSON PostProcessor and passed to subsequent requests using a Cookie header.
2. **Create Booking (`POST /booking`)**: Uses external CSV test data (`booking_guests.csv`) to dynamically populate the `firstname` and `lastname` fields. The generated `bookingid` is extracted for downstream requests.
3. **Retrieve Booking (`GET /booking/${bookingid}`)**: Fetches the newly created booking record to verify creation.
4. **Delete Booking (`DELETE /booking/${bookingid}`)**: Controlled by a Throughput Controller, this step executes for only 25% of the iterations and asserts a `201` HTTP response code upon completion.

## Global Timers & Assertions
* **Pacing**: A Gaussian Random Timer adds a 1500ms constant delay with a 100ms random variation between requests to simulate realistic think time.
* **Performance Baseline**: A global Duration Assertion ensures no request exceeds a 1500ms response time limit.

## Prerequisites
* Apache JMeter 5.6.3 or higher.

---

## Recommended Code Improvements (For local development)

To optimize the `.jmx` file for version control and CI/CD pipelines, consider making the following updates to your test plan:

* **Use Relative Paths for Test Data:** Change the hardcoded absolute path in the CSV Data Set Config to a relative path (`booking_guests.csv`)
