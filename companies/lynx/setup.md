# Lynx Custom Configuration

## Customer Information

**Customer name​:** Lynx

**Customer ID​ (Prod):** 11


## Description

Lynx operates on a subset of our standard (base) AI-detected items. To implement this, we use a filtering mechanism via the ai-module-company-item relationship.

- Specifies items that should be filtered out from the AI results.

- Uses the OMMIT action in the ai-module-company-item object to exclude these items from the final output.



## Related Objects

- *ai-module-company*: A configuration object that defines the relationship between an AI module and a company. This relationship enables specific modules for a given company.

- *ai-module-company-item*: A configuration object that maps AI modules to company-specific items, using actions such as OMIT to exclude results.

## Parameter Overview

| Parameter  | Type    | Value | Related Object     | Description                             |
|------------|---------|-------|---------------------|-----------------------------------------|
| hvac_type  | boolean | 1     | Frontend            | Enables HVAC detection in AI-module     |
| rdm_type   | boolean | 1     | Frontend            | Enables RDM filtering in job requests    |
| rdm        | boolean | 1     | Ai-module-company   | Enables RDM module for company          |


## Feature Flags / Modules​

The expected behavior is achieved by using the parameters above as follows:

1. *hvac_type* must be selected in the UI to trigger execution of the HVAC AI module and detect relevant items.
2. *rdm* must be enabled in the ai-module-company relationship, indicating that the company uses the RDM functionality (i.e., filtering). 
3. *rdm_type* must be selected in the UI to activate backend filtering for the items defined in ai-module-company-item.


## Behavioral Notes​

- The user requesting the job must be associated with this company (ID 11) in production. Without this association, Lynx-specific functionality will not be executed.

- If the user is associated with multiple companies, filtering might not be correctly applied.

## Default Behavior (When parameters are not set):

1. If hvac_type is not selected when submitting a job, the AI module will return no items (empty result set).

2. If rdm_type is not selected, no filtering will be applied to the results.


## Filtering Logic

For each relevant item, there must be one entry in the ai-module-company-item configuration with the OMMIT action:

- OMMIT: Filters the item from the final frontend results.



## Conditional logic​

When rdm_type is enabled and ai-module-company-item entries exist, the system will:

- Apply filtering using the OMMIT action.

- Return the filtered results to the user via the frontend.

