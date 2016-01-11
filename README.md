[![Build Status](https://travis-ci.org/funny/slab.svg)](https://travis-ci.org/funny/slab)
[![Coverage Status](https://coveralls.io/repos/funny/slab/badge.svg?branch=master&service=github)](https://coveralls.io/github/funny/slab?branch=master)

Introduction
============

Slab allocation memory pools for Go.

Usage
=====

Use lock-free memory pool:
```go
pool := slab.NewLockFreePool(
	64,          // The smallest chunk size is 64B.
	64 * 1024,   // The largest chunk size is 64KB.
	2,           // Power of 2 growth in chunk size.
	1024 * 1024, // Each slab will be 1MB in size.
)

buf1 := pool.Alloc(64)

    ... use the buf ...
	
pool.Free(buf)
```

Use `sync.Pool` based memory pool:
```go
pool := slab.NewSyncPool(
	64,          // The smallest chunk size is 64B.
	64 * 1024,   // The largest chunk size is 64KB.
	2,           // Power of 2 growth in chunk size.
)

buf := pool.Alloc(64)

    ... use the buf ...
	
pool.Free(buf)
```

Performance
===========

When `GOMAXPROCS=16`:

```
Benchmark_LockFree_AllocAndFree_128-4	10000000	       150 ns/op	       0 B/op	       0 allocs/op
Benchmark_LockFree_AllocAndFree_256-4	10000000	       181 ns/op	       0 B/op	       0 allocs/op
Benchmark_LockFree_AllocAndFree_512-4	10000000	       181 ns/op	       0 B/op	       0 allocs/op

Benchmark_SyncPool_AllocAndFree_128-4	20000000	        65.7 ns/op	      32 B/op	       1 allocs/op
Benchmark_SyncPool_AllocAndFree_256-4	20000000	        67.9 ns/op	      32 B/op	       1 allocs/op
Benchmark_SyncPool_AllocAndFree_512-4	20000000	        69.7 ns/op	      32 B/op	       1 allocs/op

Benchmark_Make_128-4                 	30000000	        45.4 ns/op	     128 B/op	       1 allocs/op
Benchmark_Make_256-4                 	20000000	        71.9 ns/op	     256 B/op	       1 allocs/op
Benchmark_Make_512-4                 	20000000	       102 ns/op	     512 B/op	       1 allocs/op
```
