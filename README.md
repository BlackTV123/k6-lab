# k6-lab
# K6 lab results

## Part 0

- Step 0.1, http_req_duration: 266.58 ms

- Step 0.2: 80 requests, 3.803281 requests/s, p(95) = 277.65 ms.
  The rate is below 5 requests/s because each virtual user waits
  for the response and then sleeps for 1 second before repeating.
  The average iteration duration was 1.29 seconds.