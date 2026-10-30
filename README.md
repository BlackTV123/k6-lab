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

- Step 0.8:
  - The request used lowercase 'pepperoni' and 'knife',
    but the API expects the case-sensitive names
    'Pepperoni' and 'Knife'.
  - Before the fix: the no-pepperoni check failed 3/75 times,
    and the no-knife check failed 31/75 times.
  - After the fix: all 225 checks passed across 75 requests.
  - Repeated requests exposed the problem because random
    recommendations sometimes included the unwanted items.
    One manual request could miss it by chance.
  - Status 200 alone did not guarantee a correct recommendation.

## Part A results

| Step | Test type | Users | Req/s | p(95) | Max | Failed | Checks |
| ---- | --------- | ----- | ----- | ----- | --- | ------ | ------ |
| A1 | Smoke | 1 | 0.477173 | 369.95 ms | 388.38 ms | 0.00% | 100.00% |
| A2 | Average load | 3 | 1.284898 | 358.6 ms | 378.85 ms | 0.00% | 100.00% |
| A3 | Stress | 5 | 2.138237 | 365.11 ms | 492.01 ms | 0.00% | 100.00% |
| A4 | Spike | 5 | 1.048369 | 362.85 ms | 376.08 ms | 0.00% | 100.00% |
| A5 | Soak | 2 | 0.97777 | 372 ms | 391.48 ms | 0.00% | 100.00% |

### A4 spike observation

During the spike, p(95) increased modestly to approximately
350–370 ms. After the load returned to 1 VU at around
00:25:49, p(95) returned to approximately 320 ms by 00:26:19.
The estimated recovery time was about 30 seconds.
This estimate is approximate because the pre-spike response
times fluctuated. No requests failed, and all checks passed.

### A5 soak observation

Response times fluctuated but showed no sustained upward
trend from the first minutes to the last minutes.
Average response times stayed around 275–300 ms, while
p(95) fluctuated around 310–380 ms.
Across the entire run, p(95) was 372 ms, the failure rate
was 0%, and all checks passed.

No performance degradation was observed during this
11-minute test. However, this short test cannot rule out
long-term problems. A soak test lasting several hours
could reveal a memory leak that gradually consumes
memory and causes slower responses or failures.