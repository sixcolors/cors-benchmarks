# Benchmarks comparing rs/cors and jub0bs/cors and gofiber/fiber/v2/middleware/cors

This repo is a fork of [jub0bs/cors-benchmarks](https://github.com/jub0bs/cors-benchmarks) with the addition of the gofiber/fiber/v2/middleware/cors middleware.

This repo contains benchmarks that compare the performance
of three CORS middleware libraries:

- the more popular [rs/cors](https://github.com/rs/cors) (v1.10.1), and
- the more modern and user-friendly [jub0bs/cors](https://github.com/jub0bs/cors) (v0.1.0).
- the Fiber CORS middleware [gofiber/fiber/middleware/cors](https://github.com/gofiber/fiber) (v2.52.4)

## Running the benchmarks

Run the following commands in your shell:

```shell
git clone https://github.com/sixcolors/cors-benchmarks
cd cors-benchmarks
go test -run ^$ -bench .
```

## Some results

Note: BenchmarkMiddleware/fiber_cors___malicious_ACRH_vs_preflight-24 is causing a panic in Fiber, I will investigate and fix it, then update the results.

I've slightly redacted the results below for better readability.
In particular, I've added a red dot next to cases where jub0bs/cors
fares worse than rs/cors, and a green dot otherwise.

```text
goos: darwin
goarch: amd64
pkg: github.com/jub0bs/cors-benchmarks
cpu: Intel(R) Core(TM) i7-6700HQ CPU @ 2.60GHz

no_CORS_________________________________-8       619 ns/op       1024 B/op    10 allocs/op

rs_cors_________________single_vs_actual-8       689 ns/op       1056 B/op    10 allocs/op
jub0bs_cors_____________single_vs_actual-8       689 ns/op       1056 B/op    10 allocs/op 🟢

rs_cors_______________multiple_vs_actual-8       657 ns/op       1056 B/op    10 allocs/op
jub0bs_cors___________multiple_vs_actual-8       654 ns/op       1056 B/op    10 allocs/op 🟢

rs_cors___________pathological_vs_actual-8       784 ns/op       1072 B/op    12 allocs/op
jub0bs_cors_______pathological_vs_actual-8       700 ns/op       1040 B/op    10 allocs/op 🟢

rs_cors___________________many_vs_actual-8       715 ns/op       1072 B/op    12 allocs/op
jub0bs_cors_______________many_vs_actual-8       651 ns/op       1040 B/op    10 allocs/op 🟢

rs_cors____________________any_vs_actual-8       662 ns/op       1056 B/op    10 allocs/op
jub0bs_cors________________any_vs_actual-8       609 ns/op       1040 B/op    10 allocs/op 🟢

rs_cors______________single_vs_preflight-8       527 ns/op        960 B/op     7 allocs/op
jub0bs_cors__________single_vs_preflight-8       479 ns/op        944 B/op     7 allocs/op 🟢

rs_cors____________multiple_vs_preflight-8       554 ns/op        960 B/op     7 allocs/op
jub0bs_cors________multiple_vs_preflight-8       481 ns/op        944 B/op     7 allocs/op 🟢

rs_cors________pathological_vs_preflight-8       546 ns/op        960 B/op     9 allocs/op
jub0bs_cors____pathological_vs_preflight-8       518 ns/op        928 B/op     7 allocs/op 🟢

rs_cors________________many_vs_preflight-8       510 ns/op        960 B/op     9 allocs/op
jub0bs_cors____________many_vs_preflight-8       447 ns/op        928 B/op     7 allocs/op 🟢

rs_cors_________________any_vs_preflight-8       551 ns/op        960 B/op     7 allocs/op
jub0bs_cors_____________any_vs_preflight-8       455 ns/op        944 B/op     7 allocs/op 🟢

rs_cors________________ACRH_vs_preflight-8       558 ns/op        984 B/op    10 allocs/op
jub0bs_cors____________ACRH_vs_preflight-8       470 ns/op        928 B/op     7 allocs/op 🟢

rs_cors______malicious_ACRH_vs_preflight-8  49326231 ns/op  121177083 B/op  1061 allocs/op 😱
jub0bs_cors__malicious_ACRH_vs_preflight-8       478 ns/op        928 B/op     7 allocs/op 🟢
```
