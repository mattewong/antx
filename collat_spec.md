This document describes the design specification for the collateral cashflow component. Notation used herein is generally either JSON (for data structures), Haskell (for function signatures), or Javascript (for function implementation)

## Input
Inputs should include:
* asset-level data
* callback to provide scenario information such as interest rates
* callbacks to calculate outputs, OR, predefined callbacks (e.g. "CPR"), together with requisite input information (e.g. curves)

## Output
Outputs should include, for each provided scenario:
* loan-level cash flow results? Yes. 
* aggregated cash flow results? Yes, at least to some degree
* support for irregular intervals? No

### Each cashflow result is a time series of a cash flow tuple
Elements of the tuple may include:
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
  transformation: { output: structure, functions: function },
  aggregation: { input: structure, output: structure, functions: [ function ] }
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
* transformation generates a cashflow tuple related to a single asset for a single time period
A transformation may include sub-transformations, which is the common approach whereby sub-transformations include e.g.
prepayment, default and other functions. See below for an example

#### Output
Outputs are a series of cash flow results. At the highest level:
```
output = [ cashflow_result ]

# cashflow_result is a time series of a cash flow tuple
cashflow_result = [ cashflow_tuple ] 

# cashflow_tuple = output for a single asset for a single period
cashflow_tuple = ( beginning_balance, principal_payment, ... )

# aggregation = time series of aggregated output for a group of assets
```

#### Transformations
Transformations are what take us from asset-level input to asset-level output.
Each transformation function outputs a single output structure of the form specified by ```transformation.output``` and takes three inputs:
* a single asset in the form specified by ```assets.input```
* a integer indicating the current period
* the scenario data

#### Aggregations
Aggregations are what take us from asset-level output to group-level output.
aggregation.functions[n] 

## Examples
### Common curve transformations
A common approach to cash flow modeling is to use curves such as CPR, CDR, Severity etc. Let's take an example with CPR, CDR and severity. We will create one function for each. Since CPR and CDR are an annualized values for SMM, and our functions are per-period functions, we will assume our curves have been already converted to SMM.
```
// prepayment_by_smm :: asset -> curve -> [ element ]
function prepayment_by_smm(asset, period, scenario) {
  var smm_pct = scenario.cpr_smm[period];
  // return prepay_prin
  return [ smm_pct * (asset.beg_bal - asset.sched_prin) ];
}

// default_by_smm :: asset -> curve -> [ element ]
function default_by_smm(asset, period, scenario) {
  var smm_pct = scenario.cdr_smm[period];
  // return default_prin
  return smm_pct * (asset.beg_bal - asset.sched_prin - asset.prepay_prin);
}

// loss_by_severity :: asset -> curve -> [ element ]
function loss_by_severity(asset, period, scenario) {
  // for this simple example, ignore the time lag that would occur between default and loss
  var loss_pct = scenario.severity[period];
  // return loss_prin
  return loss_pct * asset.default_prin;
}

// curve_based_transformation :: curve_function -> curve_function -> curve_function -> transformation
function curve_transformation(prepayment_func, default_func, loss_func) {
  return function(asset, period, scenario) {
    asset.prepay_prin  = prepayment_func(asset, period, scenario);
    asset.default_prin = default_func(asset, period, scenario);
    asset.loss_prin    = loss_func(asset, period, scenario);
    return asset;
  }
}
```
