# Hydrauliques Custom Configuration

## Customer Information

**Customer name​:** Hydrauliques

**Customer ID​ (Prod):** 14


## Description

Hydrauliques focuses on a subset of our standard (base) AI-detected items, to which an additional length parameter is calculated. To support this, we use both filtering and expansion mechanisms implemented via the ai-module-company-item relationship.

In this configuration:

- Items are filtered from the AI results using the OMIT action in the ai-module-company-item object.

- Items are expanded using the EXPAND action, which creates a copy of the original item on which the length calculation is performed.

## Related Objects

- *ai-module-company*: A configuration object that defines the relationship between an AI module and a company. This relationship enables specific modules for a given company.

- *ai-module-company-item*: A configuration object that maps AI modules to company-specific items, with actions such as OMMIT and EXPAND.



## Parameter Overview

| Parameter  | Type    | Value | Related Object     | Description                             |
|------------|---------|-------|---------------------|-----------------------------------------|
| hvac_type  | boolean | 1     | Frontend            | Enables HVAC detection in AI-module     |
| hyd_type   | boolean | 1     | Frontend            | Enables HYD filtering and expansion in job requests    |
| hyd        | boolean | 1     | Ai-module-company   | Enables HYD module for company          |


## Feature Flags / Modules​

The expected behavior is determined by the parameters listed above:

1. *hvac_type* must be selected in the UI to trigger the HVAC AI module and detect related items.
2. *hyd* must be enabled via the ai-module-company relationship, indicating that the company uses the HYD functionality (filtering and expansion).
3. *hyd_type* must be selected in the UI to activate filtering and expansion in the backend, based on ai-module-company-item.


## Behavioral Notes​

- The user requesting the job must be associated with this company (ID 14) in the production environment. If the association does not exist, Hydrauliques-specific functionality will not be executed.

- If the user is linked to multiple companies, filtering may not be correctly applied.


If not variables are set, we can encounter the following behaviour:

1. If hvac_type is not selected when submittinga job, no items will be returned by the ai-module. We'll get an empty resultset.
2. if hyd_type is not selected when submitting a job, no filtering or expansion will be applied to the final results.


## Filtering and Expansion Logic
Each relevant item must have two entries in the *ai-module-company-item* configuration:

- One with action OMIT: Filters the item from the final frontend results.

- One with action EXPAND: Creates a copy of the original item, suffixed with _HYD, on which the length calculation will be performed.



## Conditional logic​

When hyd_type is enabled and *ai-module-company-item* entries exist, the system will:

- Filter specified items using the OMMIT action.

- Expand items using the EXPAND action, appending _HYD to the item ID and calculating the length.

- Return the final results to the user via the frontend accordingly.

