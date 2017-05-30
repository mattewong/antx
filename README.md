# sfcf (Structured finance cash flow)
Cash flow engine library for structured finance assets and capital structures

# Objectives (current)

* Coverage of common abs asset classes including residential mortgages, auto loans, student loans, credit card loans, etc
* Coverage of all functionality required to tie out information typically included in securities offering documents e.g. common "curves" (e.g. CPR, CDR, ABS, etc) and structures
* Function-based approach supporting user-defined functions
* High performance
* API for capital structures applied to pool-level cashflows
  * Streamed output (bounded memory)
  * Lightweight (memory as well as binary size)
  * Compatible with distributed "lambda" architectures such as 

* Distributable / parallelizable across unlimited number of nodes

* Cross-platform: Windows, Linux, Mac

# License
* You may use this library for educational or academic use (which of course is non-commercial) under the MPL 2.0 license
* For commercial use: contact the project team

# Objectives (future)
The following longer-term objectives are not currently reflected in the library. Suggestions / comments / pull requests that help to make these future changes easier to achieve are much appreciated

* Arbitrary precision
* OpenCL or similar GPU / HPC for higher performance

# What this is not

Predictive: this API will not predict asset performance. Rather, it will model cashflows based on the provided assumptions of future asset performance
