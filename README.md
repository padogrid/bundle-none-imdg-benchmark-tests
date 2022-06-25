# IMDG Product Benchmart Tests

This bundle provides step-by-step instructions for creating an local environment and conducting benchmark tests on all the PadoGrid supported IMDG products using the `perf_test` app.

## Installing Bundle

![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*Driven by PadoGrid*](https://github.com/padogrid)

```bash
install_bundle -download bundle-none-imdg-benchmkark-tests
```

## Use Case

As of writing, PadoGrid supports four (4) IMDG products: Coherence, Geode/GemFire, Hazelcast, and Redis. This bundle creates a local environment in which you can conduct benchmark tests on all of these products. The test results will be consolidated in CSV files which you can analyze using your favorite spreadsheet application.

PadoGrid is already equipped with a benchmark app called `perf_test`, with which let you create and run custom test cases without coding. Creating a test environment with `perf_test` is quite simple and straight forward. This bundle provides step-by-step instructions along with some useful test cases that you can run with `perf_test`.

![Benchmark Clusters](images/benchmark-clusters.png)

## Required Software

- Padogrid 0.9.18+
- Coherence 1.13.x, 1.14.x
- Geode 1.x, GemFire 9.x
- Hazelcast 3.x, 4.x, 5.x
- Redis 6.x, 7.x

## Bundle Contents

```console
apps
├── perf_test_coherence
├── perf_test_geode
├── perf_test_hazelcast
└── perf_test_redis
```

## Configuring Bundle Envrionment

Let's create clusters that you want to test. To be objective, each cluster should have the same number of members. The minimum number of members is six (6) due to the Redis minimum requirement of six (6) members per cluster with replicas of one (1).

```bash
# Add 4 to the default Coherence cluster
make_cluster -product coherence -cluster mycoherence
add_member -cluster mycoherence -count 4

# Add 4 to the default Hazelcast cluster
make_cluster -product hazelcast -cluster myhz
add_member -cluster myhz -count 4

# Add 4 to the default Geode/GemFire cluster
make_cluster -product geode -cluster mygeode
add_member -cluster mygeode -count 4

# The default Redis cluster in PadoGrid has 6 members
make_cluster -product redis -cluster myredis
```

## Startup Sequence

### 1. Coherence

- Start cluster

```bash
switch_cluster -cluster mycoherence
start_cluster
```

- Run `perf_test_coherence`

```bash
# Run test_group 5 times. Each run generates a file in results/.
cd_app perf_test_coherence/bin_sh
for i in $(seq 5); do ./test_group -run; done
```

- Stop cluster

```bash
stop_cluster
```

### 2. Geode/GemFire

- Start cluster

```bash
switch_cluster -cluster mygeode
start_cluster
```

- Run `perf_test_geode`

```bash
# Run test_group 5 times. Each run generates a file in results/.
cd_app perf_test_geode/bin_sh
for i in $(seq 5); do ./test_group -run; done
```

- Stop cluster

```bash
stop_cluster
```

### 3. Hazelcast

- Start cluster

```bash
switch_cluster -cluster myhz
start_cluster
```

- Run `perf_test_hazelcast`

```bash
# Run test_group 5 times. Each run generates a file in results/.
cd_app perf_test_hazelcast/bin_sh
for i in $(seq 5); do ./test_group -run; done
```

- Stop cluster

```bash
stop_cluster
```

### 4. Redis

- Start cluster

```bash
switch_cluster -cluster myredis
start_cluster
```

- Run `perf_test_geode`

```bash
# Run test_group 5 times. Each run generates a file in results/.
cd_app perf_test_redis/bin_sh
for i in $(seq 5); do ./test_group -run; done
```

- Stop cluster

```bash
stop_cluster
```

### 5. Consolidate test results

- Change directory to any of the `perf_test` apps and run `create_csv -all`.

```bash
cd_app perf_test_geode/bin_sh
./create_csv -all
```

Output:

```console
```

- Open the consolidated CSV file in your spreadsheet application.

```bash
# macOS
open /....
```

## Teardown

```bash
# Stop all running clusters in the workspace
stop_workspace -all
```

## References

1. Coherence `perf_test`, https://github.com/padogrid/padogrid/wiki/Coherence-perf_test-App
1. Geode/GemFire `perf_test`, https://github.com/padogrid/padogrid/wiki/Geode-perf_test-App
1. Hazelcast `perf_test`, https://github.com/padogrid/padogrid/wiki/Hazelcast-perf_test-App
1. Redis `perf_test`, https://github.com/padogrid/padogrid/wiki/Redis-perf_test-App

