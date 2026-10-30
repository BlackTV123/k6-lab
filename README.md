# k6-lab
# K6 lab results

## Part 0

- Step 0.1, http_req_duration: 266.58 ms

- Step 0.2: 80 requests, 3.803281 requests/s, p(95) = 277.65 ms.
  The rate is below 5 requests/s because each virtual user waits
  for the response and then sleeps for 1 second before repeating.
  The average iteration duration was 1.29 seconds.

- Step 0.6:
  - Normal run: p(95) = 369.4 ms, all thresholds passed,
    exit code = 0.
  - P95=300 run: p(95) = 390.9 ms, response-time threshold
    failed, exit code = 99.
  - Checks passed 100% in both runs, but the second test
    failed because its response time exceeded the threshold.

- Step 0.7:
  - Original requirement: 3 VUs for 30 seconds,
    p(95)<1000 ms, p(99)<1500 ms, and failure rate<1%.
  - All thresholds passed: p(95)=392.92 ms,
    p(99)=447.25 ms, failure rate=0%.
  - Changed the p(95) requirement from 1000 ms to 100 ms.
  - Modified run: p(95)=383.61 ms, p(99)=432.07 ms,
    failure rate=0%.
  - The p(95)<100 threshold failed; exit code=99.