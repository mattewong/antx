# xtna
Cash flow engine library for structured finance assets and capital structures

# Objectives (current)

* MIT for this library, MIT or Apache or similarly permissive license for any libraries this relies on

* Calculation of loan-level and pool-level collateral cashflows for common abs asset classes including residential mortgages, auto loans, student loans, credit card loans, etc

* Support for common "curves" (e.g. CPR, CDR, ABS, etc)

* Support for per-period, function-based curves

* API for capital structures applied to pool-level cashflows

* High performance
Streamed output (bounded memory)
Lightweight (memory as well as binary size)
Compatible with distributed lambda architectures

* Distributable / parallelizable across unlimited number of nodes

* Cross-platform: Windows, Linux, Mac

# Objectives (future)
The following longer-term objectives are not currently reflected in the library. Suggestions / comments / pull requests that help to make these future changes easier to achieve are much appreciated

* Arbitrary precision
* OpenCL

# What this is not

Predictive: this API will not predict asset performance. Rather, it will model cashflows based on the provided assumptions of future asset performance

# Usage
