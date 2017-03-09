# antx
Cash flow engine API for structured finance assets and capital structures

# Objectives

* REST+JSON stateless API for loan-level and pool-level collateral cashflows for common abs asset classes including residential mortgages, auto loans, student loans, credit card loans, etc

* Support for common "curves" (e.g. CPR, CDR, ABS, etc)

* Support for per-period, function-based curves

* API for capital structures applied to pool-level cashflows

* High performance per node

* Distributable / parallelizable across unlimited number of nodes

# What this is not

Predictive: this API will not predict asset performance. Rather, it will model cashflows based on the provided assumptions of future asset performance
