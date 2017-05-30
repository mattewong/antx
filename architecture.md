# Collateral cash flow

## Input
Inputs should include:
* asset-level data
* callback to provide scenario information such as interest rates
* callbacks to calculate outputs, OR, predefined callbacks (e.g. "CPR"), together with requisite input information (e.g. curves)

## Output
Outputs should include, for each provided scenario:
* loan-level cash flow results? Yes. 
* aggregated cash flow results? TBD
* support for irregular intervals? No

### Each cashflow result is a time series of a cash flow tuple. Elements of the tuple may include:
* Beginning balance
* Principal payment
  * Scheduled
  * Received
* Principal write-down
* Interest payment
  * Scheduled
  * Received
* Interest shortfall
* Principal recovery
* etc

### Specifications and definitions
#### Inputs
```
{
  assets: { input: structure, data: [ asset ] },
  scenarios: [ scenario ],
  transformation: { output: structure, functions: [ function ] }
}
```
where each
* structure defines input or output data as a series of elements e.g. ``` { elements: [ element ] } ```. 
  * element is of the form ```{ name: element_name }```. For example, "Beginning balance" and "First Payment Date" would each be elements.
* asset is a list of characteristic values corresponding to the given columns
(Note that it is possible for an element to be a time-series, expressed as a non-scalar (e.g. array),
such as one might implement for pay history)
* scenario contains arbitrary information that is made available to the functions. 
For example, scenario information may include curves for interest rates, prepayments, defaults etc.
The nature of the information contained in a scenario is not predefined, but rather, is dictated by whatever is required by the transformation functions.
* transformation generates a cashflow tuple related to a single asset for a single time period. 
Multiple transformations may be applied in sequence, but the end result will remain a single cashflow tuple.
Transformations may include sub-transformations, which is the common approach whereby sub-transformations include e.g.
prepayment, default and other functions

#### Output
Outputs are a series of cash flow results. At the highest level:
```
output = [ cashflow_result ]

# cashflow_result is a time series of a cash flow tuple
cashflow_result = [ cashflow_tuple ] 

# cashflow_tuple = output for a single asset for a single period
cashflow_tuple = ( beginning_balance, principal_payment, ... )
```

#### Transformations
Transformations are what take us from input to output.
Each transformation function outputs a single output structure of the form specified by ```transformation.output``` and takes three inputs:
* a single asset in the form specified by ```assets.input```
* a integer indicating the current period
* the scenario data

