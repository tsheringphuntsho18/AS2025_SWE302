# Practical_03 Report: Specification-Based Testing in Go

## Overview

This practical demonstrates specification-based testing in Go for a shipping fee calculator. It covers both the original and updated business rules for calculating shipping fees, using black-box testing techniques such as Equivalence Partitioning and Boundary Value Analysis.

**Full working code Repository:** [https://github.com/tsheringphuntsho18/shippingFee](https://github.com/tsheringphuntsho18/shippingFee)

## Test Design for `shipping_v2.go`

### 1. Equivalence Partitioning (with Rationale)

**Why?**
Equivalence partitioning is used to reduce the number of test cases by dividing input data into partitions of equivalent data from which test cases can be derived. The idea is that if one test case in a partition passes, all others will as well (assuming the implementation is correct).

**How the partitions were identified:**

- **Weight (`float64`)**

  - _Invalid partition_: Any weight ≤ 0 (should always error, as per spec)
  - _Valid partition_: Any weight in (0, 50] (should be accepted)
  - _Invalid partition_: Any weight > 50 (should always error)
  - _Heavy surcharge partition_: Any weight > 10 (triggers a surcharge)

- **Zone (`string`)**

  - _Valid partition_: "Domestic", "International", "Express" (the only accepted values)
  - _Invalid partition_: Any other string (should always error)

- **Insured (`bool`)**
  - _true_: Insurance cost should be added
  - _false_: No insurance cost

**Why these partitions?**
Each partition represents a set of inputs that should be handled identically by the function. For example, all invalid weights (≤ 0 or > 50) should result in an error, regardless of zone or insurance. All valid weights with a valid zone should compute a fee, with the insurance flag determining if an extra cost is added.

### 2. Boundary Value Analysis (with Rationale)

**Why?**
Most bugs occur at the edges of input domains. Boundary value analysis ensures that the function behaves correctly at, just below, and just above the boundaries of valid input ranges.

**How the boundaries were identified:**

- **Weight**

  - _Lower boundary_: 0 (invalid), 0.01 (just valid)
  - _Heavy surcharge threshold_: 10 (no surcharge), 10.01 (surcharge applies)
  - _Upper boundary_: 50 (valid), 50.01 (invalid)

- **Zone**

  - No numeric boundaries, but all valid strings and at least one invalid string are tested to ensure correct error handling.

- **Insured**
  - Boolean, so both true and false are always tested for each relevant case.

**Why these boundaries?**
Testing at and around these values ensures the function does not have off-by-one or similar errors, and that surcharges and errors are triggered at the correct points.

### 3. Test Implementation

All partitions and boundaries are covered in a single table-driven test in `shipping_v2_test.go`. Each test case specifies:

- A descriptive name
- Inputs: weight, zone, insured
- Expected fee (if valid)
- Whether an error is expected

## How to Run

1. Ensure you have Go installed.
2. In the project directory, run:
   ```bash
   go test -v
   ```
   All tests should pass.

![test](/assets/test1.png)  
![test](/assets/test2.png)