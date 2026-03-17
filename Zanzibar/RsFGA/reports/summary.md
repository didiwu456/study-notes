ywu@MSI:/mnt/d/GitHub/rsfga/load-tests/scripts$ bash run-suite.sh check-direct
[INFO] k6 version: k6 v1.6.1 (commit/devel, go1.26.0, linux/amd64)
[INFO] Checking RSFGA server at http://localhost:8080...
[SUCCESS] Server is healthy
[INFO] Configuration:
  Server URL: http://localhost:8080
  Reports: /mnt/d/GitHub/rsfga/load-tests/scripts/../reports

[INFO] Running scenario: check-direct
Command: k6 run -e RSFGA_URL=http://localhost:8080 --out json=/mnt/d/GitHub/rsfga/load-tests/scripts/../reports/check-direct_20260317_234213.json /mnt/d/GitHub/rsfga/load-tests/scripts/../k6/scenarios/check-direct.js


         /\      Grafana   /‾‾/
    /\  /  \     |\  __   /  /
   /  \/    \    | |/ /  /   ‾‾\
  /          \   |   (  |  (‾)  |
 / __________ \  |_|\_\  \_____/


     execution: local
        script: /mnt/d/GitHub/rsfga/load-tests/scripts/../k6/scenarios/check-direct.js
        output: json (/mnt/d/GitHub/rsfga/load-tests/scripts/../reports/check-direct_20260317_234213.json)

     scenarios: (100.00%) 1 scenario, 200 max VUs, 5m30s max duration (incl. graceful stop):
              * check_direct: 100.00 iterations/s for 5m0s (maxVUs: 50-200, gracefulStop: 30s)

INFO[0000] Created store: 01KKY7CFCRA8A8BCMEQNSHS71Q     source=console
INFO[0000] Created model: 01KKY7CFEFJNC3TGBK9DKGDA6H     source=console
INFO[0005] Wrote 10000 tuples                            source=console
INFO[0308] Deleted store: 01KKY7CFCRA8A8BCMEQNSHS71Q     source=console


  █ THRESHOLDS

    cache_hit_rate
    ✗ 'rate>0.9' rate=0.00%

    check_latency
    ✗ 'p(95)<5' p(95)=30.68ms
    ✗ 'p(99)<20' p(99)=58.47ms

    error_rate
    ✓ 'rate<0.001' rate=0.00%

    http_req_duration{endpoint:check}
    ✗ 'p(95)<5' p(95)=30.68ms
    ✗ 'p(99)<20' p(99)=58.47ms

    http_req_failed{endpoint:check}
    ✓ 'rate<0.001' rate=0.00%


  █ TOTAL RESULTS

    CUSTOM
    cache_hit_rate.................: 0.00%  0 out of 30001
    check_allowed_rate.............: 0.00%  0 out of 30001
    check_count....................: 30001  94.22904/s
    check_latency..................: avg=21.46ms min=13.43ms med=18.67ms max=595.31ms p(90)=26.2ms  p(95)=30.68ms
    error_rate.....................: 0.00%  0 out of 30001

    HTTP
    http_req_duration..............: avg=21.64ms min=9.59ms  med=18.68ms max=2.54s    p(90)=26.37ms p(95)=31.04ms
      { endpoint:check }...........: avg=21.46ms min=13.43ms med=18.67ms max=595.31ms p(90)=26.2ms  p(95)=30.68ms
      { expected_response:true }...: avg=21.64ms min=9.59ms  med=18.68ms max=2.54s    p(90)=26.37ms p(95)=31.04ms
    http_req_failed................: 0.00%  0 out of 30104
      { endpoint:check }...........: 0.00%  0 out of 30001
    http_reqs......................: 30104  94.552549/s

    EXECUTION
    iteration_duration.............: avg=71.68ms min=15.4ms  med=71.41ms max=338.18ms p(90)=111.8ms p(95)=116.78ms
    iterations.....................: 30001  94.22904/s
    vus............................: 0      min=0          max=11
    vus_max........................: 50     min=50         max=50

    NETWORK
    data_received..................: 3.8 MB 12 kB/s
    data_sent......................: 9.9 MB 31 kB/s




running (5m18.4s), 000/050 VUs, 30001 complete and 0 interrupted iterations
check_direct ✓ [======================================] 000/050 VUs  5m0s  100.00 iters/s
ERRO[0308] thresholds on metrics 'cache_hit_rate, check_latency, http_req_duration{endpoint:check}' have been crossed
[ERROR] Scenario check-direct failed
ywu@MSI:/mnt/d/GitHub/rsfga/load-tests/scripts$ bash run-suite.sh check-userset-scale
[INFO] k6 version: k6 v1.6.1 (commit/devel, go1.26.0, linux/amd64)
[INFO] Checking RSFGA server at http://localhost:8080...
[SUCCESS] Server is healthy
[INFO] Configuration:
  Server URL: http://localhost:8080
  Reports: /mnt/d/GitHub/rsfga/load-tests/scripts/../reports

[INFO] Running scenario: check-userset-scale
Command: k6 run -e RSFGA_URL=http://localhost:8080 --out json=/mnt/d/GitHub/rsfga/load-tests/scripts/../reports/check-userset-scale_20260317_235629.json /mnt/d/GitHub/rsfga/load-tests/scripts/../k6/scenarios/check-userset-scale.js


         /\      Grafana   /‾‾/
    /\  /  \     |\  __   /  /
   /  \/    \    | |/ /  /   ‾‾\
  /          \   |   (  |  (‾)  |
 / __________ \  |_|\_\  \_____/


     execution: local
        script: /mnt/d/GitHub/rsfga/load-tests/scripts/../k6/scenarios/check-userset-scale.js
        output: json (/mnt/d/GitHub/rsfga/load-tests/scripts/../reports/check-userset-scale_20260317_235629.json)

     scenarios: (100.00%) 2 scenarios, 180 max VUs, 6m0s max duration (incl. graceful stop):
              * low_userset: 50.00 iterations/s for 5m0s (maxVUs: 30-100, gracefulStop: 30s)
              * high_userset: 20.00 iterations/s for 5m0s (maxVUs: 20-80, startTime: 30s, gracefulStop: 30s)

INFO[0000] Created store: 01KKY86K19RYKXB7DC60D9DB1P     source=console
INFO[0000] Created model: 01KKY86K1GCZS9ZJ82JSVV9W52     source=console
INFO[0000] Setup: 500 groups × 50 members = 25000 member users  source=console
INFO[0000] Each document will have 100–1000 viewer_group tuples  source=console
INFO[0000] Writing 25000 membership tuples...            source=console
INFO[0014] Writing 61590 viewer_group tuples...          source=console
INFO[0046] Setup complete. Total tuples written: 86590   source=console
WARN[0050] Insufficient VUs, reached 100 active VUs and cannot initialize more  executor=constant-arrival-rate scenario=low_userset
WARN[0083] Insufficient VUs, reached 80 active VUs and cannot initialize more  executor=constant-arrival-rate scenario=high_userset
INFO[0401] Deleted store: 01KKY86K19RYKXB7DC60D9DB1P     source=console


  █ THRESHOLDS

    error_rate
    ✗ 'rate<0.01' rate=90.67%

    http_req_failed
    ✗ 'rate<0.01' rate=62.21%

    userset_check_allowed_rate
    ✗ 'rate>0.3' rate=8.63%

    userset_check_latency{userset_tier:high}
    ✗ 'p(95)<500' p(95)=32.02s
    ✗ 'p(99)<1000' p(99)=32.18s

    userset_check_latency{userset_tier:low}
    ✗ 'p(95)<100' p(95)=32.02s
    ✗ 'p(99)<200' p(99)=32.45s


  █ TOTAL RESULTS

    CUSTOM
    error_rate.....................: 90.67% 1722 out of 1899
    userset_check_allowed_rate.....: 8.63%  164 out of 1899
    userset_check_count............: 1899   4.569927/s
    userset_check_latency..........: avg=30.24s     min=1.55s  med=31.48s  max=32.74s p(90)=31.87s p(95)=32.02s
      { userset_tier:high }........: avg=30.33s     min=1.55s  med=31.48s  max=32.64s p(90)=31.87s p(95)=32.02s
      { userset_tier:low }.........: avg=30.16s     min=1.63s  med=31.47s  max=32.74s p(90)=31.88s p(95)=32.02s
    userset_negative_latency.......: avg=31.47s     min=15.58s med=31.49s  max=32.74s p(90)=31.9s  p(95)=32.05s
    userset_positive_latency.......: avg=29.08s     min=1.55s  med=31.47s  max=32.64s p(90)=31.85s p(95)=32.01s
    userset_size...................: avg=618.695629 min=102    med=669     max=1000   p(90)=917    p(95)=970

    HTTP
    http_req_duration..............: avg=20.76s     min=6.32ms med=31.43s  max=32.74s p(90)=31.79s p(95)=31.96s
      { expected_response:true }...: avg=2.97s      min=6.32ms med=41.02ms max=31.16s p(90)=15.04s p(95)=24.02s
    http_req_failed................: 62.21% 1722 out of 2768
    http_reqs......................: 2768   6.661168/s

    EXECUTION
    dropped_iterations.............: 19098  45.959168/s
    iteration_duration.............: avg=29.28s     min=1.56s  med=30.48s  max=31.67s p(90)=30.85s p(95)=31.01s
    iterations.....................: 1899   4.569927/s
    vus............................: 0      min=0            max=180
    vus_max........................: 180    min=50           max=180

    NETWORK
    data_received..................: 425 kB 1.0 kB/s
    data_sent......................: 7.5 MB 18 kB/s




running (6m55.5s), 000/180 VUs, 1899 complete and 3 interrupted iterations
low_userset  ✓ [======================================] 003/100 VUs  5m0s  50.00 iters/s
high_userset ✓ [======================================] 00/80 VUs    5m0s  20.00 iters/s
ERRO[0401] thresholds on metrics 'error_rate, http_req_failed, userset_check_allowed_rate, userset_check_latency{userset_tier:high}, userset_check_latency{userset_tier:low}' have been crossed
[ERROR] Scenario check-userset-scale failed
ywu@MSI:/mnt/d/GitHub/rsfga/load-tests/scripts$ bash run-suite.sh check-userset-small
[INFO] k6 version: k6 v1.6.1 (commit/devel, go1.26.0, linux/amd64)
[INFO] Checking RSFGA server at http://localhost:8080...
[SUCCESS] Server is healthy
[INFO] Configuration:
  Server URL: http://localhost:8080
  Reports: /mnt/d/GitHub/rsfga/load-tests/scripts/../reports

[INFO] Running scenario: check-userset-small
Command: k6 run -e RSFGA_URL=http://localhost:8080 --out json=/mnt/d/GitHub/rsfga/load-tests/scripts/../reports/check-userset-small_20260318_002036.json /mnt/d/GitHub/rsfga/load-tests/scripts/../k6/scenarios/check-userset-small.js


         /\      Grafana   /‾‾/
    /\  /  \     |\  __   /  /
   /  \/    \    | |/ /  /   ‾‾\
  /          \   |   (  |  (‾)  |
 / __________ \  |_|\_\  \_____/


     execution: local
        script: /mnt/d/GitHub/rsfga/load-tests/scripts/../k6/scenarios/check-userset-small.js
        output: json (/mnt/d/GitHub/rsfga/load-tests/scripts/../reports/check-userset-small_20260318_002036.json)

     scenarios: (100.00%) 3 scenarios, 300 max VUs, 10m30s max duration (incl. graceful stop):
              * low_rate: 100.00 iterations/s for 3m0s (maxVUs: 30-80, gracefulStop: 30s)
              * mid_rate: 200.00 iterations/s for 3m0s (maxVUs: 60-150, startTime: 3m30s, gracefulStop: 30s)
              * high_rate: 400.00 iterations/s for 3m0s (maxVUs: 100-300, startTime: 7m0s, gracefulStop: 30s)

INFO[0000] Created store: 01KKY9JS7657JYG2XYBZKBH1CW     source=console
INFO[0000] Created model: 01KKY9JS7E7CXE1143WJ5N9SJF     source=console
INFO[0000] 300 groups × 50 members = 15000 users         source=console
INFO[0000] 200 documents, 50–100 groups each             source=console
INFO[0000] Writing 15000 membership tuples...            source=console
INFO[0005] Writing 15221 viewer_group tuples...          source=console
INFO[0012] Setup complete. Total: 30221 tuples           source=console
WARN[0014] Insufficient VUs, reached 80 active VUs and cannot initialize more  executor=constant-arrival-rate scenario=low_rate
WARN[0224] Insufficient VUs, reached 150 active VUs and cannot initialize more  executor=constant-arrival-rate scenario=mid_rate
WARN[0434] Insufficient VUs, reached 300 active VUs and cannot initialize more  executor=constant-arrival-rate scenario=high_rate
INFO[0634] Deleted store: 01KKY9JS7657JYG2XYBZKBH1CW     source=console


  █ THRESHOLDS

    error_rate
    ✗ 'rate<0.01' rate=29.39%

    http_req_failed
    ✗ 'rate<0.01' rate=27.76%

    userset_small_check_latency{rate_tier:high}
    ✗ 'p(95)<100' p(95)=32.18s
    ✗ 'p(99)<200' p(99)=32.48s

    userset_small_check_latency{rate_tier:low}
    ✗ 'p(95)<30' p(95)=18.38s
    ✗ 'p(99)<60' p(99)=20.15s

    userset_small_check_latency{rate_tier:mid}
    ✗ 'p(95)<50' p(95)=31.16s
    ✗ 'p(99)<100' p(99)=31.39s


  █ TOTAL RESULTS

    CUSTOM
    error_rate.......................: 29.39% 1530 out of 5205
    userset_small_allowed_rate.......: 42.01% 2187 out of 5205
    userset_small_check_count........: 5205   7.950575/s
    userset_small_check_latency......: avg=19.8s    min=472.96ms med=19.37s max=32.96s p(90)=31.98s p(95)=32.11s
      { rate_tier:high }.............: avg=26.92s   min=1.25s    med=31.45s max=32.96s p(90)=32.13s p(95)=32.18s
      { rate_tier:low }..............: avg=10.74s   min=472.96ms med=11.15s max=21.59s p(90)=17.1s  p(95)=18.38s
      { rate_tier:mid }..............: avg=17.94s   min=935.5ms  med=18.93s max=31.86s p(90)=29.53s p(95)=31.16s
    userset_small_negative_latency...: avg=24.55s   min=5.26s    med=27.9s  max=32.96s p(90)=32.06s p(95)=32.15s
    userset_small_positive_latency...: avg=14.96s   min=472.96ms med=12.56s max=32.96s p(90)=31.58s p(95)=31.98s
    userset_small_size...............: avg=76.43804 min=50       med=76     max=100    p(90)=97     p(95)=99

    HTTP
    http_req_duration................: avg=18.7s    min=6.82ms   med=18.28s max=32.96s p(90)=31.95s p(95)=32.1s
      { expected_response:true }.....: avg=13.7s    min=6.82ms   med=13.48s max=32.63s p(90)=26s    p(95)=28.87s
    http_req_failed..................: 27.76% 1530 out of 5511
    http_reqs........................: 5511   8.417987/s

    EXECUTION
    dropped_iterations...............: 120797 184.515977/s
    iteration_duration...............: avg=19.18s   min=492.27ms med=18.69s max=32.17s p(90)=30.87s p(95)=30.94s
    iterations.......................: 5205   7.950575/s
    vus..............................: 0      min=0            max=300
    vus_max..........................: 300    min=100          max=300

    NETWORK
    data_received....................: 765 kB 1.2 kB/s
    data_sent........................: 4.0 MB 6.1 kB/s




running (10m54.7s), 000/300 VUs, 5205 complete and 0 interrupted iterations
low_rate  ✓ [======================================] 00/80 VUs    3m0s  100.00 iters/s
mid_rate  ✓ [======================================] 000/150 VUs  3m0s  200.00 iters/s
high_rate ✓ [======================================] 000/300 VUs  3m0s  400.00 iters/s
ERRO[0635] thresholds on metrics 'error_rate, http_req_failed, userset_small_check_latency{rate_tier:high}, userset_small_check_latency{rate_tier:low}, userset_small_check_latency{rate_tier:mid}' have been crossed
[ERROR] Scenario check-userset-small failed